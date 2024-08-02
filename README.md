# CamillaDSP-Building-a-Config-2 Add Linear Phase XOs
## 2. Create Linear Phase XOs in Rephase and Add to the Pipeline in CamillaDSP

CamillaDSP supports FIR Filters, so I use rePhase to define the XO filters. The main advantage of rePhase is its Linear Phase filters that do not alter the raw phase response of the drivers. Also, in a step later in the process I use rePhase to flatten the phase response once I have time aligned and set gains of the drivers. Rather than create another FIR filter in the pipeline, I reload the XO filter in rePhase and add the phase manipulation.

ChrisA has written a very informative discussion of the advantages of linear phase.
https://community.klipsch.com/index.php?/topic/182419-subconscious-auditory-effects-of-quasi-linear-phase-loudspeakers/page/3/&tab=comments#comment-2379562

rePhase showing Linear Phase Filter for bass low pass filter.

 
Visit https://rephase.org/ and download rePhase.
Open rePhase and start by selecting the "Linear Phase Filters" tab, select type - either low or high pass, the slope from the drop down list, the param is the steepness in dB per octave and finally the frequency of the crossover. A bass XO will normally be just a single low pass while a mid XO will have a high pass and a low pass. rePhase will draw the predicted response in blue as the data is input. There is a lot of debate for and against steep slope filters, I use them as I have read that they reduce lobing from adjacent driver interference and if done as I describe they sound good.
The "Impulse Settings" panel needs to be altered, the settings I use here are a good starting point for CamillaDSP FIR filters. At the very least, fill in the "taps", "rate", "format", "filename" and "directory". 4096 taps doesn't add too much delay while giving accurate results. I use the same number of taps on each driver so that the delay is the same. The rules of thumb are the lower the frequency the more taps you need and the more pre-ringing you get and never manipulate the phase below 100Hz. Make sure that the rate is the same as in CamillaDSP and that the format is 64 bits IEEE-754 (.dbl).
The "Ranges" panel is used to make the graph more meaningful by defining "from to" frequency, db and degrees, while the "Measurement" panel is used after importing a REW measurement.
When you click Generate, rePhase will calculate the FIR filter with the filename specified and put it in the directory specified. The Generate takes a few seconds on a modern laptop and rePhase will draw the calculated response in red on the graph.
These settings work for me, maybe a rePhase guru could suggest better. 
The blue line is the predicted response, rePhase will write a red line of it's calculated result. The resulting .dbl file is copied to /home/camilla/camilladsp/coeffs. Again my filenames are (pedantic) descriptors.
Save the rePhase settings (File/Save Settings As ...) so that the Phase can be manipulated later. Double clicking on a saved .rephase file will fire up a new instance of rePhase with all settings loaded ready for further modification.
To transfer the generated .dbl file to CamillaDSP, open the Files tab in the CamillaDSP gui, scroll down past the Configs to the Filters section and click the "Upload Files" box
 CamillaDSP gui showing upload files button.jpg
Navigate to the directory where you saved the XO .dbl file, select it and click "open" to import it to the CamillaDSP Filters section. The .rephase files are the saved settings files, double click on one of them and rePhase is fired up and the settings loaded.
 CamillaDSP gui showing upload files select.jpg

Here are the XO Filters in CamillaDSP.
 Config showing dbl files.jpg

and here is a screenshot of a portion of the Filter tab showing how an XO Filter is built using the .dbl file. To add a XO filter, click the green "+" at the end of the filter list, name your filter, select type and subtype, then click the File lookup icon - the middle icon on the left in the filter box - and select the XO.dbl file.
 Config showing Conv filters for XO.jpg
Note that the mid and hi XO have a suffix of PFn meaning Phase Fix n, more about that later.
The top icon on the left will show the plot of the filter, in this case Mid XO.
 CamillaDSP gui showing plot of mid XO.jpg
The last step is to add the XO filter to the pipeline.
 CamillaDSP gui add a pipeline step.jpg

Here is the pipeline showing XOs and Biquad EQ filters with the filters collapsed for clarity.
 CamillaDSP gui pipeline plot  T30 PEQs XO.jpg
Here is a REW measurement Full System (FS) 20-20,000Hz of the UL5 T31 Biquads with XOs config.
 Dec 5 5 T31 81db Fs 20-20kHz Biquads and XOs.jpg
Next step is to set Gain and Delay for each driver and invert phase if needed. The suck out between 3 and 4kHz is a classic indication of a driver out of phase and confirmed by the green phase plot showing 180 degree phase change around the 3600Hz XO.
