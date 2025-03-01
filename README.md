# CamillaDSP-Building-a-Config
## Building a config for a 3 way Xover with Driver Time Alignment using REW and rePhase.

The aim of this series is to show the procedure I followed to build a config file for a Linear Phase cross over for CamillaDSP to tri-amp a pair of modified Klipschorns. A secondary aim is to provide enough information for someone to follow the procedure to build a CamillaDSP config for their own speakers. 

The key assumption in following this procedure is that the user can make REW measurements of their system and that implies that CamillaDSP is set up and working and the audio interface is connected to the amplifiers and speakers and the user can navigate the CamillaDSP GUI on a browser on a laptop.

REW, rePhase and the CamillaDSP GUI in a browser all run on the same laptop.

CamillaDSP is running on a RPi5 and using a Motu Ultralight Mk5 audio interface as shown by  https://www.audiosciencereview.com/forum/index.php?threads/rpi4-camilladsp-tutorial.29656/

This is not the only way to build a config, the methods and options I have chosen work for me.

While not aiming to be another tutorial on using REW and rePhase with CamillaDSP, there are a lot of  measurements shown as screengrabs with the settings and some comments on the process and parameters. REW has excellent help and there are good tutorials on the web, rePhase also has some good tutorials on the web. 

For day to day use, I run CamillaDSP on an RPi mounted on the back of a 10.1" screen, this pic shows the CamillaDSP GUI and JiveLite streaming software.

![alt text](<Images/10.1 screen pi running CDSP & JiveLite.jpg>)
10.1 screen pi running CDSP & JiveLite.jpg

Here is a REW measurement of my K-Horn with AK-3 passive crossover . This is the starting point, passive XO, no mods bought new in 1990. 
![alt text](<Images/K-Horn measurement AK-3 passive xover.jpg>)
Frequency response like a stage of the Tour de France, phase, well just a few wraparounds.

The basic procedure I followed was -
1. Measure drivers using REW and use REW EQ to calculate EQ settings for a flat frequency response using 1/6 smoothing. 
Add the EQs to the Pipeline in CamillaDSP (REW has added the function to save EQ filters in CamillaDSP format while CamillaDSP GUI can import the REW measurements direct to the Config file).

https://github.com/wirrunna/CamillaDSP-Building-a-Config-1-Measure-Drivers-with-REW

REW plot showing Raw and EQ corrected response for each driver.
![alt text](<Images/Bass Mid Hi raw EQ.jpg>)
Bass Mid Hi raw EQ.jpg

2. Set initial Linear Phase XOs in Rephase and add to the Pipeline in CamillaDSP and measure response of individual drivers and Full System 20-20,000Hz (FS). CamillaDSP can import rePhase .dbl files direct to the Config file.

https://github.com/wirrunna/CamillaDSP-Building-a-Config-2-Create-Linear-Phase-XOs

Rew plot showing driver response with EQ and XO
![alt text](<Images/Bass Mid Hi EQ XO.jpg>)
Bass Mid Hi EQ XO.jpg

3. Using FS measurements, determine gains for each driver to match levels and add gain filters to the CamillaDSP pipeline. Determine delays for each driver and add delay filters to the pipeline and measure FS to confirm correct delays and gains. Invert driver phase if needed. Adjust XO frequencies and slopes to give the smoothest summed result while taking advantage of "acoustic crossover" of the horn drivers.

https://github.com/wirrunna/CamillaDSP-Building-a-Config-3-Set-Gains-and-Delays

*** REW plot showing cleaned up response after adding gain and delay filters.
![alt text](<Images/Dec 5 9 T31 84db Gains Biquads XOs 5ms delay and Inv Phase for Mid and Hi.jpg>)

4. Extract a measurement of Excess Phase from a FS measurement and Export it for input to rePhase (thank you fluid). Using previously saved XO settings for mid and hi drivers, import the Excess Phase measurement into rePhase and manipulate the phase to get the Excess Phase close to zero. Replace the XO filter with the new XO and PF (Phase Fix) in the Pipeline. Measure FS to confirm o (zero) degree Excess Phase.

https://github.com/wirrunna/CamillaDSP-Building-a-Config-4-Get-Excess-Phase-to-Zero

*** REW plot of Full System with PF flat phase
![alt text](<Images/Jun 23 2 T44_A67 new pf - no input peqs.jpg>)

5. Add the finishing touches by doing another EQ of the full system to flatten SPL peaks and troughs across the cross over regions. Show how to use the CamillaDSP Bass and Treble filters to provide a room curve.

https://github.com/wirrunna/CamillaDSP-Building-a-Config-5-Finishing-Touches

Here are some REW plots showing where I got to.

*** REW plot of working config including EQ on the inputs
![alt text](<Images/Jun 23 5 T45_A67 FS 77db new pf input peqs.jpg>)

*** REW plot showing Impulse (Step Response)
![alt text](<Images/Jun 23 2 T44_A67 new pf - no input peqs Impulse - Step Response.jpg>)

### Klipschorns and room.

Klipschorns are heritage horn loaded speakers with low distortion and high efficiency, however due to different horn lengths there are driver time alignment differences  and the folded bass horn results in a lumpy SPL response making them ideal candidates for a tri-amped DSP system smoothing SPL responses, time aligning the drivers, providing crossover networks and flattening driver phase response.

The stock Klipsch bass horn is about 105db/W and response falls of after 400Hz.

My K-Horn mods are a new top hat using B&C DCX464 coax mid/hi drivers mounted on an Eliptrac horn. The B&C DCX464 is a 111db/W 1.4" compression horn with a 290-20,000Hz response, the Eliptrac horn is an eliptical tractrix kit horn with a Fc of 320Hz made from CNC machined MDF and was designed as a replacement for the K400 mid horn in Klipsch heritage speakers. One advantage of using a coax from a seperate tweeter is that the mid and hi use the same delay for time alignment. Due to horn length the mid and hi are delayed by 5 msec (far more than a standard 3 driver box) and I show how to determine this and the DSP filters required for time alignment.

The listening room is 6.5m wide by 10.8m long with 2.6m ceiling (21 x 35.4 x 8.5 feet), kitchen and dining area at one end, speakers, TV and heater at the other. The flooring is polished hardwood with extensive rug covering; couches and soft furnishings also provide sound absorbtion. Walls are sheetrock drywall with curtained windows. Klipschorns are corner horns and the corner sheetrock was re-enforced during building with extra studs and structural ply to reduce flex as the corner wall is part of the bass horn. The floor immediately in front of the K-Horns is covered with large wooly dog beds to reduce reflection.


### DSP to amplifier setup.
My primary source is music streamed from a Squeezebox Server (LMS) to software players running JiveLite, TV sound comes in as analog. Jivelite is the GUI counterpart to Squeezelite that interacts with LMS to control the player streams and can be also be used to control remote players.

JiveLite runs on the same Raspberry Pi that runs the CamillaDSP software and feeds CamillaDSP a digital stereo stream. The RPi5 is mounted on the back of a 10.1" touch screen that provides touch control of JiveLite and the CamillaDSP GUI provides a visual indicator of the volume level. The mounting of the RPi5 behind the screen meant no case was needed and aids the passive cooler. The RPi connects to the screen with an HDMI cable and a USB for the touch function. Once connected it just works.

A Motu UltraLite Mk5 USB audio interface provides I/O and converts TV analog to digital and feeds CamillaDSP via USB for processing. JiveLite streams to CamillaDSP within the RPi5.
The processed digital streams feed the Motu Ultralite via the USB and are converted to analog and balanced analog is fed to the amplifiers. The Motu has sockets for 1/4" TRS plugs (Tip Ring Screen) so I made sets of TRS to XLR cables although these are freely available commercially.

A simple remote and a FLIRC provide source selection and volume control.

Pic of RPi display running CamillaDSP and JiveLite with remote in front.
![alt text](<Images/10.1 screen front on shelf.jpg>)

Pic of amps - bottom left N-Core for bass, top left SMSL SH9 THX Headphone amp for Hi, top right Topping LA90 for mid and bottom right Motu UL5. The amp stands are plastic wire coated shelf stands from K-Mart and provide exellent air flow.
![alt text](<Images/Motu UL5 and amps.JPG>)
 
Pic of amps - connector side - Topping LA90 and SMSL SH9 THX, Bobwire 12v Trigger, and the Motu UL5 underneath with TRS plugs for the three amps and TRS input plugs for analog in.
![alt text](<Images/Amp and UL5 back.jpg>)

The Bobwire 12v Trigger solves the problem of having to turn on 3 stereo amps everytime you play music. Use of the Bobwire is part of the RPi4-CamillaDSP-Tutorial https://github.com/mdsimon2/RPi-CamillaDSP?tab=readme-ov-file#trigger-output

### Measuring setup with REW.
So with REW running on a PC we have to get the signal to CamillaDSP and routed to the amplifier / speaker. 

Two basic options, an analog signal out of the headset jack into an analog input on the USB Audio Interface or a digital signal via USB straight to the Raspberry Pi running CamillaDSP. 

To use the USB method the Raspberry Pi has to be configured as a USB end point which in Raspberry Pi land is called Gadget Mode. This is the best way as the signal stays in the digital domain. I used the config and the USB-C power/data splitter in the guide.
https://github.com/mdsimon2/RPi-CamillaDSP?tab=readme-ov-file#8-enable-usb-gadget-optional with the addition of 96000 capture rate to the cs_rate parameter.  

When using USB interfaces in REW there is no need to calibrate the interface (see REW Help, Getting set up for measuring).

*** Screenshot of REW Pref setup for Gadget Mode and UMIC-1
![alt text](<Images/Gadget Mode REW output device selection 48k.jpg>)


In the 'Cal files' tab select the Umik-1 calibration file, then in the 'Analysis' tab set these defaults.
 
![alt text](<Images/REW Prefs for UMIC Analysis.jpg>) 
 
Finally, in the 'Equaliser' tab select CamillaDSP which REW will use to determine the number of IIR filters for EQ and the behaviour of the save files routines to suit CamillaDSP.
![alt text](<Images/REW Prefs - Equaliser.jpg>)


CamillaDSP

The Pipeline -
![alt text](<Images/REW UL5 Gadget in - pipeline.jpg>)


and in the Mixer I mute all outputs except the one I am measuring. 
![alt text](<Images/UL5 Gadget in Mixer.jpg>)


WinSCP - this utility is excellent for looking at the RPi files from a windows laptop. Well worth downloading installing and configuring as shown below will allow editing and saving of files - and deleting, so be careful.

![alt text](<Images/WinSCP setup.jpg>)

Thank you Henrik for CamillaDSP and Michael for his tutorial on setting up Camilla on a Raspberry Pi and a Motu Ultralight Mk 5. Thanks also to Cask05 for help with REW in my early days of coming to grips with DSP.


Flat Excess Phase is still a contentious issue so I have provided a couple of links for reading.

Links :

https://www.roomeqwizard.com/

https://www.avnirvana.com/forums/official-rew-room-eq-wizard-support-forum.10/

https://rephase.org/

https://winscp.net/eng/download.php
https://winscp.net/eng/docs/guides

https://community.klipsch.com/index.php?/topic/117543-active-bi-ampingtri-amping-faq/#comments

https://www.avnirvana.com/resources/getting-started-with-rew-a-step-by-step-guide.19/

https://www.avnirvana.com/threads/some-guides-to-rew-and-acoustic-measurement.121/

https://audiophilestyle.com/forums/topic/30035-using-rew-and-rephase-to-generate-amplitude-and-time-domain-corrections/?tab=comments#comment-604620

Links to Linear Phase discussions :

https://mynewmicrophone.com/the-complete-guide-to-linear-phase-equalization-eq/

https://www.linkwitzlab.com/Attributes_Of_Linear_Phase_Loudspeakers.pdf

https://community.klipsch.com/index.php?/topic/182419-subconscious-auditory-effects-of-quasi-linear-phase-loudspeakers/


Link to development of the Eliptrac horn

https://community.klipsch.com/index.php?/topic/122814-round-tractrix/

###camilladsp-controller
Environment: RPi 5 running full desktop, username camilla, Output DAC8X, config file attached

1. Install pyalsa :
sudo apt install python3-pyalsa

2. Re-install venv with the new system package pyalsa :
sudo python -m venv --system-site-packages /opt/venv

3. Install camilladsp-controller :

git clone https://github.com/HEnquist/camilladsp-controller ~/camilladsp/camilladsp-controller


4. Modify a working config that has a resampler by appending _{samplerate} to the name of the .yaml file -
for example config file "Gin_96k_DAC8X_out_Blank.yml" becomes "Gin_96k_DAC8X_out_Blank_96000.yml"

5. Run command for camilladsp-controller in ADAPT mode where it will adapt an existing config for other sample rates :

/home/camilla/camilladsp/.venv/bin/python3 /home/camilla/camilladsp/camilladsp-controller/controller.py -d hw:UAC2Gadget -p 1234 -a /home/camilla/camilladsp/configs/Gin_96k_DAC8X_out_Blank_96000.yml

/opt/venv/bin/python3 /home/camilla/camilladsp/camilladsp-controller/controller.py -d hw:UAC2Gadget -p 1234 -a /home/camilla/camilladsp/configs/Gin_96k_DAC8X_out_Blank_96000.yml



Notes: CamillaDSP must be running, at least in the "running" or "stalled" state before you run the controller.

I run camilladsp-controller in a Putty terminal window

