# CamillaDSP-Building-a-Config
## Building a config for a 3 way Xover with Driver Time Alignment using REW and rePhase.

The aim of this series is to show the procedure I followed to build a config file for a Linear Phase cross over for CamillaDSP to tri-amp a pair of modified Klipschorns. A secondary aim is to provide enough information for someone to follow the procedure to build a CamillaDSP config for their own speakers. 

The key assumption in following this procedure is that the user can make REW measurements of their system and that implies that CamillaDSP is set up and working and the audio interface is connected to the amplifiers and speakers and the user can navigate the CamillaDSP GUI.

CamillaDSP is running on a RPi5 and using a Motu Ultralight Mk5 audio interface as shown by  https://www.audiosciencereview.com/forum/index.php?threads/rpi4-camilladsp-tutorial.29656/

This is not the only way to build a config, the methods and options I have chosen work for me.

While not aiming to be another tutorial on using REW and rePhase, there are a lot of  measurements shown as screengrabs with the settings and some comments on the process and parameters. REW has excellent help and there are good tutorials on the web, rePhase also has some good tutorials on the web. 

K-Horn with AK-3 . This is the starting point, passive XO, no mods bought new in 1990. 
![alt text](<Images/K-Horn measurement AK-3 passive xover.jpg>)
Frequency response like a stage of the Tour de France, phase, well just a few wraparounds.

The basic procedure I followed is -
1. Measure drivers using REW and use REW EQ to calculate EQ settings for a flat frequency response using 1/6 smoothing. Add the EQs to the Pipeline in CamillaDSP (REW has added the function to save EQ filters in CamillaDSP format while CamillaDSP GUI can import the REW measurements direct to the Config file).

https://github.com/wirrunna/CamillaDSP-Building-a-Config-1-Measure-Drivers-with-REW

*** REW plots showing Raw and EQ corrected response for each driver with XO.
![alt text](<Images/RAW and EQd for Bass Mid Hi.jpg>)

2. Set initial Linear Phase XOs in Rephase and add to the Pipeline in CamillaDSP and measure response of individual drivers and Full System 20-20,000Hz (FS). Again, CamillaDSP can directly import rePhase .dbl files into the Config file.

https://github.com/wirrunna/CamillaDSP-Building-a-Config-2-Create-Linear-Phase-XOs

*** Rew plots showing XO response and full system (FS) response
![alt text](<Images/Dec 5 5 T31 81db Fs 20-20kHz Biquads and XOs.jpg>)

3. Using FS measurements, determine gains for each driver to match levels and add gain filters to the CamillaDSP pipeline. Determine delays for each driver and add delay filters to the pipeline and measure FS to confirm correct delays and gains. Invert driver phase if needed. Adjust XO frequencies and slopes to give the smoothest summed result while taking advantage of "acoustic crossover" of the horn drivers.

https://github.com/wirrunna/CamillaDSP-Building-a-Config-3-Set-Gains-and-Delays

*** REW plot showing cleaned up response after adding gain and delay filters.
![alt text](<Images/Dec 5 9 T31 84db Gains Biquads XOs 5ms delay and Inv Phase for Mid and Hi.jpg>)

4. Extract a measurement of Excess Phase from a FS measurement and Export it for input to rePhase (thank you fluid). Using previously saved XO settings for mid and hi drivers, import the Excess Phase measurement into rePhase and manipulate the phase to get the Excess Phase close to zero. Replace the XO filter with the new XO and PF (Phase Fix) in the Pipeline. Measure FS to confirm o (zero) degree Excess Phase. As stated earlier, due to phase differences between analog and digital stream inputs I made seperate config files for analog input and stream input.

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

### First a description of the equipment and room.

Klipschorns are vintage horn loaded speakers with low distortion and high efficiency, however due to different horn lengths there are driver time alignment differences  and the folded bass horn results in a lumpy SPL response making them ideal candidates for a tri-amped DSP system smoothing SPL responses, time aligning the drivers, providing crossover networks and flattening driver phase response.

The stock Klipsch bass horn is about 105db/W and response falls of after 400Hz.

My K-Horn mods are a new top hat using B&C DCX464 coax mid/hi drivers mounted on an Eliptrac horn. The B&C DCX464 is a 111db/W 1.4" compression horn with a 290-20,000Hz response, the Eliptrac horn is an eliptical tractrix kit horn with a Fc of 320Hz made from CNC machined MDF and was designed as a replacement for the K400 mid horn in Klipsch heritage speakers. One advantage of using a coax from a seperate tweeter is that the mid and hi use the same delay for time alignment. Due to horn length the mid and hi are delayed by 5 msec (far more than a standard 3 driver box) and I show how to determine this and the DSP filters required for time alignment.

I am using CamillaDSP with two configurations, one for analog input and the second for streamed content through JiveLite running on the same RPi as CamillaDSP. Switching between the inputs and volume control is done by a remote as detailed in the rpi4-camilladsp-tutorial.

REW measurements showed some difference between phase response on the digital (streamed) and analog inputs. I measured the streamed digital response according to REW Help, Making Measurements, Measuring with file playback. 

The outputs from the Motu UL5/RPi are balanced analog and feed a single stereo Audiophonics MPA-S125NC 75W N-Core amp for left and right K-Horn bass, a Topping LA90 50W for mid and a SMSL SH9 THX Headphone amp for hi. The nominally 16ohm (tests show 12ohm) B&C DCX are 111db/W and the SH9 will put out 6W into 16ohm and 9W into 8ohm. More than enough power for a domestic system remembering that there are no passive crossovers sucking power. Also, these amps and the Motu UL5 and Raspberry Pi cost about half what I spent a few years ago building an ALK passive steep slope crossovers.

For the analog input measurements I use a Behringer ECM8000 measurement condenser mic that connects with an XLR plug to a Motu M4 audio interface. I use loopback in the M4 to provide reference timing. A miniDSP UMIC will work just as well with acoustic timing for reference, an advantage of the Behringer and Motu M4 is that I can also use Open Sound Meter, a disadvantage is that I must calibrate the sound level of the Behringer each measurement session.


For the didgital stream measurements I use a miniDSP UMIC and the REW acoustic timing reference recorded on the sweep.
Software used is REW V5.30.x and Rephase 1.4.3 . I run all this on Win 11 on an Asus Zenbook laptop. The CamillaDSP RPi5 is on the LAN and its GUI is accessed via Firefox browser. The GUI is used to transfer filters from the Laptop to CamillaDSP on the RPi 5. Both CamillaDSP and my laptop are on a LAN.

Measurements are taken with the tip of the microphone 1m from the centre of the Eliptrac horn which is 118cm off the floor. This microphone position is generally accepted in the Klipsch community for measuring K-Horns as it all but eliminates room reflections. The listening room is 6.5m wide by 10.8m long with 2.6m ceiling. The flooring is polished hardwood with extensive rug covering; couches and soft furnishings also provide sound absorbtion. Walls are sheetrock drywall with curtained windows. Klipschorns are corner horns and the corner sheetrock was re-enforced during building to reduce flex as the corner wall is part of the bass horn. The floor immediately in front of the K-Horns is covered with large wooly dog beds to reduce reflection.

*** Photo of K-Horns and Mics.
![alt text](<Images/K-Horn with mics.JPG>)



DSP to amplifier setup.
My primary source is music streamed from my Squeezebox Server (LMS) to software players running JiveLite, TV sound comes in as analog.

JiveLite runs on the same Raspberry Pi that runs the CamillaDSP software and feeds CamillaDSP a digital stereo stream. The RPi5 is mounted on the back of a 10.1" touch screen that provides touch control of JiveLite and the CamillaDSP GUI provides a visual indicator of the volume level. The mounting of the RPi5 behind the screen meant no case was neede and aids the passive cooler. The RPi connects to the screen with an HDMI cable and a USB for the touch function. Once connected it just works.

A Motu UltraLite Mk5 USB audio interface provides I/O and converts TV analog to digital and feeds CamillaDSP via USB for processing. JiveLite streams to CamillaDSP within the RPi5..
The processed digital streams feed the Motu Ultralite via the USB and are converted to analog and balanced analog is fed to the amplifiers. The Motu has sockets for 1/4" TRS plugs (Tip Ring Screen) so I made sets of TRS to XLR cables although these are freely available commercially.

A simple remote and a FLIRC provide source selection and volume control.

Pic of RPi display running CamillaDSP and JiveLite with remote in front.
![alt text](<Images/10.1 screen front on shelf.jpg>)

Pic of amps - bottom left N-Core for bass, top left SMSL headphone amp for Hi, top right Topping LA90 for mid and bottom right Motu UL5. The amp stands are plastic wire coated shelf stands from K-Mart and provide exellent air flow.
![alt text](<Images/Motu UL5 and amps.JPG>)
 
### Getting Started with CamillaDSP *** Under construction ***
I am assuming you have installed CamillaDSP according to Michael's tutorial. The easiest way to get it working is to download a config file from Michael's site.
Once you have a RPi setup and CamillaDSP installed and running it is time to load your first config.
*** CamillaDSP GUI with blank config
, you should be able to get the GUI 


* Connect Motu M4, download from Michael's tutorial configs for M4 stream and M4 analog.
In CDSP GUI Files tab, first, click New blank config, then click Import Config and 

*** Pic of Files tab with cursor on Import Config box

*** Pic of Files Tab after clicking Import Config box

Click the CamillaDSP Config box and navigate to the folder you downloaded the configs from the Tutorial and select the analog config.
* message successful import, 
*** Pic of Successful download
then down to Enter a name for the new config
*** Pic of new config dialog
and click the disk icon to save it, then apply to GUI.

Click the Pipeline tab and check the pipeline.

 


find IP address, display in browser - local screen 127.0.0.1
go to Files Tab.

### Measuring setup.
1. Microphones.
For measurement I use a Behringer ECM8000 mic connected to a Motu M4 audio interface, also there is a UMIC-1 connected to a second pc via USB. 
This pic shows the two mics in a retort stand in front of the right K-Horn, dog bed reflection absorber on the floor between the speaker and the mics.
![alt text](<Images/K-Horn with mics.JPG>) 

This pic is of the two pcs with the Motu M4, speaker under measurement in the background.
![alt text](<Images/K-Horn with measurement PCs.JPG>) 


Setting up for Umik-1 USB mic.
REW prefs for Umik-1, first in the 'Soundcard' tab select your microphone. 
![alt text](<Images/REW Prefs for UMIC Analysis.jpg>) 

In the 'Cal files' tab select the Umik-1 calibration file, then in the 'Analysis' tab set these defaults.
![alt text](<Images/REW Prefs for UMIC Soundcard.jpg>)
 
Finally, in the 'Equaliser' tab select CamillaDSP which REW will use to determine the number of IIR filters for EQ and the behaviour of the save files routines to suit CamillaDSP.
![alt text](<Images/REW Prefs - Equaliser.jpg>)

Seting up for Behringer ECM8000 mic.
Motu M4 front connections, IN1 - Loopback cable TRS plug, IN2 - Mic via XLR plug.
![alt text](<Images/Motu m4 front.JPG>)

and back showing Line Out TRS sockets - 3L Loopback cable to Front, 4R TRS to Motu Ultralight Mk5 / CamillaDSP and USB to PC.
![alt text](<Images/Motu M4 back.JPG>) 

In REW Preferences, Soundcard 
and Analysis - note Adjust clock with loopback ticked
 

2. CamillaDSP
In CamillaDSP I load a config with no filters, the analog TRS plug from the Motu M4 goes into ch2 for Left or ch3 for Right
The Pipeline -
 
UL5 analog - Blank.yml.jpg
and in the Mixer I mute all outputs except the one I am measuring.
 
CamillaDSP showing muted channels with "destination 3" (channel 3) open for measuring.jpg





Flat Excess Phase is still a contentious issue so I have provided a couple of links for reading.

Thank you Henrik for CamillaDSP and Michael for his tutorial on setting up Camilla on a Raspberry Pi and a Motu Ultralight Mk 5. Thanks also to Cask05 for help with REW in my early days of coming to grips with DSP.

Links :

https://www.roomeqwizard.com/

https://www.avnirvana.com/forums/official-rew-room-eq-wizard-support-forum.10/

https://rephase.org/

https://winscp.net/eng/download.php

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




