## Arduino Digital Clock ##

![IMG_8690](https://github.com/user-attachments/assets/6af1cc19-3540-4036-96f1-5817e365b3b5)

### Sketch ###
![Sketch Digital Clock](https://github.com/user-attachments/assets/b1e6d706-2d78-4bdc-9be3-9d422e8d0059)

<html>
#include <DS3231.h>
#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <EEPROM.h>
 
String l1,l2,l3,l4;

int hr_24  ;
int hr_12 ;
String AMPM ;

DS3231 rtc;
RTCDateTime now;
//we are using 20x4 LCD -- for 16x4 you would need to change the code accordingly . 
LiquidCrystal_I2C lcd(0x27,2,1,0,4,5,6,7); // 0x27 is the default I2C bus address of the backpack-see article

void setup() {
  Serial.begin(9600);
  rtc.begin(); 
  lcd.begin (20,4); // 20 x 4 LCD module
  lcd.setBacklightPin(3,POSITIVE); // BL, BL_POL
  lcd.setBacklight(HIGH);

  //if your Clock time is incorrect. uncomment the line below and update code.
  //after that time will get sync with system clock 
  //comment out the line below once that time is set. and upload code again.
  //rtc.setDateTime(__DATE__, __TIME__); //TO SET CURRENT DATE TIME
  
  l1 = "JAI GURU DEV" ;
  l2 = "JAI SHIV SHANKAR" ;
  l3 = "Designed By" ;
  l4 = "DEEPAK LOHIA" ;
  printLCD() ;
  delay(2000);
}

void loop() { 
 
  now = rtc.getDateTime();  
  String yy = yy.substring(now.year) ;
  hr_24 = now.hour;
 
  if (hr_24==0) hr_12=12;
  else hr_12=hr_24%12;
  if (hr_24<12) AMPM = "AM";
  else AMPM = "PM";

  l1 = getDW(now.dayOfWeek) + " " +  String( now.day)  + "-" + getMonthStr(now.month)  + "-" + String(now.year) ;  
  l2 = "--------------------";
  l3 = padZero(hr_12) + ":" + padZero(now.minute) + ":" + padZero(now.second)  + AMPM ; 
  l4 = "--------------------";
  
  printLCD();
  delay(1000);
}

void printLCD(){
  //printSerial();
  //lcd.backlight();
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print(l1);
  lcd.setCursor(0,1); 
  lcd.print(l2);  
  lcd.setCursor(0,2); 
  lcd.print(l3);
  lcd.setCursor(0,3); 
  lcd.print(l4);    
  //lcd.noBacklight(); 
}
void printSerial(){
  Serial.println(l1);
  Serial.println(l2);
  Serial.println(l3);
  Serial.println(l4);
}

String getMonthStr(int m){
  String arr[] = {"Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec" };
  return arr[m-1];
}

String padZero(int val){
  if (val < 10){ return "0" + String(val); }
  else{ return String(val); }
}

String getDW(int dw){
  String arr[] = {"Sunday", "Monday", "Tuesday", "Wednsday", "Thursday", "Friday", "Saturday" };
  return arr[dw];
}

</html>
