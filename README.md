## brazil-wireless-counter
Software for the Citizen Science project - Data collection in Sete Barras, Brazil

### Building blocks

1. Hostapd and dnsmasq to serve a Wi-Fi hotspot 
2. Python to serve a web page and RESTful services
3. Lora modules to interface with the antenna

### Setup

These guidelines have been ellaborated using a Raspberry Pi model 3 and Raspbian.

Firstly, configure the Raspberry as a Wi-Fi hotspot following this tutorial:
http://www.raspberryconnect.com/network/item/333-raspberry-pi-hotspot-access-point-dhcpcd-method
In our tests, the Pis have used this configuration:

 *Network: 192.168.4.0 
 *IP for the Pi: 192.168.4.1 
 *Range: 192.168.4.2 - .20 

After this, clone this repository in a folder. In our tests we have used ~/Documents.

Assuming that Python and pip are installed in the sytem, use the following command to install the required modules to run the web service:

```
pip install flask flask-restful
```

Finally, enable SUID in the LoRa binary. This is required for the software to use the LoRa module. From the directory where you have clonned the repo, type:

```
cd lora_interface/cooking/code/LoRa/
sudo ./make_suid ./main-tx-rx.cpp_exe
```

Optional: the letter in counter.conf (root of the repository) is read by the web server to show the RPi name in the web page. Change it to show a different one.

### Running the service

From the root of the repository, type:

```
cd python_server
python server.py
```

### Enabling the service to run at startup

To make the Pi launch the web service at startup, add the following lines to /etc/rc.local *before* exit 0:

```
cd /home/pi/Documents/brazil-wireless-counter/python_server
sudo -H -u pi python server.py &
```

Optionally, add ```service ssh start``` after those lines to enable SSH connectivity once the Pi is running.
