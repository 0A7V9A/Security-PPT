Who hijacked My Smart Home
--A url hacked all IOT devices
Han Zidong@tencent

Self Introduction
l  l  
  l       

Agenda

� Smart home security introduction � Tradtional attack in IoT � Our advanced approach � Case of some vulnerabilities � Q&A

Smart home security introduction
� Smart home architecture � Vulnerability in IoT Device

IoTBridge With Cloud
Server

IoTBridge Without Cloud Server

 


 

� "!%! 
� 
#!#"!! !& � !  "!%  !#
� 
"!%
� ! ! '
!!#%!( # 
� ! 
$! "$%

 


 

�      �     �  

   
  
 �  
   
 

Tradtional attack in IoT
� Attack target device � Single point attack in IoT devices with more intelligent action � Smart Tv ,Smart Router,Smart Speaker and etc... � Combined Attack in IoT devices with gateway dependency � Smart lamp,Smart adapt,Smart cleaner robot,Smart lock and etc...

Tradtional attack in IoT
� Common Attack Approach
� Heap or Stack Overflow attack � Command Inject � Android/Linux N-Day CVE � External IP and sensitive interface exposure

Some New Attack Approach
� Why a url?
� As a trap to attack more concealed
� What can a url do?
� Gain control of IoT in some way
� Some Attack Surface
� Attack IoT bridge protocol � Security in brain App of IoT � More ...

 

� 
#*'##!"$$"#(%',
� +$# ' #%"$$

 

&%

#*" # $ # 

'&%"#

#!!" #)

%"$$

#"'
"# & #(&


$' &

#%

('#%-'#" &

"##' #"'%#

''"'%,

')#*'

$$(%',"

#(%',&

 

� 
       
�  

Url Attack Smart Home

 

� 





� 

� 
 � 

 � 

� 





�  

 � 

 � 
 


 

RCE From RemoteUrl

Url Entry

DNS-Rebinding

App scheme

Brain App to manage IoT

Open-Port Upnp Action WebSocket Fuzz-Data Inject code RCE IoT Device ......

Private Network

� Attack open-port to get protocal type � Analyze sensitive action or exposed interface � Inject backdoor to access persisting RCE attack

Step 1

Step 2

Pwned

Attack Surface Browser/Url App/Url

Victim with backdoor

Private Network

Smart Tv = Backdoor?

Cases Study
� Smart Tv attack case � GeekPwn hacker-house case

Attack Smart Tv
Case 1
�Expose some Interface with no authorization �Basically DLNA screen-mirroring �Inject backdoor into Tv

Attack Smart Tv
Case 1
�Dangerous Upnp Action �Remote Download->Install App->Launch App �Attacker hjacked private network

Attack Smart Tv
Case 2
�Weak App Code Protection �Communicate with Tv with no authorization �Remote attack Smart Tv imitate Center App Action

GeekPwn hacker-house case
� A mini simulating smart home � Pwn all of IoT devices in this virturl house � Attack and hack IoT device from center brain app � Expand and exploit JSAPI ability to access smart home control � Achieve persistence and concealed

Attack Remote Url

Private-Network Mobile Phone

Hacker-House

Lamp

Speaker

HackerHouse Case 1

Smart Tv Robotic Cleaner SmartLock

Router

Camera

�Analyze and Choose Attack Surface �Pwn target devices with obvious showing

Hacker-
House Case 2

XSS JSAPI

On/Off

Smart Tv Install BackDoor

Lamp Move
Robotic Cleaner

Dns-Hjack

Token DisClouse

Brain App

Mobile Phone

Oauth Router Risk
Speaker BLE Attack
SmartLock

Camera Get Talk Record Remote Playing Music
Remote Opening Door

Hacker-
House Case 3

Brain App

Mobile Phone Vulnerability source

Smart Tv

Router

Speaker Vulnerability diffusion

Robotic Cleaner Lamp

Camera

SmartLock Vulnerability sink

Conclusion
� We have found about 50 0-Day vulnerabilities in famous IoT Vendor within two month
� Code Execution � Remote Contorl � Information Disclouse � Permanent denial of service
� We were ranked #1 in GeekPwn Hacker-House in 2018

Q&A
THANKs
Han Zidong@tencent

