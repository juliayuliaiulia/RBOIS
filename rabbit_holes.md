<!--
      /\ /|
      |||| |
       \ | \
   _ _ /  @ @
  /    \  =>X<=
 /|      |  /
 \|     /__| |
   \_____\ \__\
-->
#update
after reading articles on the solarwinds attack, I had many questions about how the attack could have been prevented or better detected

I found [openssf](https://openssf.org/) which includes SLSA which apparently requires/recommends an isolated build machine as well as signed provenance 

unfortunately, i didn't go down any rabbit holes while building this repo, but i'll include...

the most recent rabbit hole i went down, related to [cross device tracking](https://en.wikipedia.org/wiki/Cross-device_tracking#Ultrasonic_tracking), specifically ultrasonic cross device tracking (uXDT) and bioMEMS

here's the paper which i wrote on the topic for one of my classes (it had to be a novel concept, so admittedly, it's a little out there)

## It’s (Not Only) That Damn Phone: Using BioMEMS and uXDT for Tracking
Julia Druck

### Abstract
There are ways to track an individual without permission if they turn off their location services. There are still ways to track them if they turn off their microphone and speaker. And there are still ways to track them if they throw their phone away—one of those ways which may be through certain implantable medical devices.

This paper aims to detail the ways in which interested parties bypass traditional methods to determine an individual’s location, introducing a critical escalation in zero-permission tracking, where medical devices implanted within the human body could fulfill a secondary role as a tracking device. By exploiting Ultrasonic Cross Device Tracking (uXDT) and the mechanical resonance of Micro-Electro-Mechanical Systems (MEMS) sensors, adversaries can transform common devices into covert tracking tools. The extension of this attack to implantable BioMEMS devices, such as pacemakers, illustrates a possible reality where individuals are involuntarily tracked with no means to “shut it off” or “throw it out”. 

### The Groundwork
The primary defense most users use against location tracking is turning off their location services. Yet in the 2017 paper Privacy Threats through Ultrasonic Side Channels on Mobile Devices by Arp et al., an attack was outlined which involves “embed[ding] ultrasonic beacons in audio and track[ing] [users] using the microphone” to determine information such as their “current location…TV viewing habits, or link together… different mobile devices.” This completely bypasses the traditional location gathering metrics, instead relying on sending and receiving ultra high frequency sound as a form of data transfer. 

Ultrasonic Cross Device Tracking, also known as uXDT, relies on a speaker that can transmit ultrasonically, and a microphone that can be used to pick up those transmissions. Importantly, the “ultrasonic frequency range[s] between 18 and 20 kHz”, which is detectable by technology such as mobile phones, but not by the human ear (Arp et al., 2017). This is not a theoretical or niche threat. At the publishing time of Arp et. al.’s paper, the team identified “234 Android applications that are constantly listening for ultrasonic beacons in the background without the user’s knowledge.”

### Upping the Ante
After learning of Ultrasonic Cross Device Tracking, a privacy-conscious user may choose to disable their speaker and microphone to mitigate these kinds of attacks. However, this does not render their device invulnerable to attack. Modern smartphones contain Micro-Electro-Mechanical Systems, otherwise known as MEMS (Matyunin et al., 2018). These MEMS include accelerometers and gyroscopes, which are used to determine velocity and orientation of the device. They provide valuable supplementary information for applications such as GPS—but due to their “low-risk” classification, MEMS are often zero-permission (Matyunin et al., 2018). This means that their data can be accessed without a user permission request, unlike components such as cameras or microphones. 

The MEMS sensors are the exploitable vulnerability in this scenario. They possess a resonant frequency which is reactive to the ultrasonic range, and when an external speaker emits a sound at this exact frequency, it causes the gyroscope’s internal components to vibrate physically. Matyunin et al. demonstrated in their 2018 paper, Zero-Permission Acoustic Cross-Device Tracking, that “by analyzing spectral characteristics of gyroscope’s response to sound, the signal can be decoded even in the presence of device movements, e.g., when a smartphone is held in a hand, or a smartwatch is worn on a wrist.” Furthermore, “adversaries today can embed tracking identifiers into ultrasonic sound and covertly transmit them between devices without users realizing that this is happening.” This is further proven with the 2016 paper Inferring User Routes and Locations using Zero-Permission Mobile Sensors by Narain et al., where the authors “show that a zero-permissions Android app can infer vehicular users’ location and traveled routes, with high accuracy and without the users’ knowledge, using gyroscope, accelerometer, and magnetometer information.” Essentially, the gyroscope, like the microphone from the first scenario, is used unintentionally to obtain data that the user is unaware of, and MEMS may be even more vulnerable to attack than the smartphone microphone due to their zero-permission nature. 

### The Next Level
If the smartphone, which is a device that can be powered down or left behind, is susceptible to this type of attack, the security implications for Implantable Medical Devices are profound. 

Modern pacemakers and other implantable medical devices are increasingly equipped with BioMEMS accelerometers and gyroscopes to deliver a higher standard of care (Marinescu et al., Matula et al., and Arnaud et al.). This paper proposes a novel attack method by which individuals are able to be tracked through their implantable devices’ BioMEMS. 

Furthermore, these implantable devices may be moving increasingly toward acoustic communication, as noted in the 2018 paper authored by Li et al. Feasibility Analysis on the Use of Ultrasonic Communications for Body Sensor Networks states that, “Ultrasonic waves have good propagation in the human body and have been widely applied in biomedical device design without any reported side effects.” and that “ultrasonic waves are feasible and can be used for digital communication.” Additionally, traditional medical device communication channels, such as wireless, are energy-hungry, while ultrasonic power transfer offers an attractive solution and is detailed in the paper Ultrasonic power and data transfer for active medical implants using adaptive beamforming (Li et al.,). 

While this may appear beneficial on the surface, the adoption of acoustic-only communication only widens the attack vector for bad actors. The proposed attack utilizes the internal BioMEMS sensors that would be found in an implantable medical device, such as a pacemaker.
First, the attacker deploys a uXDT emitter in a strategic location. The emitter pulses a unique ultrasonic code, which interacts with the accelerometer or gyroscope embedded in the implantable medical device. The resonant frequency causes this code to be detected as a mechanical vibration. Then, the firmware of the device creates a log of the activity at the specific time at which the ultrasonic frequency was emitted. Finally, if the bad actor has access to these logs, they are able to determine the location of the victim as well as the time they visited that location based on what the ultrasonic code recorded in the logs.

### References
Privacy threats through ultrasonic side channels on mobile devices | IEEE conference publication | IEEE xplore. (n.d.). https://ieeexplore.ieee.org/document/7961950/
Zero-permission acoustic cross-device tracking Nikolay Matyunin. (n.d.-b). https://caslab.io/publications/matyunin2018zeropermission.pdf
Inferring user routes and locations using zero-permission mobile sensors. (n.d.-a). http://www.ccs.neu.edu/home/noubir/publications-local/NVBN2016.pdf
Marinescu, M.-R., Ionescu, O. N., Pachiu, C. I., Dinescu, M. A., Muller, R., & Șuchea, M. P. (2025, October 19). Next-Gen Healthcare Devices: Evolution of MEMS and biomems in the era of the internet of bodies for Personalized Medicine. Micromachines. https://pmc.ncbi.nlm.nih.gov/articles/PMC12565985/
Wiley Online Library | Scientific Research Articles, journals, ... (n.d.-c). https://onlinelibrary.wiley.com/doi/abs/10.1111/j.1540-8159.1996.tb03394.x
Arnaud, A., & Galup-Montoro, C. (2006, September 11). Fully integrated signal conditioning of an accelerometer for implantable pacemakers - analog integrated circuits and Signal Processing. SpringerLink. https://link.springer.com/article/10.1007/s10470-006-9708-y
Li, M., & Kim, Y. T. (2018, December 19). Feasibility analysis on the use of ultrasonic communications for body sensor networks. Sensors (Basel, Switzerland). https://pmc.ncbi.nlm.nih.gov/articles/PMC6308560/

