# embedCam

Steps to completely set **embeded camera** up:

### Things you need:
- Embeded system microchip with DC power source and an SD Card (I used Raspberry Pi 2, you can try with BeagleBone too)
- Working LAN cable (if you have WiFi enabled embeded microchip like Pi 3 WiFi, you don't need this, obviously)
- Decent Web Cam. (I used Logitech C270 HD Webcam)
- A Linux/MACOS system (Sorry Windows Users) (I used Ubuntu 14.04)

### Steps for Installing:
  
1. Connect your embeded camera to your computer via LAN or WiFi. This sounds easy at first but you have to understand that physically connecting the devices doesn't mean they are actually connected. Power on your chip, connect it via LAN. Go to ***Edit Connections*** in Network Settings and Create a New Connection which is Ethernet. In IPv4 settings, change Method to "Shared with other computers". Unless you see an IP listed in `ifconfig` you are not done connecting.

2. Next step is finding IP Address to SSH our way into the chip. For this, run `ifconfig` on the terminal and look for the IP Adress which says Raspberry in its description. Sometimes, this IP address won't work, in such cases, run `nmap` in the last subnet of the IP Address.

3. In terminal,
   ```sh
   $ ssh -X pi@10.42.0.1
   ```
   Replace IP Address with yours, obviously. For password, enter "raspberry". If everything went fine, you will get access to Raspberry    Pi. If you faced any problem, create an issue and I will help you out.
  
4. Go to the directory where you downloaded "main.cpp" and copy it file to rasberry pi, using-
   ```sh
   $ scp main.cpp pi@10.42.0.1:
   ```
  
5. Following steps are done in Raspberry Pi using SSH. Install OpenCV (latest version, I used 3.4.1) from [here](https://www.learnopencv.com/install-opencv3-on-ubuntu/)
   When you are using Raspbian, installing OpenCV from source can be painfull. However, I strongly suggest you bear with it if you really wish to work on Computer Vision.

6.  Install ZBar from [here](http://blog.mafrog.info/lelectronique/raspberry-pi/installer-zbar-sur-raspberry-pi/)

7. Compile the main.cpp file using - 
    ```sh
    $ g++ main.cpp $(pkg-config --cflags --lflags opencv) -l lzbar -o qrcode.bin
    ```
    or in some cases -
    ```sh
    $ g++ main.cpp /usr/local/include/ /usr/local/lib/ -lopencv_highgui.2.4.8 -lopencv_core.2.4.8 -o qrcode.bin
    ```
   If you get Linking Errors, you probably have not run `sudo ldconfig` or in bad luck, you might have to reinstall everything. 

   
8. Connect Webcam to Rasberry Pi via USB and in terminal-
   ```sh
   $./qrcode.bin
   ```

Now you can bring any QR Code infront of the camera and you will see the info. Speed and framerate are remarkably fast for a simple code like this.
   
