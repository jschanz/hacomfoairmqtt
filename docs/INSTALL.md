# Installation

## Quick start as Docker container

### Clone Github repositiory

```bash
git clone https://github.com/jschanz/hacomfoairmqtt.git
cd hacomfoairmqtt
``````

### Edit docker-compose.yml

```yaml
version: '2'

services:    
  hassioaddon-comfoair350:
    image: k42sde/hassioaddon-comfoair350:latest
    container_name: hassioaddon-comfoair350
    environment:
      - SOCAT=True
      - COMFOAIR_IP=192.168.1.50
      - COMFOAIR_PORT=502
      - SERIAL_PORT="/dev/comfoair350"
      - RS485_PROTOCOL=False
      - REFRESH_INTERVAL=10
      - ENABLE_PC_MODE=False
      - DEBUG=False
      - MQTT_SERVER=mosquitto.domain.tld
      - MQTT_PORT=1883
      - MQTT_KEEPALIVE=45
      - MQTT_USER=username
      - MQTT_PASSWORD=password
      - HA_ENABLE_AUTO_DISCOVERY_SENSORS=True
      - HA_ENABLE_AUTO_DISCOVERY_CLIMATE=True
      - HA_AUTO_DISCOVERY_DEVICE_ID=ca350
      - HA_AUTO_DISCOVERY_DEVICE_NAME=CA350
      - HA_AUTO_DISCOVERY_DEVICE_MANUFACTURER=Zehnder
      - HA_AUTO_DISCOVERY_DEVICE_MODEL=Comfoair 350
    restart: always
```

If you want to use a Serial-2-Ethernet-Converter (e. g. from <https://www.waveshare.com> or <https://www.pusr.com/> ), you have to enable it with __SOCAT=True__. During startup socat is creating a virtual device __SERIAL_PORT__ which will connect to __COMFOAIR_IP__ and __COMFOAIR_PORT__. 

```bash
    /usr/bin/socat -d -d pty,link="$SERIAL_PORT",raw,group-late=dialout,mode=660 tcp:"$COMFOAIR_IP":"$COMFOAIR_PORT" &
```

Container was tested with USR-TCP232-302

<https://www.pusr.com/products/1-port-rs232-to-ethernet-converters-usr-tcp232-302.html>

Running the container with local attached serial connection is still untested. Feedback is welcome.

### Starting Docker Container

```bash
docker-compose up
```

If everything is setup correctly, you should see something like that:

```bash
2023-08-24T15:26:31.644203595Z create serial device over ethernet with socat for ip 192.168.1.50:502
2023-08-24T15:26:31.649293698Z 2023/08/24 15:26:31 socat[9] N PTY is /dev/pts/0
2023-08-24T15:26:31.650497425Z 2023/08/24 15:26:31 socat[9] N opening connection to AF=2 192.168.1.50:502
2023-08-24T15:26:31.652213345Z 2023/08/24 15:26:31 socat[9] N successfully connected from local address AF=2 172.31.59.2:46566
2023-08-24T15:26:31.652254376Z 2023/08/24 15:26:31 socat[9] N starting data transfer loop with FDs [5,5] and [7,7]
```

Additionally check the status and settings on the Serial-2-Ethernet-Device. The __TX Count / RX Count__ should increase, if communication is happening.
![Current Status of device](iot-01.png)

If you encounter some stability problems, try to disable the __RESET__ checkbox.
![Current Status of device](iot-02.png)

If you want to connect more than one device, increase the number of concurrent connection.
![Current Status of device](iot-03.png)

## FreeBSD (TrueNAS Core)

[Installation instructions for FreeBSD (e.g. TrueNAS) deployment](https://github.com/adorobis/hacomfoairmqtt/wiki/FreeBSD-(TrueNAS-Core))

## Home Assistant Operating System

[Home Assistant Operating System](https://github.com/adorobis/hacomfoairmqtt/wiki/Home-Assistant-Operating-System)
