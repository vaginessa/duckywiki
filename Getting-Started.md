
Your only requirement is to have installed [Arduino IDE](https://www.arduino.cc/en/Main/Software). Once installed, download bad_ducky.ino and open it with Arduino IDE. Plug in your device (it should be recognized as Arduino Leonardo), choose proper serial port and upload the sketch. Format your SD card to FAT32 and save your first script on the card.  

For writing scripts you can use any text editor that supports Unix LF line ending (if you are using Windows you can use notepad++). So, let's create first script. Open your text editor and write:

    STRING Hello World!

Save it as demo1.txt on the SD card (device supports 8 characters long file name + 3 characters for extensions), insert the card into the device and plug it to your computer. Open Arduino IDE, check if you have selected proper port and open Serial Monitor. You should get something like this:



Now you can choose desired delivery mode. There are three work modes:
- management - it does not do anything special, it just allows you mode/payload switching (LED is on)
- continuous delivery mode - it executes selected payload every time you plug your device
- auto-disarm mode (one-time delivery) - it executes selected payload only one time, and than the mode will be changed to management

Input "a" and press enter for selecting auto-disarm mode. Now input "demo1.txt", press enter and you are ready for the first script execution. Unplug your device, open text editor and plug it back in. You should be getting "Hello World!" text written in your text editor. If you unplug it and than plug it back again, you should not get anything this time. After executing your script, device switched it's mode to management. 

If you have an error in your script or command is not recognized (even a blank line is considered to be an error), affected line will be skipped and rest of the script will be executed (the LED will start blinking at the end of execution).  
