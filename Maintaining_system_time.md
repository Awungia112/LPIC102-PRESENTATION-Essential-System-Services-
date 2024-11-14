# Essential System Services 

- Introduction
- Maintain System Time
- System Logging
- Mail Transfer
- Manage Printers and printing

 ## 📄 Introduction

Two clocks are important in Linux: 
A 'hardware clock' also known as RTC, CMOS or BIOS clock. This is the battery-backed clock that keeps time even when the system  is shutdown. This hardware clock is often a feature of the motherboard and keeps time regardless of if the computer is running or not. 

The second clock is called the 'system clock' or 'kernel clock': 
When a Linux computer boots up, it starts keeping time. We refer to this as a system clock because  it is updated by the operating system.

On most modern Linux systems, system time and hardware time are synchronised to network time, which is implemented by the Network Time Protocol (NTP).
((NTP) is an internet protocol used to synchronize with computer clock time sources in a network)

## Local Versus Universal Time
The system clock is set to Coordinated Universal Time (UTC), which is the local time at Greenwich,
United Kingdom. Usually a user wants to know their local time. Local time is calculated by taking
UTC time and applying an offset based on time zone and Daylight Savings.

### Date

This simple prints the local time.

```sh
date
``` 
There are a number of flags you can add to the date command that will determine how your output will look like.

![alt text](<Screenshot from 2024-11-13 22-17-02.png>)


### Hardware Clock 
A user may run the hwclock command to view the time as maintained on the real-time clock (RTC).

```sh
 sudo hwclock 
 ```
```sh
 sudo hwclock --verbose
  ```
  ![alt text](<Screenshot from 2024-11-13 20-50-30.png>)

### Timedatectl 
timedatectl is a command that can be used to check the general status of time and date, including whether or not network time has been synchronised. 

```sh
 timedatectl
 ```

![alt text](<Screenshot from 2024-11-13 15-47-47.png>)

### Setting timezone using ```timedatectl``` 

The ```/usr/share/zoneinfo``` directory contains information for the different time zones that are possible.

###### steps to setting up time zone without using GUI 

There are subdirectories that contain the names of continents
as well as other symbolic links. It is recommended to find your region’s zoneinfo starting from
your continent.
The contents of
```/etc/localtime``` will be read when Linux needs to determine the local time zone.

```sh 
ln -s /usr/share/zoneinfo/Canada/Eastern /etc/localtime
```

After setting the correct time zone, run

```sh 
 hwclock --systohc
```
After changing system time, it is recommended to also set the hardware clock so that both system
and hardware clocks are synchronised
 
Run 
```sh
  cat /etc/timezone 
 ``` 
to see the new info.


### Setting Date and Time Without timedatectl

Note that ; These are legacy commands and as such we don't use them in modern systems using sytemd 

###### Using date

Date has an option to set the system time. Use --set or -s to set the date and time.

```sh
 sudo date --set="15 Nov 2024 11:11:11"
```
an alternative is to set the time then set the date independently

```sh
date +%Y%m%d -s "20241115"
```

```sh
date +%T -s "13:11:00"
```
```sh
hwclock --systohc
```
systohc means “system clock to hardware clock”.


### 🛠️ Maintaining System Time

The modern world has devised a system where all internet-connected computer systems can be synchronised to reference clocks using what is known as the Network Time Protocol (NTP).
A computer system with NTP will be able to synchronise their system clocks to the time provided by reference clocks. 
some important terms to note when discussing about NTP

###### Offset

This refers to the absolute difference between system time and NTP time. For example, if the sytem clock reads 10:00:04 and NTP reads 9:59:59 then the offset it 5s.

###### Step
This is a significant change in the system time that occurs when the offset between the NTP provider and a consumer is 'greater' than '128ms'.

###### Slew
Slewing refers to the graduall changes made to system time when the offset between system time and NTP is 'less' than 128ms.

###### Insane Time
If the offset between system time and NTP time is greater than 17 minutes, then the system time is considered insane

###### Drift 
This happens when two clocks initially in sync later become out of sync overtime. This is known as a clock drift.

###### Jitter
Jitter refers to the amount of drift since the last time a clock was queried. So if the last NTP sync occurred 17 minutes ago, and the offset between the NTP provider and consumer is 3 milliseconds, then 3 milliseconds is the jitter.

If your Linux distribution uses timedatectl, then by default it implements an SNTP client his is a less complex implementation of network time. 
In this case, SNTP will not work unless timesyncd service is running. An we can verify that it is running with:

```sh
systemctl status systemd-timesyncd
```
![alt text](<Screenshot from 2024-11-14 15-00-40.png>)

The status of timedatectl SNTP synchronisation can be verified using show-timesync:

```sh
timedatectl show-timesync --all
```
![alt text](<Screenshot from 2024-11-14 09-09-25.png>)






