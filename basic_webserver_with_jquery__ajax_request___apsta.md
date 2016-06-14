# Basic WebServer with JQuery และ AJAX request ใน โหมด AP_STA



หลังจากที่เราต่อ ESP8266 ของเรากับ WiFi ใน STA (client) Mode แล้ว เรามาใส่ jQuery เข้าไปใน โปรเจ็คของเรากันเถอะครับ

วิธีการจะง่ายมากครับ เราสามารถใช้ code เดิมได้เลย เพียงแต่เพิ่ม

แท็ก `<script>` ที่ทำหน้าที่โหลด jquery เข้ามาใน browser เท่านั้น! 
หน้าตาก็จะออกมาประมาณนี้ครับ (ผู้เขียนเลือกโหลดมาจาก google cdn นั่นเองครับ)
`<script src="https://ajax.googleapis.com/ajax/libs/jquery/2.2.2/jquery.min.js"></script>`

มาปรับแก้โค้ดแล้ว จะได้โค๊ดเพิ่มขึ้นมา 1 บรรทัด หลังจากนั้นเราสามารถ บรรเลงบทเพลงทำนอง jQuery ได้เลยครับ แบบนี้นะ..

```
  server.on("/", []() {
    digitalWrite(led, !digitalRead(led));
    static String responseHTML = F(""
      "<!doctype html>                             "
      "<html>                                      "
      "  <head>                                    "
      "<script src=\"https://ajax.googleapis.com/ajax/libs/jquery/2.2.2/jquery.min.js\"></script>                                          "
      "    <title>Hello CMMC</title>               "
      "  </head>                                   "
      "  <body>                                    "
      "      <h1>HELLO WORLD                       " 
                <span id=\"millis\"></span>        "
      "       </h1>                                "
      "  </body>                                   "
      "</html>");
      server.send (200, "text/html", responseHTML.c_str() );
  });
```
ดูกันเต็มๆ..

{% gist id="https://gist.github.com/NAzT/b27abfcfce3a62fb1bfbe35f6dbf96b3" %}{% endgist %}
 
จากตัวอย่างข้างบนรอบนี้ เราจะต้องมาทำ Ajax Request กันครับ ผมจะทดสอบโดยการทำ web page แสดงค่า อุณหภูมิ และความชื้นนะครับ.. เอาล่ะเริ่มกันได้เลย
```
  <script type="text/javascript">
    var updateUI = function(res) {
      $("#temperature").text("temp");
      $("#humidity").text("humid");
    }
    var getData = function() {
      $.getJSON("/data", updateUI);
    }
    
    setInterval(1000, getData);
  </script>
```