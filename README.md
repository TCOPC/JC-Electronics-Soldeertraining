Fork van de originele JC-Electronics soldeertraining.

ESPHome frimware voor de soldeertraining om een mediaplayer te maken voor Home Assistant.
Op dit moment werkt alleen de announcement_pipeline nog vanwege een bug in ESPHome, door de bug werkt de mixer component niet op de originele ESP32.


I2S gebruikt de volgende pinnen:
ESP32 pin  -  I2C signal
```
GPIO25        - LRCK
GPIO27        - BCLK
GPIO26        - DATA
```

I2C gebruikt de volgende pinnen:
ESP32 pin  -  I2C signal
```
GPIO32   - SCL
GPIO33   - SDA
```
AMP enable zit op GPIO18
