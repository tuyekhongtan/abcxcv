 #include<SPI.h>
 #include<Ethernet.h>
 #define ChanA0 0
 #define ChanA1 1
 #define ChanA2 2
 #define ChanA3 3
 #define ChanA4 4 
 #define ChanA5 5
 float giatriA0, giatriA1, giatriA2, giatriA3, giatriA4, giatriA5;
 int led = 9;
 byte mac[]={
   0xDE,0xAD,0xBE,0xEF,0xFE,0xED};
  IPAddress ip(192,168,2,196);
  IPAddress gateway(192,168,2,1);
  IPAddress subnet(255,255,255,0);
  EthernetServer server(80);
  String readString;
 void setup(){
   Serial.begin(9600);
   while(!Serial){
     ;
   }
   pinMode(led,OUTPUT);
  Ethernet.begin(mac,ip);
  server.begin();
 Serial.print("Dia chi server la");
Serial.println(Ethernet.localIP());
 }
 void loop()
 {
   EthernetClient client=server.available();
   giatriA0=analogRead(ChanA0);
   giatriA1=analogRead(ChanA1);
   giatriA2=analogRead(ChanA2);
   giatriA3=analogRead(ChanA3);
   giatriA4=analogRead(ChanA4);
   giatriA5=analogRead(ChanA5);
   if(client){
     while(client.connected())
     {
       if(client.available())
       {
         char c=client.read();
         
         if(readString.length()<100){
           
           readString+=c;
           
         }
         
         if(c=='\n')
         {
           Serial.println(readString);
           client.println("HTTP/1.1 200 OK");
           client.println("Content-Type: text/html");
           client.println("Refresh: 5");
           client.println();
           client.println("<HTML>");
           client.println("<HEAD>");
           client.println("<TITLE>DIEU KHIEN MACH ARDUINO QUA INTERNET</TITLE>");
           client.println("</HEAD>");
           client.println("<BODY>");
           client.println("<CENTER>");
           client.println("<H1>DONG MO CONG TAC QUA INTERNET</H1>");
           client.println("<br/>");
           client.println("<a href=\"/?button1on\"\">BAT</a>");
           client.println("<br/>");
           client.println("<br/>");
           client.println("<a href=\"/?button1off\"\">TAT</a>");
           client.println("<br/>");
           client.println("<br/>");
           client.println("<p>GIA TRI CAM BIEN DAU VAO</p>");
           client.println("<br/>");
           client.print("GIA TRI CHAN A0 LA: ");
           client.print(giatriA0);
           client.println("<br/>");
           client.print("GIA TRI CHAN A1 LA: ");
           client.print(giatriA1);
           client.println("<br/>");
           client.print("GIA TRI CHAN A2 LA: ");
           client.print(giatriA2);
           client.println("<br/>");
           client.print("GIA TRI CHAN A3 LA: ");
           client.print(giatriA3);
           client.println("<br/>");
           client.print("GIA TRI CHAN A4 LA: ");
           client.print(giatriA4);
           client.println("<br/>");
           client.print("GIA TRI CHAN A5 LA: ");
           client.print(giatriA5);
           client.println("<br/>");
           client.println("<p></p>");
           client.println("<br/>");
           client.println("</BODY>");
           client.println("</HTML>");
           delay(1);
           
           client.stop();
           Serial.println("client disconnected");
           if(readString.indexOf("?button1on")>0)
           {
             digitalWrite(led,HIGH);
           }
           if(readString.indexOf("?button1off")>0)
           {
             digitalWrite(led,LOW);
           }
           readString="";
           }
       }
     }
   }
 }
