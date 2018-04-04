I've had couple questions on how to create keymap files...  Well, it's pretty tedious, but i didn't find any better solution for "translating" payload on the fly to any other keyboard layout.

Every keyboard character is presented with value between 0 and 255 (1 
byte) while in English keyboard layout printable characters have values between 33 and 126. For more info check [Arduino documentation](https://www.arduino.cc/reference/en/language/functions/usb/keyboard/). I created a little [helper sketch](https://github.com/mharjac/bad_ducky/blob/master/keydumper/keydumper.ino) which will print these characters together with their corresponding values (hex). 

Upload helper sketch to your device, open text editor, change your 
keyboard layout to English and plug your device. You should get something like this:

21 ! 22 " 23 # 24 $ 25 % 26 & 27 ' 28 ( 29 ) 2A * 2B + 2C , 2D - 2E . 2F / 30 0 31 1 32 2 33 3 34 4 35 5 36 6 37 7 38 8 39 9 3A : 3B ; 3C < 3D = 3E > 3F ? 40 @ 41 A 42 B 43 C 44 D 45 E 46 F 47 G 48 H 49 I 4A J 4B K 4C L 4D M 4E N 4F O 50 P 51 Q 52 R 53 S 54 T 55 U 56 V 57 W 58 X 59 Y 5A Z 5B [ 5C \ 5D ] 5E ^ 5F _ 60 ` 61 a 62 b 63 c 64 d 65 e 66 f 67 g 68 h 69 i 6A j 6B k 6C l 6D m 6E n 6F o 70 p 71 q 72 r 73 s 74 t 75 u 76 v 77 w 78 x 79 y 7A z 7B { 7C | 7D } 7E ~ 

Next step is switching your keyboard layout to desired (I'm using Turkish layout for this example) and plugging your device once again:

21 ! 22 " 23 # 24 $ 25 % 26 & 27 ' 28 ( 29 ) 2A * 2B + 2C , 2D - ...   
21 ! 22 İ 23 ^ 24 + 25 % 26 / 27 i 28 ) 29 = 2A ( 2B _ 2C ö 2D * ...

Now eliminate characters with the same values in both layouts:

22 " 23 # 24 $ 26 & 27 ' 28 ( 29 ) 2A * 2B + 2C , 2D - 2E . 2F / 3A : 3B ; 3C < 3D = 3E > 3F ? 40 @ 5B [ 5C \ 5D ] 5E ^ 5F _ 60 ` 7B { 7C | 7D } 7E ~  
22 İ 23 ^ 24 + 26 / 27 i 28 ) 29 = 2A ( 2B _ 2C ö 2D * 2E ç 2F . 3A Ş 3B ş 3C Ö 3D - 3E Ç 3F : 40 ' 5B ğ 5C , 5D ü 5E & 5F ? 60 " 7B Ğ 7C ; 7D Ü 7E é 

In final step you have to walk through all characters in first row 
(English layout) and find corresponding values in targeted layout. For 
example, first character **"** has value 0x22 in English layout and 0x60 in 
Turkish layout. Write down these values as:

`220060`

0x00 in between represents modifier value. Possible modifier keys are:

* KEY_LEFT_CTRL 0x80
* KEY_LEFT_SHIFT 0x81
* KEY_LEFT_ALT 0x82
* KEY_LEFT_GUI 0x83
* KEY_RIGHT_CTRL 0x84
* KEY_RIGHT_SHIFT 0x85
* KEY_RIGHT_ALT 0x86
* KEY_RIGHT_GUI 0x87
* and for none 0x00

Second character **#** has value 0x23 in English layout but there isn't 
such character in targeted layout. Now you have to find out how to 
print this character with targeted keyboard (you can just [google](https://www.google.hr/search?q=turkish+keyboard+layout&source=lnms&tbm=isch&sa=X&ved=0ahUKEwid0dbC0aDaAhWP2qQKHW2uB5IQ_AUICigB&biw=1544&bih=773) for an 
image of targeted keyboard). In my example you can see that with Turkısh layout you have to press **rıght alt** + **number 3** to prınt **#** character. Now append these values:

`220060238633`

Repeat these steps for every character and write gathered values in a new bin file with hex editor. You can test created keymap file with the [keytest payload](https://github.com/mharjac/bad_ducky/blob/master/keydumper/keytest.txt). Open new document in text editor, execute payload with en layout, switch language to your new layout and than execute payload once again in a new line. If resulting lines match you have successfully created the keymap file. 
