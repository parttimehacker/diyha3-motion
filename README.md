# diyha3-motion
Detect motion and report to the MQTT broker and the django web server. 
## Description: 
This is an updated version of a **Raspberry Pi** project that implements a PIR motion sensor to collect and report status to my "do it yourself home automation" system.  The application requires **Raspbian OS** and is written in **python3**. I usually create a **systemd service** so the application runs at boot. Each python DIYHA application is hosted on a Raspberry Pi server and will respond to a variety of subscribed topic and report on their status or application specific test data. 

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
![PyPI - Python Version](https://img.shields.io/pypi/pyversions/Django)

> Live demo [_here_](https://www.example.com). <!-- If you have the project hosted somewhere, include the link here. -->

## Table of Contents
* [General Info](#general-information)
* [Technologies Used](#technologies-used)
* [Features](#features)
* [Screenshots](#screenshots)
* [Architecture](#architecture)
* [Setup](#setup)
* [Usage](#usage)
* [Project Status](#project-status)
* [Room for Improvement](#room-for-improvement)
* [Acknowledgements](#acknowledgements)
* [Contact](#contact)
<!-- * [License](#license) -->
## General Information
- Provide general information about your project here.
  - This is one of several Python processes used in my home automation system (**DIYHA**). I've used OOP, MVC, and MTV concepts in my DIYHA system. 
- What problem does it (intend to) solve?
  - I wanted to isolate the server information and status into a single process.
- What is the purpose of your project?
  - My home automation system contains environment sensors, motion sensors, LED clocks, light switches, emergency sirens, a django web server, interfaces to Adafruit.io and a mosquitto MQTT broker.
- Why did you undertake it?
  - This was a fun project to learn about python, Raspberry Pi, Arduino processors, hardware and more.
<!-- You don't have to answer all the questions - just the ones relevant to your project. -->
## Technologies Used
- python=3.7.3
- Adafruit-Blinka=7.1.0
- paho-mqt=1.6.1
- RPI.GPIO
- digitalio
- adafruit-circuitpython-rgb-display
- python3-pil

* Prior to Raspian 12
```
sudo pip install Adafruit-Blinka 
sudo pip install adafruit-circuitpython-rgb-display
sudo pip install paho-mqtt==1.6.1
sudo pip install RPI.GPIO 
sudo apt-get install python3-pil
sudo pip install psutil
sudo pip3 install adafruit-circuitpython-ssd1306
```
* Raspian 12 and later
```
sudo pip install Adafruit-Blinka --break-system-packages
sudo pip install adafruit-circuitpython-rgb-display --break-system-packages
sudo pip install paho-mqtt==1.6.1 --break-system-packages
sudo pip install RPI.GPIO --break-system-packages
sudo apt-get install python3-pil --break-system-packages
sudo pip install psutil --break-system-packages
sudo pip3 install adafruit-circuitpython-ssd1306 --break-system-packages
```

## Features
List the ready features here:
- Reports on motion in proximity to a PIR sensor.
- Code passes pylint with a score of 10.0
## Screenshots
Not applicable.
<!-- ![Example screenshot](./diyhadiagram.png)-->
<!-- If you have screenshots you'd like to share, include them here. -->
## Architecture
![Example screenshot](./diyhadiagram.png)
<!-- If you have screenshots you'd like to share, include them here. -->
## Setup
What are the project requirements/dependencies? Where are they listed? A requirements.txt or a Pipfile.lock file perhaps? Where is it located?
- git clone the repository 
```
git clone https://github.com/parttimehacker/diyha3-motion.git
cd diyha3-motion
```
<!--
<div align="left">
    <img src="assettree.png" width="200px"</img> 
</div>
-->
## Usage
You need to decide whether you want to manually run the application or have it started as part of the boot process. I recommend making a **Raspbian OS systemd service**, so the application starts when rebooted or controled by **systemctl** commands. The **systemd_script.sh** creates a admin directory in **/usr/local directory**. The application files are then copied to this new directory. The application will also require a log file in **/var/log directory** named asset.log.
### Manual or Command Prompt
To manually run the application enter the following command (sudo may be required on your system)
```
sudo python3 motion.py --mq MQTTBROKERSERVER --lt LOCATIONTOPIC -ws DJANGOWEBSERVER
```
- MQTTBROKERSERVER is the host name or IP address of MQTT broker. I use the Open Source Mosquitto broker and bridge.
- LOCATIONTOPIC is the MQTT topic name for the location of the server. 
- DJANGOWEBSERVERis the host name or IP address of RESTful API web server. I use django to host my local DIYHAS web site.
### Raspbian systemd Service
First edit the **asset systemd service** and replace the MQTT broker, room values and django web server with their host names or IP addresse. A systemd install script will move files and enable the applicaiton via **systemctl** commands.
- Run the script and provide the application name **asset** to setup systemd (the script uses a file name argument to create the service). 
```
vi motion.service
./systemd_script.sh motion
```
This script also adds four aliases to the **.bash_aliases** in your home directory for convenience.
```
sudo systemctl start motion
sudo systemctl stop motion
sudo systemctl restart motion
sudo systemctl -l status motion
```
- You will need to login or reload the **.bashrc** script to enable the alias entries. For example:
```
cd
source .bashrc
```
### MQTT Topics and Messages
The application subscribes to two MQTT topics and publishes six status messages. Three are are sent at initialization and then handled by a **diy/system/who** message. Three other messages are sent every 15 minutes after calculating an average. The first three are:
```
self.os_version_topic = "diy/" + location + "/motion"
```
- The **diy/system/who** sends local server information to the MQTT Broker. 
## Implementation Status
![Status](https://progress-bar.dev/80/?title=progress)
## Room for Improvement
Include areas you believe need improvement / could be improved. Also add TODOs for future development.

Room for improvement:
- Further refactoring to more generalize the class

To do:
- Integrate into other DIYHA applications and repositories
- Develop a new installation process for seperate repositories
## Acknowledgements
Give credit here.
- My "do it yourself home automation" system leverages the work from the Eclipse IOT Paho project. https://www.eclipse.org/paho/
- Many thanks to...
- Adafruit supplies most of my hardware. http://www.adafruit.com
- I use the PyCharm development environment https://www.jetbrains.com/pycharm/
## Contact
Created by [@parttimehacker](http://parttimehacker.io/) - feel free to contact me!
### Repository Stats
![Your Repository’s Stats](https://github-readme-stats.vercel.app/api?username=parttimehacker&show_icons=true)
### Repository Languages
![Your Repository's Stats](https://github-readme-stats.vercel.app/api/top-langs/?username=parttimehacker&theme=blue-green)
### HITS
![Hits](https://hitcounter.pythonanywhere.com/count/tag.svg?url=https://github.com/parttimehacker)
<!-- Optional -->
<!-- ## License -->
<!-- This project is open source and available under the [... License](). -->

<!-- You don't have to include all sections - just the one's relevant to your project -->
