#include <SoftwareSerial.h>

SoftwareSerial espSerial =  SoftwareSerial(2,3);   
#include <DHT.h>       
#include <LiquidCrystal.h>
LiquidCrystal lcd(14,15,16,17,18,19);
#define DHTPIN 12               
#define DHTTYPE DHT11 
DHT dht(DHTPIN, DHTTYPE);

String apiKey = "UM71PM0QOBX00QEP";     

String ssid="Link 16p";    

String password ="123456*123";  

boolean DEBUG=true;

//======================================================================== 

void showResponse(int waitTime){

    long t=millis();

    char c;

    while (t+waitTime>millis()){

      if (espSerial.available()){

        c=espSerial.read();

        if (DEBUG) Serial.print(c);

      }

    }

                   

}

//========================================================================

boolean thingSpeakWrite(float value1, float value2){

  String cmd = "AT+CIPSTART=\"TCP\",\"";                  // TCP connection

  cmd += "184.106.153.149";                               // api.thingspeak.com

  cmd += "\",80";

  espSerial.println(cmd);

  if (DEBUG) Serial.println(cmd);

  if(espSerial.find("Error")){

    if (DEBUG) Serial.println("AT+CIPSTART error");

    return false;

  }

  String getStr = "GET /update?api_key=";   

  getStr += apiKey;

   getStr +="&field1=";

  getStr += String(value1);

  getStr +="&field2=";

  getStr += String(value2);

    getStr += "\r\n\r\n";

  // send data length

  cmd = "AT+CIPSEND=";

  cmd += String(getStr.length());

  espSerial.println(cmd);

  if (DEBUG)  Serial.println(cmd);

  

  delay(100);

  if(espSerial.find(">")){

    espSerial.print(getStr);

    if (DEBUG)  Serial.print(getStr);

  }

  else{

    espSerial.println("AT+CIPCLOSE");

 

    if (DEBUG)   Serial.println("AT+CIPCLOSE");

    return false;

  }

  return true;

}

//================================================================================ setup

void setup() {     

 lcd.begin(16, 2);

 lcd.clear();

 lcd.print("Humidity & Temp");

 lcd.setCursor(0,1);

 lcd.print(" Measurement");

 delay(2000);

 lcd.clear();

 //lcd.print("READY");

 //lcd.setCursor(0,1);

 lcd.print("HELLO");

 delay(2000);

 Serial.begin(9600);

 lcd.clear();

 lcd.print("WIFI Connecting");

 lcd.setCursor(0,1);

 lcd.print("Please wait....");

           

  DEBUG=true;           

  Serial.begin(9600); 

 
  dht.begin();         

  espSerial.begin(9600);  
  

  espSerial.println("AT+RST");         

  showResponse(1000);

  espSerial.println("AT+UART_CUR=9600,8,1,0,0");    

  showResponse(1000);

 
  espSerial.println("AT+CWMODE=1");   

  showResponse(1000);

  espSerial.println("AT+CWJAP=\""+ssid+"\",\""+password+"\"");  

  showResponse(5000);

 if (DEBUG)  Serial.println("Setup completed");

 delay(1500);

 lcd.clear();

 lcd.print("WIFI Connected.");

 delay(1000);

 lcd.setCursor(0,1);

 lcd.print("Setup comleted");

 delay(3000);

}


// ====================================================================== loop

void loop() {



  // Read sensor values

   float t = dht.readTemperature();

   float h = dht.readHumidity();

        if (isnan(t) || isnan(h)) {

        if (DEBUG) Serial.println("Failed to read from DHT");

      }

      else {

          if (DEBUG)  Serial.println("Temp="+String(t)+" *C");

           lcd.setCursor(0,0);

           lcd.print("Temperature:");

           lcd.print(t);

          if (DEBUG) Serial.println("Humidity="+String(h)+" %");

          lcd.setCursor(0,1);

          lcd.print("Humidity: ");

          lcd.print(h);

          thingSpeakWrite(t,h);                                      
      }

    

  delay(16000);  

}
