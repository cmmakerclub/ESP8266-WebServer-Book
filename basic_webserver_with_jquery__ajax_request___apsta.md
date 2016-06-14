# Basic WebServer with JQuery และ AJAX request ใน โหมด AP_STA



หลังจากที่เราต่อ ESP8266 ของเรากับ WiFi ใน STA (client) Mode แล้ว เรามาใส่ jQuery เข้าไปใน โปรเจ็คของเรากันเถอะครับ

วิธีการจะง่ายมากครับ เราสามารถใช้ code เดิมได้เลย เพียงแต่เพิ่ม

แท็ก `<script>` ที่ทำหน้าที่โหลด jquery เข้ามาใน browser เท่านั้น! 
หน้าตาก็จะออกมาประมาณนี้ครับ (ผู้เขียนเลือกโหลดมาจาก google cdn นั่นเองครับ)
`<script src="https://ajax.googleapis.com/ajax/libs/jquery/2.2.2/jquery.min.js"></script>`

มาปรับแก้โค้ดแล้ว จะได้โค๊ดเพิ่มขึ้นมา 1 บรรทัด หลังจากนั้นเราสามารถ บรรเลงบทเพลงทำนอง jQuery ได้เลยครับ!!


{% gist id="https://gist.github.com/NAzT/b27abfcfce3a62fb1bfbe35f6dbf96b3" %}{% endgist %}
 
ทีนี้ Ajax Request ผมจะทดสอบให้ทำ web page แสดงค่า อุณหภูมิ และความชื้นนะครับ ก็จะต้องมาเขียน ajax request กันหน่อย
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