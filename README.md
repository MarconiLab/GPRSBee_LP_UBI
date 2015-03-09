# GPRSBee_LP_UBI

This code sends SMS or post to Ubidots using GPRSBee and Seeeduino Stalker V2.3.

In the file Diag.h is defined SEND_SMS. Defined sends a SMS text, undefined post to Ubidots

Send SMS text with SMSdata. SMSdata is time stamped RTC temperature and battery voltage value using GPRSBee. Low power is implemented every SLPNG seconds defined in the code.

Example of SMS message received: 2015/2/9 11:31:35 Temp:26.25, Volt:4.26

Data upload to Ubi is Temperature and Voltage.

#Configuration
Send SMS every SLPNG seconds and go to sleep. SMS massage contains is date and hour + RTC temperature + Battery voltage
Post to Ubidots data temperature and Battery voltage. It is necesary to have an account in Ubidots and the Channel Api key.
  

The code uses Stalker + GPRSBee
  
  Stalker:
    Jumper INT PD2 welded in Stalker to Use Interruption with RTC
    Jumper PD5-En welded in Stalker to swtich on and off bee power
    Cable from Bee port #9 to GPRSBEE_PWRPIN. The GPRSbee uses DTR pin (pin 9)  for software ON/OFF. Switching on or off is simply a matter of momentarily switching DTR high.
   Cable from bee port 12 to XBEECTS_PIN. The CTS pin (pin 12) is used for power status.If the CTS pin is high the GPRSbee is on, if it is low it’s off.
   
   Cable CTS and PWRPin from GPRSBee to Stalker In pins
  
  GPRSBEE_PWRPIN (Stalker) <--> pin#9 (GPRSBee)
  
  XBEECTS_PIN    (Stalker) <--> pin#12 (GPRSBee)
  
  Port0 (Stalker) <--> pin#1 (GPRSBee) Tx
  
  Port1 (Stalker) <--> pin#2 (GPRSBee) Rx
    
  
  
  To read from serial monitor in the computer, in Diag.h #define enable_diag 1 otherwise #undef. Define wich ports are going to be used with serial monitor RX/TX DIAGPORT_TX/DIAGPORT_RX
  
  #define ENABLE_DIAG     1
  
  UartSBee Gnd <--> Gnd Stalker
  
  UartSBee Rx <--> DIAGPORT_TX Stalker
  
  UartSBee TX <--> DIAGPORT_RX Stalker
  
  To program Stalker unconnect pin0 and 1 or take gprsbee out. (sharing ports 0 and 1 from Stalker)
  

## Correct post to Ubidots from serial monitor
+CSQ: 13,0

OK
>> AT+CREG?

+CREG: 0,2

OK
>> AT

OK
>> AT+CREG?

+CREG: 0,2

OK
>> AT

OK
>> AT+CREG?

+CREG: 0,2

OK
>> AT

OK
>> AT+CREG?

+CREG: 0,5

OK
>> AT+CGATT=1

OK
>> AT+SAPBR=3,1,"CONTYPE","GPRS"

OK
>> AT+SAPBR=3,1,"APN","internet.wind"

OK
>> AT+SAPBR=1,1

OK
>> AT+SAPBR=2,1

+SAPBR: 1,1,"10.196.63.161"

OK
>> AT+COPS=?

+COPS: (2,"Si.mobil","","29340"),,(0,1,4),(0,1,2)

OK
>> AT+COPS?

+COPS: 0,0,"Si.mobil"

OK
>> AT+HTTPINIT

OK
>> AT+HTTPPARA="CID",1

OK
>> AT+HTTPPARA="URL","http://50.23.124.66/api/postvalue/?token=YRWsXoUOptheqaDFN2T58o65hyHe4m&variable=54db5c1176254262808e8f2b&value=27.75"

OK
>> >> AT+HTTPPARA="REDIR",1

OK
>> AT+HTTPACTION=0

OK
+HTTPACTION:0,201,168
doHTTPGET failed!

>> AT+HTTPREAD

+HTTPREAD:1

OK
>> AT+HTTPTERM

OK

+SAPBR 1: DEACT

NORMAL POWER DOWN
Received: 5

Errors
Even here we get doHTTPGET failed! Is is because response code is 201 instead of 200 but even though it is posting to Ubidots

+HTTPACTION:0,status,1
status page 222
http://www.simcom.ee/documents/gsm-gprs/sim900/SIM900_AT%20Command%20Manual_V1.09.pdf

##Libraries
https://github.com/SodaqMoja/GPRSbee

##Hardware
Stalker V2.3 http://www.seeedstudio.com/wiki/Seeeduino_Stalker_v2.3

GPRSBee http://www.gprsbee.com
