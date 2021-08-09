This is a delayed shutdown solution for a Car head unit(Android Auto based on OpenAuto Pro running on a Raspberry Pi). Generally if we power up the RPI from the cigarette lighter plug, it will turn off abuptly as soon as the the ignition key is switched off. Repeating this over and over may damage the board or kill the SD card very soon! To avoid this and have a graceful soft shutdown of the RPI, I have built this module based on a timer relay(FRM01) inspired by this thread.    

![RPI_Shutdown_Circuit](https://user-images.githubusercontent.com/16430033/128670816-533ff493-f1d9-47ca-bc32-3e5413bb67a1.png)

YouTube Demo

Python Script

1. Download the power-monitor.py script to your home directory /home/pi
2. Create directory /opt/power-monitor - > sudo mkdir /opt/power-monitor
3. Copy script to /opt/power-monitor/power-monitor.py >sudo cp /home/pi/power-monitor.py /opt/power-monitor/
4. Set owner - >sudo chown root:root /opt/power-monitor/power-monitor.py
5. Add execute attribute to script - >sudo chmod +x /opt/power-monitor/power-monitor.py
6. Set required GPIO pin (PORT) and shutdown delay (SHUTDOWN_DELAY) in script ->sudo nano /opt/power-monitor/power-monitor.py


Run it as a Service

1. Download the power-monitor.service to your home directory /home/pi
2. Copy service definition file to /etc/systemd/system/power-monitor.service ->sudo cp /home/pi/power-monitor.service /etc/systemd/system/
3. Set owner - >sudo chown root:root /etc/systemd/system/power-monitor.service
4. Install service - >sudo systemctl enable power-monitor.service
5. Start service - >sudo systemctl start power-monitor.service
6. Check status of service - > sudo systemctl status power-monitor.service



Testing / debugging 

1. In the script set the DEBUG variable to 1 - >sudo nano /opt/power-monitor/power-monitor.py
2. Restart the service - >sudo systemctl restart power-monitor.service
3. Debug output is directed to /var/log/daemon - view with >tail -f /var/log/daemon
4. When finished set the DEBUG variable in the script back to 0 >sudo nano /opt/power-monitor/power-monitor.py

Notes
A SPST NO relay can be used instead of a SPST NC relay. To do so edit the power-monitor.py script and change all instances (3) of 

if not IGN_UP 
to 
if IGN_UP
