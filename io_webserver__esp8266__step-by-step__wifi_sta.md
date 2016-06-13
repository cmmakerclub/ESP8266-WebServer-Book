# WebServer ควบคุม I/O ในโหมด WIFI_STA

บทความนี้จะพูดถึงการทำให้ ESP8266 เป็น WebServer ที่แสดงผล html(webpage) และการควบคุม I/O (input/output)
เอาล่ะครับ งั้นเริ่มกันเลยเนาะ.. ไปกันแบบ step-by-step นะ

1. blah blah
2. หลังจากที่เรา initialze พวก IO ทั้งหลายแล้ว เปิด Serial port แล้ว เราก็มาเตรียม config  url handler บนตัว web server กันครับ code จะเป็นแบบนี้นะ
```
  void init_webserver() {
    server.on ( "/", []() {
      bool state = digitalRead(LED_BUILTIN);
      digitalWrite(LED_BUILTIN, !state);
      char buff[100];
      String ms = String(millis());
      sprintf(buff, "{\"millis\": %s }", ms.c_str());
      server.send ( 200, "text/plain", buff );
    });

    server.on ( "/millis", []() {
      char buff[100];
      String ms = String(millis());
      sprintf(buff, "{\"millis\": %s }", ms.c_str());
      server.send ( 200, "text/plain", buff );
    });
  }
```
ซึ่งโค้ดข้างบนจะ handle ได้ 2 url คือ


พาธ`/millis` และ `/` (root path)
ซึ่งทั้ง 2 path ที่กล่าวถึงข้างบนนั้น ทำหน้าที่คล้ายๆกันคือ และ string หน้าตาแบบ JSON และ http response (200 OK)

root path “/” นั้นพิเศษขึ้นมาหน่อยคือ จะมี code ทำหน้าที่ Toggle LED_BUILTIN อยู่ด้วยครับ ก็แค่ digitalWrite ธรรมดานั่นแหละ


