# EGI PyNetStation

## About this tool
This package allows you to synchronise EEG experiments created with [Psychopy](https://www.psychopy.org/) with the EGI Netstation amplifier. The original [package](https://github.com/nimh-sfim/egi-pynetstation) was written in Python 3. This is now provided by default with Psychopy 2022.2.0 ([here](https://psychopy.org/api/hardware/egi.html). This package allows using a Network Protocol Connection (NTP) to synchronise computers clocks. This is the same protocol used by Eprime (default software used to present stimuli with EGI machines). 
 
***This version adapts the original package to Python 2. Please, if you can, use Python 3.***

## The reason behind this
This project originates from the willingness to run precise open-source EEG studies in [Psychopy](https://www.psychopy.org/), while the only available stimulus-presentation computer runs Windows XP. In this particular situation, the OS cannot be upgraded and the available version for python is 2.7.15. The original package used to initiate a connection with the amplifier was [egi](https://github.com/gaelen/python-egi). This was directly provided by Psychopy and was cloned from a [repository](https://code.google.com/archive/p/pynetstation/) no longer maintained. 
This system works easily, but it can have some timing issues. Specifically, the time between the stimulus being presented and the trigger being recorded can be inconsistent across each trial, with significant jitter. See this [thread](https://discourse.psychopy.org/t/egi-netstation-visual-timing-latencies/2888) for an in-depth explanation. Furthermore, anecdotal evidence showed that the latencies recorded with Psychopy were greater than those recorded with Eprime. Although this is not critical (but the jitter is), it could put researchers off the use of Psychopy in favour of Eprime. 

To push more researchers, in the laboratory I am working, towards the use of open-source tools, I wanted to improve the usability and reliability of Psychopy in connection with EGI Netstation through the new [egi-pynetstation](https://github.com/nimh-sfim/egi-pynetstation). As reported above, the stimulus computer we have to work with runs on XP and cannot be modified. Thus, I adapted the code of the provided package to Python 2. This now allows the use of the precise NTP connection.

## Dowload and Installation
Download this repository and copy the folder `egi_pynetstation_PY2` into the `lib` folder of your Psychopy installation. This will allow you to import the package through the `import` statement in your script.

Note that the name of this version of the package is `egi_pynetstation_PY2`. I did this on purpose to highlight the difference from the original package. Indeed, this will force you to specifically load the `PY2` version:

```python
from egi_pynetstation_PY2.NetStation import NetStation
``` 

Everything else is exactly as the original. See next section.

## How to use
Use it exactly in the same way as the original egi-pynetstation package. Psychopy provides a [guide](https://www.psychopy.org/hardware/egiNetStation.html) for the use of this with the experiment builder code components.

For instance:

```python
from egi_pynetstation_PY2.NetStation import NetStation # Note: we are asking for the Python 2 version

#IP address of NetStation - CHANGE THIS TO MATCH THE IP ADDRESS OF YOUR NETSTATION
IP_ns = '10.0.0.42'

#IP address of amplifier (if using 300
#series, this is the same as the IP address of
#NetStation. If using newer series, the amplifier
#has its own IP address)
IP_amp = '10.0.0.42'

#Port configured for ECI in NetStation - CHANGE THIS IF NEEDED
port_ns = 55513

#Start recording and send trigger to show this
eci_client = NetStation(IP_ns, port_ns)
eci_client.connect(ntp_ip = IP_amp)
eci_client.begin_rec()
eci_client.send_event(event_type = 'STRT', start = 0.0)
```

## Timing Tests Coming Soon
