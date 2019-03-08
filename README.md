# Running motion detection via webcam while turning on Pi3

### STEPs:

**1. Buy a Webcam supporting RPi or Linux. You can refer to [RPi USB Webcams](https://elinux.org/RPi_USB_Webcams). For me personally, I choose KINYO PCM-512.**

**2. Update Pi.**
   1. `$sudo apt-get update`
   2. `$sudo apt-get upgrade`

**3. Download and install motion. `$sudo apt-get install motion`**
   
**4. Edit this file with the following changes. `$sudo nano /etc/motion/motion.conf`**

```
$daemon on 
$stream_localhost off
$webcontrol_localhost off
$ffmpeg_output_movies on
$target_dir /home/pi/Documents/motion
$stream_maxrate 100
$framerate 60
$width 640
$height 480
$on_event_start python /home/pi/background/motionalert.py %f
$on_movie_end python /home/pi/background/motionvid.py %f
$stream_port 8081
$output_pictures locate_motion_style
```

    About port selection, please refer to [List of TCP and UDP port numbers](https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers), <https://stackoverflow.com/questions/218839/assigning-tcp-ip-ports-for-in-house-application-use>
   
**5. Then edit this file. `$sudo nano /etc/default/motion`**
   And add a description in this file. `start_motion_daemon=yes` 

**6. Create a python program as "email_motion.py".**

**7. The last step is that:**
   1. `$sudo nano ~/.config/lxsession/LXDE-pi/autostart`
   2.  Then add a row of script: `@lxterminal-e sudo motion`

**8. Start the motion up to see if it works or not.**
   1. `$sudo service motion start` #you can change the command to "stop", or "restart"
   3. `$sudo motion`
   2. `http://[IPaddress]:[stream_port]`

**9. Now, you can turn off and then turn on your pi3 to test this function to check if it works or not. `$sudo reboot`**


### Note:
**1. Note that Motion occasionally fails to start up due to previous Motion processes that have failed to shut down properly and are running in the background. If you suspect this, try running the following:**
```
$ps aux | grep motion
$sudo kill -9 [pid]
````


### Reference:

**1. [Raspberry Pi 3 Motion Detection Camera With Live Feed](https://www.instructables.com/id/Raspberry-Pi-Motion-Detection-Security-Camera/)**
<br/>
**2. [Building a Motion Activated Security Camera with the Raspberry Pi Zero](https://www.bouvet.no/bouvet-deler/utbrudd/building-a-motion-activated-security-camera-with-the-raspberry-pi-zero)**
<br/>
**3. <http://blog.cavedu.com/2017/09/13/raspberry-pi%E8%87%AA%E8%A3%BD%E7%B8%AE%E6%99%82%E6%94%9D%E5%BD%B1%E6%A9%9F/>**
<br/>
**4. time lapse video free software : [Startrails](http://www.startrails.de/html/software.html)**


