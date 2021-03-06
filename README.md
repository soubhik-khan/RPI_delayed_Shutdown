# Objective
This is a delayed shutdown solution for a Car head unit(Android Auto based on OpenAuto Pro running on a Raspberry Pi). Generally if we power up the RPI from the cigarette lighter plug, it will turn off abuptly as soon as the the ignition key is switched off. Repeating this over and over may damage the board or kill the SD card very soon! To avoid this and have a graceful soft shutdown of the RPI, I have built this module based on a timer relay(FRM01) inspired by this [thread](https://bluewavestudio.io/community/showthread.php?tid=1128). 

The timer provides power to the RPi when the IGN is on. When the IGN is switched off the timer continues to provide power to the RPi for a configurable amount of time before switching off (e.g. 10 seconds). In tandem with this the relay opens when the IGN is switched off and sets GPIO pin 17 low. A script on the RPi monitors the status of the GPIO pin and ensures that the RPi shuts down before the timer cuts power to the RPi, screen and peripherals (e.g. 15 secs). If the ignition os back on before the RPi shutsdown, it will reset the timerrelay.

## YouTube Demos
- [My Complete Android Auto Setup](https://youtu.be/-Be4rQ46z9c)
- [Power Supply](https://youtu.be/2I0RPV3JhXA)


## Circuit Diagram
![RPI_Shutdown_Circuit](https://user-images.githubusercontent.com/16430033/128670816-533ff493-f1d9-47ca-bc32-3e5413bb67a1.png)

## Component Cost:(Aliexpress)
- [FRM01](https://www.aliexpress.com/item/33002684954.html?spm=a2g0s.9042311.0.0.27424c4dS6PgU7) ~ $6 
- [12V Relay](https://www.aliexpress.com/item/32872184786.html?spm=a2g0s.9042311.0.0.27424c4dS6PgU7) ~ $2.5
- [DC-DC buck converter](https://www.aliexpress.com/item/32901579606.html?spm=a2g0s.9042311.0.0.27424c4dampCZo) ~ $4
- [IN5401 Diode](https://www.aliexpress.com/item/1005001810522699.html?spm=a2g0o.productlist.0.0.17c93299TQ8YJe&algo_pvid=01d20e26-22ef-48fd-81d9-f77d84207031&algo_exp_id=01d20e26-22ef-48fd-81d9-f77d84207031-0) ~ $2.5(20pcs)(we need 3 for this project) 
- Some AWG22 Gauge wires
- Case for the Power supply([Amazon](https://www.amazon.com/gp/product/B07PWT2FBJ/ref=ppx_yo_dt_b_asin_title_o01_s00?ie=UTF8&psc=1)) ~ $8



## Copy the Script

1. Download the power-monitor.py script to your home directory /home/pi
2. Create directory /opt/power-monitor - > ```sudo mkdir /opt/power-monitor```
3. Copy script to /opt/power-monitor/power-monitor.py >```sudo cp /home/pi/power-monitor.py /opt/power-monitor/```
4. Set owner - >```sudo chown root:root /opt/power-monitor/power-monitor.py```
5. Add execute attribute to script - >```sudo chmod +x /opt/power-monitor/power-monitor.py```

## Run it as a Service

1. Download the power-monitor.service to your home directory /home/pi
2. Copy service definition file to /etc/systemd/system/power-monitor.service
```sudo cp /home/pi/power-monitor.service /etc/systemd/system/```
4. Set owner -
```sudo chown root:root /etc/systemd/system/power-monitor.service```
6. Install service - 
```sudo systemctl enable power-monitor.service```
8. Start service - 
```sudo systemctl start power-monitor.service```
10. Check status of service - 
```sudo systemctl status power-monitor.service```



## Testing / debugging 

1. In the script set the DEBUG variable to 1 -
```sudo nano /opt/power-monitor/power-monitor.py```
3. Restart the service - 
```sudo systemctl restart power-monitor.service```
5. Debug output is directed to /var/log/daemon - view with 
```tail -f /var/log/daemon.log```
7. When finished set the DEBUG variable in the script back to 0 
```sudo nano /opt/power-monitor/power-monitor.py```

## Notes
A SPST NC relay can be used instead of a SPST NO relay. To do so edit the power-monitor.py script and change all instances (3) of 
```
if IGN_UP
to 
if not IGN_UP 
```


## About OpenAuto Pro
The OpenAuto Pro is the most advanced Raspberry Pi based, custom head-unit solution ready to retrofit your vehicle.
The main functionality of the OpenAuto Pro software is to bring Users access to modern head-unit features like: Bluetooth Hands Free Profile, music streaming, integrated media player, navigation via Android Auto, screen mirroring, rear camera support, and much more interesting options and possibilities. You can get a version of OpenAuto Pro from [here](https://bluewavestudio.io/). 

**You can use this coupon to get 5% discount on your purchase**
```oap_2021_so```
