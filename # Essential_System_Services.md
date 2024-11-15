# Essential System Services 

- Introduction
- Maintaining System Time
- System Logging
- Mail Transfer
- Manage Printers and printing

 ## 📄 Introduction

Two clocks are important in Linux: 
A 'hardware clock' also known as RTC, CMOS or BIOS clock. This is the battery-backed clock that keeps time even when the system  is shutdown. This hardware clock is often a feature of the motherboard and keeps time regardless of if the computer is running or not. 

The second clock is called the 'system clock' or 'kernel clock': 
When a Linux computer boots up, it starts keeping time. We refer to this as a system clock because  it is updated by the operating system in our case our system makes use of the Unix timestamps.

On most modern Linux systems, system time and hardware time are synchronised to network time, which is implemented by the Network Time Protocol (NTP).
((NTP) is an internet protocol used to synchronize with computer clock time sources in a network)

## Local Versus Universal Time
The system clock is set to Coordinated Universal Time (UTC), which is the local time at Greenwich,
United Kingdom. Usually a user wants to know their local time. Local time is calculated by taking
UTC time and applying an offset based on time zone and Daylight Savings.

### Date

This simply prints the local time.

```sh
date
``` 
There are a number of flags you can add to the date command that will determine how your output will look like for example '-u' will give UTC time , '-I' will give ISO 8601 format and '-R' returns date and time in RFC format.

![alt text](<Screenshot from 2024-11-13 22-17-02.png>) 

The current date time format for Unix systems can be view using the the command 

```sh
date +%s
```
From date's man page we see that '%s' refers to Unix time. Unix time is used internally on most Unix-like systems. It stores UTC time as the number of seconds since Epoch, which has been defined as January 1st, 1970 at midnight. 

We can also format the date and time using the Unix time

 ```sh
 date --date='@1564013011'
 ```

The '--debug' flag analyses the inputed date and its useful for troubleshooting applications that generate time.

```sh
date --debug --date="Fri, 15 Nov 2024 14:00:17 -0500
```

### Hardware Clock 
A user may run the hwclock command to view the time as maintained on the real-time clock (RTC).

```sh
 sudo hwclock 
 ```
```sh
 sudo hwclock --verbose
  ```
  ![alt text](<Screenshot from 2024-11-13 20-50-30.png>) 

  This displays the hardware clockdrift which tells you if the the system time and hardware time are deviating. 

### Timedatectl 
timedatectl is a command that can be used to check the general status of time and date, including whether or not network time has been synchronised. 

```sh
 timedatectl
 ```

![alt text](<Screenshot from 2024-11-13 15-47-47.png>)

### Setting timezone using ```timedatectl``` 

Without any NTP rather than using hwclock and date it is advisable to use timedatectl
First you disable the ntp
```sh
timedatectl set-ntp no
```
```sh
timedatectl list-timezones
```
```sh
timedatectl set-timezone Africa/Douala
```
```sh
timedatectl
```

The ```/usr/share/zoneinfo``` directory contains information for the different time zones that are possible.



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

Then we run 
```sh
hwclock --systohc
```
to synchronize the hardwareclock time and the systemclock 
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

If your Linux distribution uses timedatectl, then by default it implements an SNTP client this is a less complex implementation of network time. 
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


#### NTP Daemon
The system time is compared to network time on a regular schedule. For this to work we must have a daemon running in the background the name of this daemon is 'ntpd'.

To ensure that the ntpd daemon is running we check its status:

```sh
systemctl status ntp
```
We might be required to both start and enable ntpd. On most Linux machines this is accomplished with:

```sh 
systemctl enable ntp && systemctl start ntp
```

#### NTP Configuration
The file /etc/ntp.conf contains configuration information about how your system synchronises with network time. This file can be read and modified using vi or nano.

```sh
nano /etc/ntp.conf
```

####### pool.ntp.org

A network administrator might also consider using (or setting up) a pool. A pool is a dynamic collection of networked computers that volunteer to provide highly accurate time via the Network Time Protocol to clients world wide.

###### ntpdate
In the event where the offset between system and NTP time is greater than 17mins (insane) the ntpd will not make any changes (slewing or step) and the system admin would be required to make manual changes.

- Use ```systemctl stop ntpd``` to stop the runing ntpd
- Next, use ```ntpdate pool.ntp.org``` to perform an initial one-time synchronisation.

###### ntpq
ntpq is a utility for monitoring the status of NTP. Once the NTP daemon has been started and configured.
  
```sh
 ntpq -p
```
The '-p' option is for print and the '-n' will cause the Host addresses to be returned as IP addresses. the command displays a table containing the remote, refid, st, when, poll,reach, delay, offset,jitter.

###### chrony
The chrony is another way to implement NTP. 

```sh
chronyc tracking
```

Finally, chronyc sources will return information about the NTP servers that are used to
synchronise time:
```sh
chronyc sources
```

