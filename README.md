# CamillaDSP-Building-a-Config
Building a config for a 3 way Xover with Driver Time Alignment using REW and rePhase.

The aim of this post is to show the process I followed to build a config file for a Linear Phase cross over for CamillaDSP to tri-amp a pair of modified Klipschorns. A secondary aim is to provide enough information for someone to follow the procedure to build a CamillaDSP config for their own speakers. 

The key assumption in following this procedure is that the user can make REW measurements of their system and that implies that CamillaDSP is set up and working and the audio interface is connected to the amplifiers and speakers and the user can navigate the CamillaDSP GUI.

CamillaDSP is running on a RPi5 and using a Motu Ultralight Mk5 audio interface as shown by  https://www.audiosciencereview.com/forum/index.php?threads/rpi4-camilladsp-tutorial.29656/

This is not the only way to build a config, the methods and options I have chosen work for me.

This is not going to be another tutorial on using REW and rePhase, but there are a lot of  measurements shown as screengrabs with the settings and some comments on the process and parameters. REW has excellent help and there are good tutorials on the web, rePhase also has some good tutorials on the web. 

Klipschorns are vintage horn loaded speakers with low distortion and high efficiency, however they suffer from driver time alignment differences due to different horn lengths and the folded horn for bass results in lumpy SPL response, making them ideal candidates for tri-amped DSP systems smoothing SPL responses, time aligning the drivers, providing crossover networks and flattening driver phase response.

My K-Horn mods are a new top hat using B&C DCX464 coax mid/hi drivers mounted on an Eliptrac horn. The B&C DCX464 is a 111db/W 1.4" compression horn with a 290-20,000Hz response, the Eliptrac horn is an eliptical tractrix kit horn with a Fc of 320Hz made from CNC machined MDF and was designed as a replacement for the K400 mid horn in Klipsch heritage speakers. The K-Horn bass is about 105db/W. One advantage of using a coax from a seperate tweeter is that the mid and hi use the same delay for time alignment. Due to horn length the mid and hi are delayed by 5 msec (far more than a standard 3 driver box) and I show how to determine this and the DSP filters required for time alignment.

I am using CamillaDSP with two configurations, one for analog input and the second for streamed content through JiveLite running on the same RPi as CamillaDSP. Switching between the inputs and volume control is done by a remote as detailed in the rpi4-camilladsp-tutorial.

REW measurements showed some difference between phase response on the digital (streamed) and analog inputs. I measured the streamed digital response according to REW Help, Making Measurements, Measuring with file playback. 

The outputs from the Motu UL5/RPi are balanced analog and feed a single stereo N-Core amp for left and right K-Horn bass, a Topping LA90 for mid and a SMSL SH9 THX Headphone amp for hi. 

The nominally 16ohm (tests show 12ohm) B&C DCX are 111db/W and the SH9 will put out 6W into 16ohm and 9W into 8ohm. More than enough power for a domestic system remembering that there are no passive crossovers sucking power. Also, these amps and the Motu UL5 and Raspberry Pi cost about half what I spent a few years ago building an ALK passive steep slope crossover.

For the analog input measurements I use a Behringer ECM8000 measurement condenser mic that connects with an XLR plug to a Motu M4 audio interface. I use loopback in the M4 to provide reference timing. A miniDSP UMIC will work just as well with acoustic timing for reference, an advantage of the Behringer and Motu M4 is that I can also use Open Sound Meter, a disadvantage is that I must calibrate the sound level of the Behringer each measurement session.

*** Photo of laptops running REW USB mic and and Motu M4 setup for XLR mic.
![alt text](<Images/K-Horn with measurement PCs.JPG>)

For the didgital stream measurements I use a miniDSP UMIC and the REW acoustic timing reference recorded on the sweep.
Software used is REW V5.30.x and Rephase 1.4.3 . I run all this on Win 11 on an Asus Zenbook laptop and use WinSCP for file transfer from Laptop to CamillaDSP 2.0.1 on an RPi 5. Both CamillaDSP and my laptop are on a LAN.

Measurements are taken with the tip of the microphone 1m from the centre of the Eliptrac horn which is 118cm off the floor. This microphone position is generally accepted in the Klipsch community for measuring K-Horns as it all but eliminates room reflections. The listening room is 6.5m wide by 10.8m long with 2.6m ceiling. The flooring is polished hardwood with extensive rug covering, couches and furnishings. Walls are sheetrock drywall with curtained windows. Klipschorns are corner horns and the corner sheetrock was re-enforced during building to reduce flex as the corner wall is part of the bass horn. The floor immediately in front of the K-Horns is covered with large wooly dog beds to reduce reflection.

*** Photo of K-Horns and Mics.
![alt text](<Images/K-Horn with mics.JPG>)

The basic procedure I followed is -
1. Measure drivers using REW and use REW EQ to calculate EQ settings for a flat frequency response using 1/6 smoothing. Add the EQs to the Pipeline in CamillaDSP (REW has added the function to save EQ filters in CamillaDSP format while CamillaDSP can import the REW measurements direct to the Config file).

*** REW plot showing Raw and corrected response


2. Set initial Linear Phase XOs in Rephase and add to the Pipeline in CamillaDSP and measure response of individual drivers and Full System 20-20,000Hz (FS). Again, CamillaDSP can directly import rePhase .dbl files into the Config file.

*** Rew plot showing RAW, Corrected and XO response and full system


3. Using FS measurements, determine gains for each driver and add gain filters to the CamillaDSP pipeline. Determine delays for each driver and add delay filters to the pipeline and measure FS to confirm correct delays and gains. Invert driver phase if needed. Adjust XO frequencies and slopes to give the smoothest summed result while taking advantage of "acoustic crossover" of the drivers.

*** REW plot showing cleaned up response


4. Extract a measurement of Excess Phase from a FS measurement and Export it for input to rePhase (thank you fluid). Using previously saved XO settings for mid and hi drivers, import the Excess Phase measurement into rePhase and manipulate the phase to get the Excess Phase close to zero. Replace the XO filter with the new XO and PF (Phase Fix) in the Pipeline. Measure FS to confirm o (zero) degree Excess Phase. As stated earlier, due to phase differences between analog and digital stream inputs I made seperate config files for analog input and stream input.

*** REW plot of PF flat phase


5. Guild the lily by doing another EQ of the full system to flatten SPL peaks and troughs across the cross over regions. Show how to use the CamillaDSP Bass and Treble filters to provide a room curve.

*** REW plot of working config


6. Stream input - show measurements and and resulting working configuration

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