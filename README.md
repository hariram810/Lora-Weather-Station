# Lora-Weather-Station

If you have hard-time 3d printing stuff and other materials which i have provided in this project please refer the professionals for the help, [JLCPCB](https://jlcpcb.com/RNA) is one of the best company from shenzhen china they provide, PCB manufacturing, PCBA and 3D printing services to people in need, they provide good quality products in all sectors

[JLCPCB](https://jlcpcb.com/RNA)


Please use the following link to register an account in [JLCPCB](https://jlcpcb.com/RNA)

https://jlcpcb.com/RNA


Pcb Manufacturing

----------

2 layers

4 layers

6 layers

jlcpcb.com/RNA



PCBA Services

[JLCPCB](https://jlcpcb.com/RNA) have 350k+ Components In-stock. You don’t have to worry about parts sourcing, this helps you to save time and hassle, also keeps your costs down.

Moreover, you can pre-order parts and hold the inventory at [JLCPCB](https://jlcpcb.com/RNA), giving you peace-of-mind that you won't run into any last minute part shortages. jlcpcb.com/RNA



3d printing

-------------------

SLA -- MJF --SLM -- FDM -- & SLS. easy order and fast shipping makes [JLCPCB](https://jlcpcb.com/RNA) better companion among other manufactures try out [JLCPCB](https://jlcpcb.com/RNA) 3D Printing servies

[JLCPCB](https://jlcpcb.com/RNA) 3D Printing starts at $1 &Get $54 Coupons for new users

Using the LoRa Module SX1278/RFM95 you can monitor the data from a few kilometer distances (up to 5Km). The device operates on a 3.7V lithium Ion Battery Battery and power consumption is low.

![Uploading Screenshot (695).png…]()


The Gateway can be placed indoors inside the house or can be placed at a certain height to achieve a long distance. The gateway is made using Lora SX1278/RFM95 and ESP32 Wifi Module. The receiver collects the data from the sender or Sensor Node and uploads it to the Server.

The received data can be observed in multiple ways. I will demonstrate how to show the data in 3 ways. The first method is to monitor the data on Webserver. Using the local IP of the ESP32 Board, you can monitor data on the Webpage. In the second method, you can upload the data to Thingspeak Server and monitor the logged data in a graphical format. In the third method, you can monitor the data online on the Blynk Application. You can choose any of the methods to monitor the weather data. Such a great wireless weather station,



The rain sensor detects water that completes the circuits on its sensor boards’ printed leads. The sensor board acts as a variable resistor that will change from 100k ohms when wet to 2M ohms when dry. In short, the wetter the board the more current that will be conducted.

BH1750 Ambient Light Sensor
The BH1750 light intensity sensor is a digital Ambient Light Sensor with an I2C bus interface. This sensor is best suited to get the ambient light data. This sensor can accurately measure the LUX value of light up to 65535. It consumes a very low amount of current & uses a photodiode to sense the light.

he Wireless Weather Station requires Sender and Receiver circuit to communicate wirelessly. So the Sender Circuit is called as Sensor Node and the Receiver Circuit is called as Gateway.

![Screenshot (701)](https://user-images.githubusercontent.com/118633170/209076587-f547c33e-7caf-41bb-9ff8-7497d1e6d715.png)


Sensor Node Circuit
We need to select a low power Arduino Board. So, Arduino Pro Mini is the best board that works on 3.3V & runs on 8 MHz Clock Frequency. We can use the LoRa Module SX1278 from Ai-Thinker. The BH1750 & BME280 Sensor works on I2C Protocol. The LoRa Module SX1278 works on SPI Protocol. The device is powered by 3.7V Lithium-Ion Battery & is connect
ed to the RAW pin of Pro Mini.

The circuit is assembled on a breadboard but you can design a custom PCB for this project. Alright, all the sensors can be placed in a small waterproof box except the rain sensor that needs to be placed outside to monitor the rainfall. The Sensor Node operates on very small power and putting the device to sleep mode will increase battery life. Also removing the unnecessary voltage regulator and using low power LDO or buck converter IC can lower down the power further.

The LoRa Module operates on frequency 433Mhz, but you can select the 868MHz or 915MHz frequency according to your region. You can use any other Lora module with a different antenna as per the availability in your region. Remember the LoRa Based Weather Station is not a waterproof device, so place it inside the waterproof casing.

Derived from the DAVIS Vantage Pro 2 model, the UBIQ-IoT LoRaWAN Weather Station WS100LRW is designed to provide the highest level of accuracy, reliability and ruggedness. It is engineered to withstand the most challenging weather conditions such as scorching sun, corrosion, strong winds and temperature extremes.

It measures air temperature and relative humidity, rainfall, wind speed and direction. The LoRaWAN transmission technology allows to cover long range distances, providing stable and reliable readings. The weather station connects over a private LoRaWAN network and transmits sensor data to a LoRaWAN Gateway.

![Uploading Screenshot (702).png…]()


Before moving to the programming part, you need to install Libraries to the Arduino IDE. The following are the list of Libraries that is used in the code below.

1. LoRa Library
The LoRa library is used for sending and receiving data using LoRa radios. This library exposes the LoRa radio directly and allows you to send data to any radios in range with the same radio parameters. All data is broadcasted and there is no addressing.The weather station data will be simply displayed in a web browser. You can reload the page to refresh the weather data. Or simply using the AJAX function in code you can refresh the data without reloading the page.



#define BLYNK_PRINT Serial

#include <SPI.h>

#include <LoRa.h>

#include <WiFi.h>

#include <Blynk.h>

#include <BlynkSimpleEsp32.h>


char auth[] = "**********************";       // You should get Auth Token in the Blynk App.

char ssid[] = "************";                 // Your WiFi credentials.

char pass[] = "************";


#define SS 5

#define RST 14

#define DI0 2


//#define TX_P 17

#define BAND 433E6

//#define ENCRYPT 0x78


String device_id;

String temperature;

String pressure;

String altitude;

String humidity;

String dewPoint;

String rainfall;

String lux;


void setup()

{

  Serial.begin(115200);

  Blynk.begin(auth, ssid, pass);

  Serial.println("LoRa Receiver");

//LoRa.setTxPower(TX_P);

//LoRa.setSyncWord(ENCRYPT);


  LoRa.setPins(SS, RST, DI0);

  if (!LoRa.begin(BAND))

  {

    Serial.println("Starting LoRa failed!");

    while (1);

  }


  Serial.println("Connecting to ");

  Serial.println(ssid);


//connect to your local wi-fi network

  WiFi.begin(ssid, pass);


//check wi-fi is connected to wi-fi network

  while (WiFi.status() != WL_CONNECTED)

  {

    delay(1000);

    Serial.print(".");

  }

  Serial.println("");

  Serial.println("WiFi connected..!");

  Serial.print("Got IP: ");

  Serial.println(WiFi.localIP());


}


void loop()

{

// try to parse packet

  int pos1, pos2, pos3, pos4, pos5, pos6, pos7;


  int packetSize = LoRa.parsePacket();

  if (packetSize)

  {

    Blynk.run();

// received a packet

    Serial.print("Received packet:  ");

    String LoRaData = LoRa.readString();

    Serial.print(LoRaData);

// read packet

    while (LoRa.available())

    {

      Serial.print((char)LoRa.read());

    }

// print RSSI of packet

    Serial.print("' with RSSI ");

    Serial.println(LoRa.packetRssi());
    
    ![Uploading Screenshot (698).png…]()
    
The LoRa Module operates on frequency 433Mhz, but you can select the 868MHz or 915MHz frequency according to your region. You can use any other Lora module with a different antenna as per the availability in your region. Remember the LoRa Based Weather Station is not a waterproof device, so place it inside the waterproof casing.

Derived from the DAVIS Vantage Pro 2 model, the UBIQ-IoT LoRaWAN Weather Station WS100LRW is designed to provide the highest level of accuracy, reliability and ruggedness. It is engineered to withstand the most challenging weather conditions such as scorching sun, corrosion, strong winds and temperature extremes.

It measures air temperature and relative humidity, rainfall, wind speed and direction. The LoRaWAN transmission technology allows to cover long range distances, providing stable and reliable readings. The weather station connects over a private LoRaWAN network and transmits sensor data to a LoRaWAN Gateway.
    
