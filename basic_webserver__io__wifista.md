# การสร้าง WebServer เบื้องต้น และการควบคุม I/O บนบอร์ด

บทความนี้จะพูดถึงการทำให้ ESP8266 เป็น WebServer ที่แสดงผล html(webpage) และสาธิตด้วยการควบคุม I/O (input/output) 
เอาล่ะครับ งั้นเริ่มกันเลยเนาะ.. ไปกันแบบ step-by-step นะครับ

อันดับแรก เรามาสร้าง WebServer ในเทคนิคการต่อ string แบบต่างๆกันครับ สามารถดูโค้ดด้านล่างได้เลย  หากไม่เข้าใจวันนี้ก็ข้ามไปก่อนได้นะครับ

อธิบาย step ง่ายๆ ดังนี้

1. Initial I/O ต่างๆ ซึ่งในที่นี้คือ led Pin
2. เชื่อมต่อ ESP8266 เข้ากับ WiFi Access Point (เป็น WIFI_STA mode และสั่งด้วยคำสั่ง WiFi.begin)
3. ตั้งค่า(Config) และสั่ง initial WebServer บน ESP8266
4. handle WebServer Request ใน loop()

โค้ดหน้าตาเป็นแบบนี้ครับ

```
#include <ESP8266WiFi.h>
#include <WiFiClient.h>
#include <ESP8266WebServer.h>

const char* ssid = "ESPERT-002";
const char* password = "espertap";
const int led = LED_BUILTIN;

ESP8266WebServer server(80);

void init_webserver();
void handleDebug();

void setup(void) {
  pinMode(led, OUTPUT);
  digitalWrite(led, 0);
  Serial.begin(115200);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    Serial.printf("Connecting to %s:%s\r\n", ssid, password);
    delay(500);
  }

  Serial.printf("Connected to %s, IP address: ", ssid);
  Serial.println(WiFi.localIP());
  init_webserver();
}

void loop(void) {
  server.handleClient();
}

void init_webserver() {
  server.on("/", []() {
    digitalWrite(led, !digitalRead(led));
    static String responseHTML = F(""
     "<!doctype html>               "
     "<html>                        "
     "  <head>                      "
     "    <title>Hello CMMC</title> "
     "  </head>                     "
     "  <body>                      "
     "      <h1>HELLO WORLD</h1>    "
     "  </body>                     "
     "</html>");
    server.send (200, "text/html", responseHTML.c_str() );
  });

  server.on("/inline", []() {
    server.send(200, "text/plain", "this works as well");
  });

  server.on( "/millis", HTTP_GET, []() {
    char buff[100];
    sprintf(buff, "{\"millis\": %lu }", millis());
    server.send (200, "text/plain", buff );
  });

  server.on("/debug", HTTP_GET, handleDebug);


  server.onNotFound([]() {
    String message = "File Not Found\n\n";
    message += "URI: ";
    message += server.uri();
    server.send(404, "text/plain", message);
  });

  server.begin();
  Serial.println("HTTP server started");
}

void handleDebug()  {
  String message = "";
  message += "URI: ";
  message += server.uri();
  message += "\nMethod: ";
  message += (server.method() == HTTP_GET) ? "GET" : "POST";
  message += "\nArguments: ";
  message += server.args(); 
  message += "\n";
  for (uint8_t i = 0; i < server.args(); i++) {
    message += " " + server.argName(i) + ": " + server.arg(i) + "\n";
  }
  
  Serial.printf("hasArg(\"test\") value = %s \r\n", server.arg("test").c_str());
  if (server.hasArg("status")) {
      if (server.arg("status") == "0") {
        digitalWrite(led, LOW);
      }
      else {
        digitalWrite(led, HIGH);
      }
  }

   
  
  server.send(404, "text/plain", message);
}
```
