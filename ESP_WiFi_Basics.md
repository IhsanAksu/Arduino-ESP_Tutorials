***Configuration:***

*Include Libraries:*
```C
#include <ESP8266WiFi.h>
#include <ESP8266WebServer.h>
```
*ESP8266 WiFi Configuration:*
```C
const char* ssid = "*******";
const char* password = "*******";
```

---

***Program Commands:***

*Connect to WiFi:*
```C
//Connect to router
WiFi.begin(ssid, password);  //WiFi Begin
WiFi.status()                //WiFi status (returns WL_CONNECTED?)
WiFi.localIP()               //local IP

//Local WiFi
WiFi.softAP(ssid, password);   //WiFi Begin
WiFi.softAPIP();               //IP Address
```
---
*Web Server:*

html/css content example
```C
const char* htmlContent = R"rawliteral(
<!DOCTYPE html>
<html>
    <head>
        <title>WebServer</title>
        <link rel="stylesheet" href="./style.css">
    </head>
    <body>
        <p id="text">IhsanAksu</p>
        <script src="./javas.js"></script> 
    </body>
</html>
)rawliteral";

const char* cssContent = R"rawliteral(
.text{
color: white;
}
)rawliteral";
```
*A little note:*
```html
<link rel="stylesheet" href="./style.css">
<script src="./java.js"></script> 
```
Server will send a request for "java.js" and "style.css" to the ESP module

---

Send commands
```C
server.send(200,"text/html","htmlContent");               //Send to server a text as a html content

server.send(200,"text/css","CSSContent");                 //Send to server a text as a CSS Code

server.send(200,"text/javascript","JavaScriptContent");   //Send to server a text as a JavaScript Code

server.send(200,"text/plain","htmlContent");              //Send to server a text content
```



On request
```C
server.on("/", HTTP_GET, myfunction()); //on "/" request, start to myfunction()
```

---

***Get Data From Server:***

*Configure the XMLHttpRequest():*

```js
var xhr = new XMLHttpRequest();
var message = "Hello World!";


xhr.open("GET","/req?text="+message,true);
xhr.send();

```
Code will send a "/req" request to the ESP module with text="Hello World" value

---

*Get value on ESP module:*

```C
server.on("/req", HTTP_GET, []{
if(server.hasArg("text"))            //if server has arg("text")
String tx = server.arg(text);        //Get text value on "/req" request and print the text value
Serial.println(tx);                  //Print out tx with serial communication
});
```

