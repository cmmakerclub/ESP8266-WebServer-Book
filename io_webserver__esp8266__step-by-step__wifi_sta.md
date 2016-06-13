# WebServer ควบคุม I/O ในโหมด WIFI_STA

บทความนี้จะพูดถึงการทำให้ ESP8266 เป็น WebServer ที่แสดงผล html(webpage) และสาธิตด้วยการควบคุม I/O (input/output) 
เอาล่ะครับ งั้นเริ่มกันเลยเนาะ.. ไปกันแบบ step-by-step นะครับ

อันดับแรก เรามาเริ่มต้นกันที่ Example Code กันเลยครับ ตัวอย่าง > ESP8266Server > HelloServer

```
#include <ESP8266WiFi.h>
#include <WiFiClient.h>
#include <ESP8266WebServer.h>

const char* ssid = "........";
const char* password = "........";

ESP8266WebServer server(80);

const int led = LED_BUILTINat;

void handleRoot() {
  digitalWrite(led, 1);
  server.send(200, "text/plain", "hello from esp8266!");
  digitalWrite(led, 0);
}

void init_webserver();

void handleNotFound(){

}

void setup(void){
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

void loop(void){
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

  server.on("/inline", [](){
    server.send(200, "text/plain", "this works as well");
  });

  server.on( "/millis", []() {
    char buff[100];
    sprintf(buff, "{\"millis\": %lu }", millis());
    server.send ( 200, "text/plain", buff );
  });  

  server.onNotFound([]() {
    String message = "File Not Found\n\n";
    message += "URI: ";
    message += server.uri();
    server.send(404, "text/plain", message);
  });

  server.begin();
  Serial.println("HTTP server started");
}
```