# 108.2 System logging

##### Logs are  a set of records or messages that Linux maintains for the administrators to keep track of important events, and can include  system and network events.Logs can be  useful when troubleshouting, monitoring or just checking program's behaviour.


On linux, logs are handled by three main services:  ```syslog```,```syslog-ng```, ```rsyslog```.
These services collect messsages from other services and programs and stores them in files called log files.

These log files are conventionally kept in the /var/log directory.Though some services take care of their logs like the linux kernel that uses the ring buffer for storing its log messages.

## Log types 
Logs can be roughly classified into system logs and service logs.

Below are some system logs:

#### ```/var/log/auth.log```:
Authentication processes.(logged users, failed login attempts)

#### ```/var/log/kern.log```
Kernel messages.

#### ```/var/log/daemon.log```:
daemons or services running in the background   
#### ```/var/log/utmp``` and ```varlog/wtmp```:
succesful logins.
#### ```/var/log/lastlog```:
timestamp for recent logins
## Reading logs.

Log files can be read as  regular files using the appropriate utilities provided we have the appropriate permissions, since log files are to be managed by the system administrator (the root user). 
Log files can be read and filtered using commands such as: `head`, `tail` `grep`, `less`, `cat`, `zless`,`zgrep`.

>The `tail -f` command is commonly used to monitor log files which are constantly growing. ie it will dynamically show new lines as they are appended.  

There are a few examples in which logs are not text, but binary files which cannot be understood by displaying their content. In scuh cases you must use special commands to be able to read the logs;

#### /var/log/wtmp
Use  ```who```.

#### /var/log/faillog
Use `faillog`.

#### /var/log/lastlog
Use ```lastlog```.

```rsyslog``` configuration file is  ```/etc/rsyslog.conf``` which is divided into three sections: ```MODULES```, ```GLOBAL DIRECTIVES``` and ```RULES```. 
```MODULES``` includes module support for logging, message capability, and UDP/TCP log reception. 

```GLOBAL DIRECTIVES``` allow us to configure a number of things such as logs and log directory
permissions.

```RULES``` is where facilities, priorities and actions come in.
The settings in this section tell the logging
daemon to filter messages according to certain rules and log them or send them.

Each log message is given a facility number and keyword that are associated with the Linux internal subsystem that produces the message: ``kern``, ``user``, ```mail```, ```daemon```, ```auth```,```authpriv```, ```syslog```, ```lpr```, ```news```, ```uucp```,```cron```, ```ftp```, ```ntp```, ```security```, ```console```, 

Then each messsage has  a priority level:
 
| Severity | Keyword | Description |
| -- | -- | -- |
| Emergency | `emerg`, ``panic ``| System is unusable |
| Alert | `alert` | Action must be taken immediately |
| Critical | `crit` | Critical conditions |
| Error | `err`,`error` | Error conditions |
| Warning | `warn`, `warning` | Warning conditions |
| Notice | `notice` | Normal but significant condition |
| Informational | `info` | Informational messages |
| Debug |   `debug` | Debu-level messages |
 

The rule format is as follows: \<facility>.\<priority> \<action> 

The <facility>.<priority> selector filters messages to match. Priority levels are hierarchically
inclusive, which means rsyslog will match messages of the specified priority and higher. The <action> shows what action to take (where to send the log message). Here are a few examples for
clarity:
> auth,authpriv*     /var/log/auth.log

Regardless of their priority (*), all messages from the auth or authpriv facilities will be sent to
/var/log/auth.log.


>mail.err   /var/log/mail.err

Messages from the mail facility with a priority level of error or higher (critical, alert or
emergency) will be sent to /var/log/mail.err.

### The ```logger``` command.
The logger command is used to  enter messages manuallyinto the system log. 
Logger will append any message it receives to /var/log/syslog (which is a log file for practically all logs processed by ```rsyslogd```).


#### Log Rotation Mechanism.

Logs are rotated on a regular basis ie they are reanmed, archived, compressed, and even deleted as they grow old.
The purpose of rotating logs is to prevent logs from occupying excess disk space and eventuallay managing their length to ease consultation.

```logrotate``` is the utility responsible for log rotation,and run as cron job daily through the script  ```/etc/cron.daily/logrotate``` and reads the configuration file ```/etc/logrotate.conf```.

Some configurations as found in the ```/etc/logrotate.conf``` file are:

- rotate 4
- weekly
- compress
- delaycompress



# 2- systemd and journald.

The systemd which is a system and service manager has been adopted by major linux distibutions, and offers utilities for multiple purposes;
one of which is the journal daemon (```systemd-journald```) which is its logging service.

# *```systemd```*

systemd is a service manager in most linux distributions has progressively replaced sys v init as init program.

It also features a logging service called the journal and also manages other programs like crons,ntp.  
systemd operates on units which  any resource that systemd can manage(eg. network,journals).

There are a number of unit types: service,mount, automount, swap, timer, device, socket,path, timer, snapshot, slice, scope and
target.

A target is a special type of unit which resembles the classic runlevels of SysV Init 
>  e.g. graphical.target is similar to runlevel 5

To check the current target in your system,use the systemctl get-default command:
```
systemctl get-default
```
## *The System Journal: systemd-journald*
```systemd-journald``` is the system service which takes care of receiving logging information from
a variety of sources: kernel messages, simple and structured system messages.
It's configuration file is ```/etc/systemd/journald.conf``` and as any other service, we can use ```systemctl```  command to `start` it , `restart` it , `stop` it or check it's `status`;

#### Querying the Journal Content.

```journalctl``` is the utility that you use to query ```systemd``` journal.
If used without options, the ```journalctl``` command prints the entire journal.
More options of ```journalctl``` are as listed below: 

#### ```-r```
   The messages of the journal will be printed in reverse order.

#### ```-f``` 
(much like ```tail -f``` ).
### ```-n``` 
It will print the value most recent lines (if no <value> is specified, it defaults to 10)

### ```-k, --dmesg``` 

#### Navigating and Searching Through the Journal.
> Similar to the navigation in the ```less``` pager.

#### Filtering the Journal Data.

The journal allows you to filter log data by different criteria:
- ##### Boot number
#### ```--list-boots```
It lists all available boots
#### ```-b```, ``--boot``
It shows all messages from the current boot.

- ##### Priority
#### ```-p```
You can also filter by severity/priority with the -p option.
```
journalctl -p err
```
- ##### Time Interval
You can have journalctl print only the messages logged within a specific time frame by using
the --since and --until switches.
for example: 
```
journalctl --since "07:00:00" --until "09:45:00"
```
You can use a slightly different time specification: "integer time-unit ago"
Keywords can also be used like; **yesterday, today, now.**

- ##### Program
To see journal messages related to a specific executable the following syntax is used:
```journalctl /path/to/executable```
- ##### Unit
### ...
##### -u 
It shows messages about a specified unit.

- ##### Fields 

The journal can also be filtered by specific fields through any of the following syntaxes:

- \<field-name>=\<value>
- \_\<field-name>=\<value>_
- \__\<field-name>=\<value>

###### **PRIORITY=**
One of eight possible syslog priority values formatted as a decimal string:
```
journalctl PRIORITY=3
```
Note how you could have achieved the same output by using the command sudo
journalctl -p err that we saw above.

 **_PID=**
 Show messages produced by a specific process ID.

 **BOOT_PID=**
 Based on its boot ID you can single out the messages from a specific boot.

 #### Combining fields.


 #### Manual Entries in the System Journal: systemd-cat.
 Just like how the logger command is used to send messages from the command line to the system
log.
There is also the possibility of specifiying a priority level with the -p option:
```
systemd-cat -p emerg echo "This is not a real emergency."
```

#### Persistent Journal Storage.

The default behaviour is as follows: if /var/log/journal/ does not exist, logs will be saved in a
volatile way to a directory in /run/log/journal/ and — therefore — lost at reboot. The name of
the directory — the /etc/machine-id — is a newline-terminated, hexadecimal, 32-character,
lowercase string;
 e.g.```/run/log/journal/8821e1fdf176445697223244d1dfbd73/```
 If you try to read it with less you will get a warning, so instead use the command journalctl 

The way the journal daemon deals with log storage can be changed after installation by tweaking its configuration file: ```/etc/systemd/journald.conf```. The
key option is Storage= and can have the following values:


-  **Storage=volatile**
Log data will be stored exclusively in memory — under /run/log/journal/. If not present,
the directory will be created.

- **Storage=persistent**
By default log data will be stored on disk — under /var/log/journal/ — with a fallback to
memory (/run/log/journal/) during early boot stages and if the disk is not writable. Both
directories will be created if needed.
- **Storage=none**
All log data will be discarded.

#### Deleting Old Journal Data: Journal Size.
Logs are saved in journal files whose file names end with .journal or .journal~ and are located
in the appropriate directory (/run/log/journal or /var/log/journal as configured).
To check
how much disk space is currently being occupied by journal files (both archived and active), use
the ```--disk-usage``` switch.

size limit enforcement on stored journal files can be managed by tweaking a series of configuration options in ```/etc/systemd/journald.conf```. These options fall into two categories
depending on the filesystem type used: persistent (/var/log/journal) or in-memory
(/run/log/journal).
systemd logs default to a maximum of 10% of the size of the filesystem where they are stored.
| SystemMaxUse | The maximum disk usage  |
| ------    | ------ |
| SystemKeepFree | Keep at least this much free space.(15% default) |
| SystemMaxFileSize | Maximum size of each individual file. (1/8 of SystemMaxUse default) |
| SystemMaxFiles | Max number of non-currently-active files. ( default is 100) |

The equivalent variables for temporal logs are obtained by replacing ```System``` with ```Runtime``` in the ```/etc/systemd/journald.conf```

##### Vacuuming the Journal.
| --vacuum-time= | clean everything older than the precise time |
| ----- | ----- |
|--vavuum-size= | eliminate till logs occupy a specific size |
--vacuum-files= | only keep this much archive files. |

The ```--flush``` option can also be used to flush logs from the ```/run/``` to the ```/var/``` directory to make the logs persistent.

> NOTE: To check the internal consistency of the journal file, use journalctl with the ```--verify``` option.

#### Retrieving Journal Data from a Rescue System.
It  may happen that a system crashes and you need to access journal
files on the hard drive of a faulty machine through a rescue system (a bootable CD or USB key
containing a live Linux distribution).

`journalctl` looks for the journal files in /var/log/journal/\<machine-id>/.
The following options may be used, since the machine-id may be different on the rescue system.

#### ```-D path/to//dir```
This option specifies the directory in which ```journalctl``` searches the journal files.

-m, --merge
It merges entries from all available journals under /var/log/journal.

--file
It will show the entries in a specific file.

#### Forwarding Log Data to a Traditional syslog Daemon.

- Forwarding messages to the socket file /run/systemd/journal/syslog for syslogd to read.

> Likewise, you can forward log messages to other destinations with the following options: ForwardToKMsg (kernel log buffer — kmsg)   
 or
  ForwardToWall (all logged-in users via wall).