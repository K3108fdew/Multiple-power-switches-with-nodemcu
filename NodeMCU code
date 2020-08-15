#include <ESP8266WiFi.h>
#include <ESP8266HTTPClient.h>
#include <WiFiClient.h>
#include <ESP8266WebServer.h>
#include <ESP8266mDNS.h>

MDNSResponder mdns;

const char* ssid = "bnet";
const char* password = "marteknutbo12";
 
ESP8266WebServer server(80);
String nettside = "";
int ledPin = 13;
int Knapp = 14;
int last = 1;
const int operasjoner[] = {2, 3};
const int Av = 1;

void Toggle(){
  digitalWrite(ledPin,LOW);
  delay(500);
  digitalWrite(ledPin,HIGH);
  delay(100);
  digitalWrite(ledPin,LOW);
  delay(500);
  digitalWrite(ledPin,HIGH);
}
void setup(){
  
  Serial.begin(115200);
  delay(10);
 
  pinMode(ledPin, OUTPUT);
  digitalWrite(ledPin, LOW);
  pinMode(Knapp, INPUT);
 
  // Connect to WiFi network
  Serial.println();
  Serial.println();
  Serial.print("Connecting to ");
  Serial.println(ssid);
  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");
  Serial.println("WiFi connected");

  if (mdns.begin("esp8266", WiFi.localIP())) {
   Serial.println("MDNS responder started");
  }
    server.on("/", [](){
      server.send(200, "text/html", nettside);
    });
    server.on("/socket1On", [](){
      server.send(200, "text/html", nettside);
    });
    server.on("/socket1Off", [](){
      server.send(200, "text/html", nettside); 
    });
    server.on("/socket2On", [](){
      server.send(200, "text/html", nettside);
    });
    server.on("/socket2Off", [](){
      server.send(200, "text/html", nettside);
    });
    server.begin();
    Serial.println("HTTP server started");

  Serial.print("Use this URL to connect: ");
  Serial.print("http://");
  Serial.print(WiFi.localIP());
  Serial.println("/");
    
}
void loop(){
  bool Buttonstate = digitalRead(Knapp);
  if(Buttonstate && last == 1){
    Buttonstate = 0;
    Serial.println(33);
    last += 1;
    Toggle();
  }
  if(Buttonstate && last == 2){
    Serial.println(Buttonstate);
    Buttonstate = 0;
    last = 1;
    digitalWrite(ledPin, LOW);
  }
  
  
  server.handleClient();
  if(WiFi.status() != WL_CONNECTED)
  {
    digitalWrite(13, LOW);
    Serial.println("");
    Serial.print("Wifi is disconnected!");
    Serial.println("");
    Serial.print("Reconnecting");
    Serial.println("");
    //WiFi.begin(ssid,password);
  }
  if (server.hasArg("Modus")){
    int Status = server.arg("Modus").toInt();
    bool Mygg = std::find(std::begin(operasjoner), std::end(operasjoner), Status) != std::end(operasjoner);
    if(last != Status && Mygg){
      Toggle();
      Serial.println(Status);
      last = Status;
    }
    if(Status == 1){
      last = 1;
      digitalWrite(ledPin,LOW);
    }
  }
  delay(1000);
}
