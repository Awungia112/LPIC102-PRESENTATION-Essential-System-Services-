# Managing printers
In Linux, printers are primary managed by a software stack called CUPS. CUPS: Common Unix Printing System; is an open-source printing system designed for Linux and Unix-based operating systems. It provides a framework for managing and controlling printers, allowing users to print documents and images to various devices. CUPS is widely used on many Linux distributions and has become the standard print manager on most popular Linux distros.

## cups installation
To install the cups
```sh 
sudo apt install cups
```
To learn more about cups
```sh 
man cups 
```
To start cups
```sh
systemctl start cups
```
To enable cups to always start on boot
```sh
systemctl enable cups
```
# printer configuration using lpadmin
syntax
```sh
lpadmin [options] [destination] [options]
```
To add a printer
```sh
lpadmin -p <printer_name>
``` 
setting defualt printer
```sh
lpadmin -d <default_printer>
```
deleting  a printer
```sh
lpadmin -x <printer_name>
```
## Printer Queue Management
# viewing queued jobs
List all printers and jobs:
```sh 
lpq
```
To list the status of the specified printer and its queued jobs.
```sh
lpq -P <printer_name>
```
To list only the jobs owned by the specified user
```sh
lpq -U username
```
# submitting print request to a printer
to print to the default printer
```sh
lpr filename
```
prints the file to the specified printer.
```sh
lpr -P printer_name filename
```
sets job options, such as paper size or orientation.
```sh
lpr -o option[=value] filename
```
prints n copies of a file
```sh
lpr -n filename
```

