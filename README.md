# monitor_interface
Monitor Interfaces Command for Arista Devices

This script continually monitors interfaces in one, or more Arista devices. 

I was a fan of the "old school" command in Junos and wanted an improved version:

```
monitor interface traffic

```
I created this program to allow one or a number of switches to be monitored at once, running on-box or off, and monitoring multiple defined interfaces, or all, and define variables like packet count, min packet rate to  show etc.  

See the help below:

```
Usage: monitor_interfaces.py [options]

Options:
  -h, --help        show this help message and exit
  -b                This option enables KBps to be dispalyed.  Default is off.
  --count_packet    This option enables packet count to be dispalyed.  Default
                    is off.
  --safe            This option enables configurations to be backed up before
                    running the program and restored upon exiting (Thus
                    keeping the load-interval commands as previous).  Default
                    is off.
  -u USERNAME       Username. Mandatory option
  -p PASSWORD       Password. Mandatory option
  -l LOAD_INTERVAL  explicit interface load-interval rate. If set then every
                    interesting interface has its load-interval set, and
                    subsequently removed when packet-rate drops below 2pps.
                    Values 5-300.  By default the programs sets the system
                    default load-interval to 5
  -r PPSRATE        min packet rate.  Any interface with a rate above this
                    will report status. Default is 2 pps.
  -a HOSTNAMES      One or more hostnames (or IP addresses) of the switches to
                    poll.  Comma separated.  Mandatory option with multiple
                    arguments
  -i INTERFACES     optional argument with multiple arguments.  Ethernet Ports
                    Only- Format: Ethernet<num>, or Eth<num>, comma separated,
                    or range separated with- e.g. Eth21-45 or
                    Eth1,Eth7,Eth21-45
                    

```

As an example if I want to monitor devices lf275,fm213,fm382, but only interfaces Eth2,4 and range 41-48, I want to show packet count and and KBps count I would run the follwoing:

```
python monitor_interfaces.py -u admin -p admin -r 10 -a lf275,fm213,fm382 --count_packet  -b -i Eth1,Eth2, Eth41-48
```

For no KBps count and all interfaces over the rate of 10pps:


```
python monitor_interfaces.py -u admin -p admin -r 10 -a lf275,fm213,fm382 --count_packet
```

Example of the last command shown below:

```


================ Switch: lf275 ===========


Interface                  Input Packets            PPS     Output Packets             PPS                 

==========================================================================================


Ethernet1                             39              0            77703840           1003

Ethernet14                            36              0            49027568           1003

Ethernet41                     158694642           2005            16227613              0



================ Switch: fm213 ===========


Interface                  Input Packets            PPS     Output Packets             PPS                 

==========================================================================================


Ethernet1                             23              0             3581081            230

Ethernet2                             23              0            10592283            512

Ethernet46                      77704893            999                  39              0

Ethernet48                            35              0            17090854            256



================ Switch: fm382 ===========


Interface                  Input Packets            PPS     Output Packets             PPS                 

==========================================================================================


Ethernet14                      49029752            999                  36              0

Ethernet48                            35              0            18652383            999

```


