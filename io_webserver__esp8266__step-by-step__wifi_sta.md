# WebServer ควบคุม I/O ในโหมด WIFI_STA

แต่ว่าาา จะสร้าง WebServer บน MCU อย่าง ESP8266 ทั้งที.. จะพลาดการร่วมมือกับ IO ได้อย่างไรล่ะเนอะ.. ขอ Toggle LED ซักหน่อยน่าาา

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
