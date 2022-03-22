### Talking Voron ###  
This is a procedure to have audio files to play throughout your macros. Gives the apperance of a talking Voron  
Here is an example: https://www.facebook.com/reel/393798032097072  
  
## Required Hardware ##
You need something to output sound. I bought a cheap speaker kit for an Arduino (linked below). This is just a small amp board with 2 3-watt speakers, but you can use whatever you choose.  I have the speakers mounted behind the skirt. The board is wired to the same 5V power supply as the Pi and the 3.5mm plug is plugged in to the audio jack of the pi. 
  
With this kit I had a huge ground loop issue. A ground loop causes the speakers to constantly output an audible buzz or tone. I might have been able to resolve it by wiring the power directly to the Pi but I never tried. I had a ground loop isolator, so I just installed that as well which eliminated the problem. A ground loop isolator is just a 1-to-1 transformer that breaks any DC currents.  A better-quality board might have been a better choice. If you experience this same problem with this board or any other board, just search “ground loop isolator” on amazon, there are numerous options.  
Speakers and amp I used:  
https://www.amazon.com/gp/product/B07CRVRG83/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1  
 if you use this kit, I designed a speaker bracket that can be found in the SLT section.  
  
## Audio Files ##
I used mp3s for this. I just used a text to mp3 website to generate the audio I wanted. I then edited them which was not necessary. I added a second of silence at the beginning of each file, increased it by 3db, and added some pitch correction. I have included some of my audio files in the Audio folder if you don’t want to create your own. 

Here is the site I used (US English /Salli Voice):  
https://ttsmp3.com/  
  
## Setting Up the Pi ##  
You will have to install a command line mp3 player. I used mpg123 because it works on Raspbian buster and bullseye.  
  
~~~
### run this command to install  
   
sudo apt install mpg123  

~~~
  
Use your preferred method to transfer the mp3 to your Pi. I use WinSCP. I placed them in “ /Home/pi/Audio” directory. You will have to create it. if you are going to use my configs then place them there too.
  
  
Test to make sure everything works by playing a file
~~~
### play a mp3  
   
/usr/bin/mpg123 /home/pi/Audio/Voron24_is_ready_to_print_please_select_model.mp3

~~~  
  
You should hear this file play. If nothing is heard, check:  
-	your amp board and speakers are wired correctly and on  
-	adjust the volume knob if it has one  
-	run the “alsamixer” command on the pi console and adjust the volume  
  
  
## Setting Up the Macros ##  

Run the Kiauh script you used to install the Klipper and everything else.  
Select option 4 [Advanced]  
Select option 9 [Shell Command]  
  
This will install the shell command extension and place a new config file in the klipper_config directory named shell_command.cfg  

Either copy and paste the contents from my shell_command.cfg or just use WinSCP to replace it. The entries look like the following. The cfg file I have provided has an entry for every sound in the Audio folder

~~~
[gcode_shell_command sound_Homing]  ##name of the command  
command: /usr/bin/mpg123 /home/pi/Audio/Homing.mp3  ## command to execute  
timeout: 5.  
verbose: True  
#    RUN_SHELL_COMMAND CMD=sound_Homing  ## this is the line you place in your macro  
  
~~~  
  
  
 To modify your Macros to play the sound just add the RUN_SHELL_COMMAND CMD=sound_[name of sound]  
 
 Here is how my G34 Macro looks:  
   
 ~~~
#####################################################################  
#   Macros  
#####################################################################  
  
[gcode_macro G32]  
gcode:  
    RUN_SHELL_COMMAND CMD=sound_Homing  
    G28  
    RUN_SHELL_COMMAND CMD=sound_Gantry_aligning  
    QUAD_GANTRY_LEVEL  
    RUN_SHELL_COMMAND CMD=sound_Gantry_aligned_rehoming  
    G28  
    RUN_SHELL_COMMAND CMD=sound_Leveling_Bed  
    BED_MESH_CLEAR  
    BED_MESH_CALIBRATE  
    RUN_SHELL_COMMAND CMD=sound_Bed_leveled_mesh_created  
  
    ##  Uncomment for 350mm build  
    G0 X175 Y175 Z30 F3600  
    #--------------------------------------------------------------------  

~~~
  
 
 

