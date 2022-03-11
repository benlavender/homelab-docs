# <ins>CheatSheet</ins>

## <ins>Command-line reference:</ins>

### PowerShell Management:

```powershell
## Get all commands and functions in a module
PS C:\>Get-Command -Module ModuleName
```

### Text streams and redirection:

```bash
# Finds the string 'ben' in the /etc/passwd file
grep 'ben' /etc/passwd 
```
```bash
# Matches in-sensitively
grep -i 
```
```bash
# Reverses the match result
grep -v 
```
```bash
# Uses extensive searches
grep -E 
```
```bash
# Use multiple search patterns
grep -e 
```
```bash
# Grep for multiple strings in output:
cmd | grep -iE 'string1|string2'
```

### Special characters:

. Any single character, or * any number of characters

[] = Match ANY single character included within the brackets. 

? = Match the preceding element zero or one times. 

"+" = Match the preceding element one or more times (un-quote).

"*" = Match the preceding element zero or more times (un-quote) . 

^ = Match the beginning of the line.

$ = Match the end of the line.

">" = (un-quote) redirect stdout to... 

">>" = (un-quote) append stdout to...

2> = redirect stderr to...

&> = redirect both stdout and stderr to...

/dev/null = Output will be discarded, such as for example 2> /dev/null will discard error messages.

```bash
# Displays diffstats like comparison of two files:
diff file1 file2
```
```bash
# Displays charachter infomation of a text file:
wc file
```
```bash
# Displays the number of lines in that file:
wc -l file
```
```bash
# Displays the number of words in that file:
wc -w filename 
```
```bash
# Displays the number of chars in that file:
wc -c file
```
```bash
# Search and then change contents of a file using sed:
# Multiple commands can be ran with ;
# Replaces any string that matches Windows with Linux in opsys then writes this to the new file newopsys.
sed 's/Windows/Linux/' opsys > newopsys 
```
```bash
# Replaces any string that matches Windows, once or more ohn the same line, with Linux in opsys then writes this to the new file newopsys:
sed 's/Windows/Linux/g' opsys > newopsys 
```
```bash
# Replaces the text in the specified files. At default sed doesn't change anything:
sed -i 's/Windows/Linux/g' opsys 
```
```bash
# Removes any empty lines from the file windows:
sed -i '/^$/ d' file 
```
```bash                    
# Removes any lines beginning with '#':
sed -i 's/#.*//g' file
```
```bash
# Removes all whitespace from the file windows:
sed -i 's/\s*//g' file 
```

### awk:

Used for searching and editing text files in pro grammatical ways line by line. awk '/search_pattern/ { action_to_take_on_matches; another_action; }' file_to_parse. For example: awk '/search_pattern/ { action_to_take_on_matches; another_action; }' file_to_parse:**

```bash
# Prints the file contents:
awk '{print}' filename 
```
```bash
# Prints any matches of Linux:      
awk '/Linux/' filename 
```
```bash
# Prints all results in the first column that match Linux:
awk '/linux/ {print $1;}' windows 
```
```bash
# Prints all results that match Linux:
awk '/linux/ {print $0;}' windows 
```
```bash
# Changes the FS variable (Field Separator) to : and displays the first column:
awk 'BEGIN {FS=":";} {print $1;}' /etc/passwd 
```
```bash
# Changes the FS variable (Field Separator) to : and displays the second column:
awk 'BEGIN {FS=":";} {print $2;}' /etc/passwd
```
```bash
# Prints Hello to the terminal.
awk 'BEGIN {print "Hello";}'
```

#### awk variables:

FILENAME: References the current input file.

FNR: References the number of the current record relative to the current input file. For instance, if you have two input files, this would tell you the record number of each file instead of as a total.

FS: The current field separator used to denote each field in a record. By default, this is set to whitespace.

NF: The number of fields in the current record.

NR: The number of the current record.

OFS: The field separator for the outputted data. By default, this is set to whitespace.

ORS: The record separator for the outputted data. By default, this is a newline character.

RS: The record separator used to distinguish separate records in the input file. By default, this is a newline character.

#### awk actions:

BEGIN = Process once at the beginning of the command before the first input is read.

END = Process once at the termination of the command.

### Text editors:

#### Vi:

Default mode is command mode, then use i for intert mode.

##### Insert mode:

###### write
:w  

###### write and overrite:               
:w!

###### Write and exit:
:wq 

###### Write, overrite and exit:
:wq! 

###### Quit:
:q 

###### Abandon  changes:
:q! 
                
##### Command mode:

###### Forward searches:
/

###### Reverse searches:
?

###### Move left:
h

###### Move up:
j

###### Move Down:  
k 

###### Move right:
l 

###### Deletes words:
dw 

###### Deletes current line:
dd
 
###### Undo
u

###### Copies current line:
yy

###### Paste buffer 
p

###### Move to last line
G

## <ins>OS Management:</ins>

```powershell
# Get windows updates from remote machine:
Get-HotFix -ComputerName <target> 
```
```bat
REM Get windows updates locally using wmic:
wmic qfe
```
```bat
REM Get windows updates from remote machine:
wmic /node:<hostname> qfe
```
```bat
REM Query scheduled tasks:
schtasks /query
```
```bat
REM Query scheduled task from top level:
schtasks /query /TN "taskname" /FO LIST /v
```
```bat
REM Query scheduled task from folder (see "schtasks /query" for folder paths):
schtasks /query /TN "\folder\taskname" /FO LIST /v
```
```bat
REM Add schtask that calls a .ps1 script
schtasks /create /TN "Manual Updates" /TR powershell.exe -command Invoke-Expression "& 'X:\Projects\vDCS\Infrastructure\VDCS Public DNS Change\Shells\Nano-Updates.ps1'" /SC DAILY /ST 20:00 /RU bvdcs-lap\ben /RP /NP 
REM or
schtasks /create /TN "Manual Updates" /TR "powershell.exe -command Set-Location C:\Shells;Invoke-Expression .\Nano-Updates.ps1"  /SC DAILY /ST 20:00 /RU administrator /RP <PASSWORD / "*"> /NP
```
```powershell
# Install Windows optional feature
Enable-WindowsOptionalFeature -Online -FeatureName <feature_name>
```
```powershell
# Install Windows capability (UWP, MSIX, Appx):
Enable-WindowsCapability -Online -FeatureName <feature_name>
```
```bash
# /sbin/sysctl updates kernel settings in /proc/sys directory 
# Edit /etc/sysctl.conf and run sysctl -p to persist settings
sysctl -p
```
```bash
# Changes the correct files when changing the system hostname@
hostnamectl set-hostname <new.name> 
```
```bash
# Repeat commands:
watch -n 5 "curl 'localhost:9200/_cat/indices?v' | grep -i "logstash-2015.07.15""  
```
```bash
# Crontab for specific user:
crontab -u <user> -e
```
```bash
# Change swap value, (where =value = last amount of ram % will begin swapping):
sysctl vm.swappiness=10
sysctl -p
```
```bash
# List available keymaps
localectl list-keymaps
# or 
ls -l /usr/share/kbd/keymaps
```
```bash
# Show currently configured keymaps for both xorg and console:
loadectl status
```
```bash
# Load keys for the current console only:
loadkeys <keymap>
```
```bash
# Retreive BIOS information from SMBIOS
dmidecode --type bios
```
```bash
# Retreive system information, such as serial number 
dmidecode --type system
```
```bash
# Get locale information:
localectl status
```
```bash
# Change system locale:
# Generate the locales if needed
# Uncomment the line from /etc/locale.gen, i.e en_GB.UTF-8 UTF-8
locale-gen
localectl set-locale LANG=en_GB.UTF-8
```

### Service control:

```bash
# Show all systemd unit files:
systemctl list-unit-files
```
```bash
# Show systemd service unit:
systemctl show <service.service>
```
```bash
# Show the contents of the service unit file:
systemctl cat <service.service>
```
```bash
# List systemd service dependencies:
systemctl list-dependencies <service.service>
```

### Package Management:

```bash
# RPM local installs:
rpm -ivh file.rpm
# or
yum localinstall package.rpm
```
```bash
# APT local installs:
dpkg -i package.deb
```
```bash
# Pacman refresh and update packages
pacman -Syu --noconfirm
```
```bash
# Install package
pacman -S <packagename>
```
```bash
# Remove an installed package, use -Rs to remove depends:
pacman -R <packagename> 
```
```bash
# Query packages:
pacman -Ss <packagename>
```
```bash
# View package group members:
pacman -Sg <package>
```
```bash
# Query package information:
pacman -Si <package>
```
```bash
# Query installed packages:
pacman -Q <packagename>
```
```bash
# Query installed package with info:
pacman -Qi <packagename>
```
```bash
# Query orphaned packages that no longer are depends:
pacman -Qd
# To remove them:
pacman -Rns $(pacman -Qtdq)
```
```bash
# Pacman info and operation verbosity, add --debug to any operation:
pacman --verbose
```

### WinEvents:

```powershell
# List winevent logs:
Get-WinEvent -ListLog *
```
```powershell
# Get Security logs
Get-WinEvent -LogName Security
```
```powershell
# Get last 10 securtity logs (oldest):
Get-WinEvent -LogName Security -Last 10
```
```powershell
# Get winevents from file:
Get-WinEvent -Path <path\to\.evtx>
```
```powershell
# Get winevents by time frame from specific logs
Get-WinEvent -FilterHashTable @{LogName='Application';StartTime='19/10/2020 00:00:00';EndTime='19/10/2020 02:00:00'}
```
```powershell
# Get all winevents from specific time frame. This command omits stderror, remove -EA if need be:
Get-WinEvent -ListLog * -EA SilentlyContinue | ForEach-Object -Process {Get-WinEvent -EA SilentlyContinue @{LogName=$_.LogName;StartTime='19/10/2020 18:00:00';EndTime='19/10/2020 19:00:00'}}
```
```powershell
# Select a string from an event in the security log:
Get-WinEvent -LogName Security | Where-Object -Property {$_.Message -Like '*<string>*'}
```
```powershell
# Select a string from all events in all logs available. This command omits stderror, remove -EA if need be:
Get-WinEvent -ListLog * -EA SilentlyContinue | ForEach-Object -Process {Get-WinEvent -EA SilentlyContinue -LogName $_.LogName} | Where-Object -Property Message -Like '*<string>*'
```
```powershell
# Using Get-EventLog, Get-EventLog uses a Win32 API that is deprecated. The results may not be accurate. Use the Get-WinEvent cmdlet instead.
Get-EventLog -ComputerName <host> <logname> | select timegenerated,message | Select-Sring <string>
```

### Files and Directories:

```powershell
# Get files and directories of a specific size:
Get-ChildItem <dir> -Recurse | Where-Object -Property Length -gt 100MB | Select-Object -Property Name,length,VersionInfo
```
```bash
# Find strings in all files:
find . -type f -exec grep -l "string" {} +
# or
grep -R "string" /
``` 
```powershell
# Get top x10 largest files:
Get-ChildItem <dir> -Recurse -File | Sort-Object -Property Length -Descending | Select-Object -First 10 -Property Length,VersionInfo
```
```powershell 
# Get item properties and attributes:
Get-ItemProperty -Path 'file/dir' | Format-List -Property *
```
```powershell
# List files created and modified today, in descending order by lastwritetime and include the name, lwt and size:
Get-ChildItem -Path <dir> -Recurse | Where-Object {$_.LastWriteTime.date -eq (Get-Date).Date} | Sort-Object LastWriteTime -Descending | Select-Object -Property Name,LastWriteTime,Length | Format-Table -AutoSize
```
```bash
# Find files that have changed in last x amount of time, where {#} = minutes since last change:
find . -cmin -{#}
```
```bat
REM Robocopy mirror sync:
ROBOCOPY <src> <dst> /MIR /Z /W:5 /R:5
```
```bat
REM XCOPY all, including attribs, hidden and system files, overwrites read-only files, surpresses errors and prompts and assumes target is a dir:
xcopy <source_dir> <dest> /f/h/i/c/k/e/r/y
```
```powershell
# Measure directory sizes:
(Get-ChildItem -Recurse -Path <path\dir> | Measure-Object -Property Length -Sum).Sum
```
```bat
REM Measure directory sizes:
dir <dir> /s /A-D /D
```
```bash
# Creates a hard link in the local directory:
ln /etc/samba/smb.conf smb.conf 
```
```bat
REM Creates a hard link between link and target:
mklink /H Link Target
```
```bat
REM Creates a directory junction (part of Windows reparse points):
mklink /J linkdir targetdir 
```
```bash
# Creates a soft link in the local directory:
ln -s /etc/samba/smb.conf smb.conf 
```
```powershell
# Creates symbolic link between directory and target:
New-Item -Path <source\dir> -ItemType SymbolicLink -Value <target\dir>
```
```bat
REM Creates a soft link, omit /D for default soft link:
mklink /D Link Target
```
```bash
# Creates the following directories and automatically creates the folder structure@
mkdir -p test1/test/2/test3 
```
```bat
REM List symlinks:
dir /AL
```
```bash
# Removes the following directories and automatically removes the folder structure, if empty:
rmdir -p test1/test/2/test3 
```
```bash
# Removes the following directories and automatically removes the folder structure:
rmdir --ignore-fail-on-non-empty test1/test/2/test3 
```
```bash
# Removes all files and directories in current directory except non-empty ones:
rm -R -- */
```
```bash
# Adds user permissions to execute:
chmod u+x file
```
```bash
# Adds other users to execute the file:
chmod o+x myfile 
```   
```bash                
# Removes w permissions from the group owner and everyone else:         
chmod go-w myfile 
```
```bash
# Explicitly sets the group permissions as is. Adds or removes any non-conforming permissions:
chmod g=wr myfile 
```
```bash
# Executable all users:
chmod +x myfile 
```
```bash
# Sets the SUID along with RWXRWR permissions:
chmod 4764 myfile 
```
```bash
# Sets the GUID along with RWXRWR permissions:        
chmod 2764 myfile 
```
```bash
# Sets the GUID on myfile:   
chmod g+s myfile 
```
```bash
# Sets the sticky bit on the directory mydir:
chmod o+t mydir 
```
```bash
# Provide minimal access to a directory:  
chmod 701 dir 
```
```bash                
# Changes the owner of myfile to the bro user:
chown bro myfile 
```
```bash
# Changes the owner and group user of myfile to user bro and group bros:    
chown bro.bros myfile 
```
```bash
# Displays current umask for user. Default umask values are stored in /etc/profile and /etc/bashrc. Generally umask values for users with IDs above 199 then the umask will be 0002, else 0022. These can be overridden using the file at ~/.bashrc
umask
```
```bash
# Displays both the standard rwx/ugo permissions as well as any ACLs if applied:
getfacl file
```
```bash
# Displays ext4 information on this partition. This should be displayed under Default mount options (Default mount options:    user_xattr acl).
tune2fs -l /dev/sda1
```
```bash
# Displays ext4 information on this partition (SDCARD):
tune2fs -l /dev/mmcblk0p2 
```
```bash
# Remounts the /home partition:
mount -o remount -acl /home 
```
```bash
# Update /etc/fstab to ensure filesystems on partitions like /home are mounted at boot with the ACL:
/dev/sda3   /home   ext4    defaults,acl    1,2
```
```bash
# Remount to take changes:
sudo mount -o remount /home 
```
```bash
# Modify (-m) add permissions on file for the user:      
setfacl -m u:user:rwx file 
```
```bash
# Removes the permissions on the file:
setfacl -x u:user:rwx file 
```
```bash                
# Modify the permissions on for for the group:
setfacl -m g:group:rwx file 
```
```bash
# Removes the permissions on the file for the user:
setfacl -m u:user:- file
```
```bash
# Removes any ACLs on file:
setfacl -b file 
```
```bash
# Actually modifies the primary permissions, potentially dangerous as it does not change ACL...
setfacl -m o:rwx file 
```
```bash
# To find the largest 10 directories:
find . -type f -print0 | xargs -0 du | sort -n | tail -10 | cut -f2 | xargs -I{} du -sh {}
# or
# Only difference is -type {d:f}.
find . -type d -print0 | xargs -0 du | sort -n | tail -10 | cut -f2 | xargs -I{} du -sh {}
# or
du -a /var | sort -n -r | head -n 10 
```
```bash
# Find files/dir with absolute paths:
find `pwd` -name "file"
# or
find / -type d -name "*dir*" -print
```
```bash
# Get directory size:
du -h -c dir
```
```bash
# Extract archives:
tar -zxvf data.tar.gz
# or
tar -zxv data.tar
```

### Process and Memory Management:

```powershell
# Get process commandline: 
Get-CimInstance -ClassName Win32_Process | Where-Object -Property Name -EQ <imagename> | Select-Object -Property CommandLine
```
```powershell
# Get process commandline using PS version below 3.0
Get-WmiObject -ClassName Win32_Process | Where-Object -Property Name -EQ <imagename> | Select-Object -Property CommandLine
```
```bash
#List descriptors of a process:
lsof -p PID
```
```bash
# Create a swapfile and partition (x2 RAM):
fallocate -l 2GB /swapfile
# Set permissions to swapfile
chmod 600 /swapfile
# Format the file
mkswap /swapfile
# Enable swap
swapon /swapfile
# Update /etc/fstab:

```

### Networking:

```powershell
# Get firewall profile associations with interfaces:
Get-NetConnectionProfile
```
```bat
REM Add inbound firewall rule using netsh:
netsh advfirewall firewall add rulle name =<rule_name> dir= IN|OUT action=ALLOW|DENY protocol=TCP|UDP localport=<port#>
```
```bash
# Add new allow_In rule:
iptables -A INPUT -p {UDP|TCP} --dport {port} -j ACCEPT
```
```bash 
# Re-direct rule as it comes in to another on localhost:
iptables -t nat -A PREROUTING -p {TCP|UDP} --destination-port {PORT} -j REDIRECT --to-ports {NEW_PORT}
```
```bash 
# Re-direct PNAT to another host:
iptables -t nat -A PREROUTING -d <current_dst_IP.addr> -p <TCP/UDP> --dport <current_dst.prt> -j DNAT --to-destination <ip.addr:prt>
```
```powershell
# Set DNS client settings/ip addr:
Set-DnsClientServerAddress -InterfaceAlias <int_name> -ServerAddresses <ip,secondip>
```
```powershell
# Get WAN IP:
Invoke-WebRequest -URI http://ipecho.net/plain
```
**or**
```bash
curl -L ipecho.net/plain
```

#### IP command, replaces older commands such as route, ifconfig, netstat and arp. ## ## These are normally temporary and won't survive reboots. 

Comparisons of the nettools and ip commands:

```bash
# Shows link status:
ifconfig 
ip -s link 
```
```bash
# Shows IP addressing information:
ifconfig
ip addr show 
```
```bash
# Shows routing table:
route 
ip route show:              
```
```bash
# arp:
arp
ip neigh 
```
```bash
# netstats:
netstat
ss 
```
```bash
# Shows netstats with flags all, numerical, proto = TCP/UDP, owner process ID:
netstat -antup:
ss -antup
```

#### Further ip commands:

```bash
# Shows link state plus statistics:
ip link -s show 
```
```bash
# Show interface L2 info.   
ip -s link show 
```
```bash
# Downs the NIC:
ip link set dev <device> down 
```
```bash
# Shows IP addressing information:
ip address show 
# or 
ip addr show 
# or 
ip a s 
```
```bash
# Clears all IP addressing info from device:
ip address flush dev <device> 
```
```bash
# Enables promiscuous mode:
ip link set dev <dev> promisc on 
```
```bash
# Changes the MTU size of the interface:
ip link set dev <dev> mtu <#> 
```
```bash
# Shows ARP table entries:
ip neigh
```
```bash
# Displays the routing table and configs, alternative is $netstat -nr:
ip route
# or
ip route show
```

#### States:

STALE = The neighbour entry is valid but suspicious. This option to ip neigh does not change the neighbour state if it was valid and the address is not changed by this command.

REACHABLE = The neighbour entry is valid until the reachability timeout expires.

DELAY = Neighbor entry validation is currently delayed.

```bash
# Activate the /etc/sysconfig/network-scripts script files for configured interfaces:
ifup
# or
ifdown
```
```bash            
# Calls DHCP and updates /etc/resolv.conf. NetworkManager will call dhclient by default.
# Renews DHCP allocations on all interfaces:
dhclient -v 
```
```bash
# Releases all DHCP allocations on all interfaces:
dhclient -r 
```
```bash
# Renews DHCP allocation on eth0 with no release:
dhclient eth0 
```
```bash            
# To view the status of NetworkManager:
nmcli -t -f RUNNING general
```
```bash               
# When updating files in /etc/sysconfig/network-scripts, you need to notify NetworkManager of the chanages using:
mcli con reload
```
```bash
# Loading interface files manually:
nmcli con load /etc/sysconfig/network-scripts/ifcfg-ifname
```
```bash
# Down and up and interface:
nmcli con down eth0
# or
nmcli con up eth0
```
```bash
# Shows all configured connections:            
nmcli con show 
```
```bash                
# Show general status of NetworkManager:
nmcli general status
```
```bash                
# Show device information that NetworkManager is managing:
nmcli dev show [<eth0>]
```
```bash          
# Create a new connection profile for eth0:
nmcli con add con-name <"Eth0-Work"> type <ethernet> ifname <ifname>
```
```bash
# Add static IP address and default gw to interface:
sudo nmcli con mod <"Eth0-Work"> ipv4.addresses <"192.168.20.100/24 192.168.20.1">
```
```bash
# Add static IP address, gateway, multiple DNS server addresses, set the connection to auto connect and the boot proto to manual:
nmcli con mod Eth1-Test ipv4.addresses 192.168.2.20/22 ipv4.gateway 192.168.2.1 ipv4.dns 192.168.1.1 +ipv4.dns 192.168.3.1 connection.autoconnect yes ipv4.method manual
```
```bash
# Remove static IP address from connection:
nmcli con mod <con-name> -ipv4.addresses 192.168.2.20/22
nmcli con up <con-name>
```
```bash
# Bring up a NetworkManager connection:
nmcli con up <con-name>
```
```bash
# Show installed WLAN NICs using iw:
iw dev
# or
iw dev <devname> info
```
```bash
# Show WLAN NIC info and associated essids:
iwconfig <dev>
```
```bash
# Scan for available APs with verbosity:
iwlist <dev> scanning
```
```bash
# Scan for available APs using nmcli
nmcli dev wifi list
```
```bash
# Scan for available APs using wpa_cli, gives more info than nmcli:
wpa_cli scan -i <int>
wpa_cli scan_results -i <int>
```

### Storage:

```bat
REM Show active ISCSI sessions:
iscsicli listtargetportals
```
```bat
REM Disconnect an ISCSI session:
iscsicli RemoveTargetPortal <address> <socket> ROOT\ISCSIPRT\0000_0 <port_number>
```
```powershell
# Map ISCSI targets
New-IscsiTargetPortal -TargetPortalAddress 172.16.44.3 -InitiatorInstanceName ROOT\ISCSIPRT\0000_0 -InitiatorPortalAddress 172.16.44.4 -IsDataDigest $False -IsHeaderDigest $False -TargetPortalPortNumber 3260
Connect-IscsiTarget -NodeAddress iqn.1991-05.com.microsoft:testvaultsan-test-ap-disk-witness-target -InitiatorInstanceName ROOT\ISCSIPRT\0000_0 -InitiatorPortalAddress 172.16.44.4 -IsPersistent $true -TargetPortalAddress 172.16.44.3 -TargetPortalPortNumber 3260
```
```bash
# Create new Linux installation partitions on a new disk with fdisk with a 200M EFI partition:
fisk </dev/disk>
# Convert partition table type to GPT:
g
# Create the EFI partition, select default as 1 and confirm default for first usable sector:
n
# Set the last sector for the EFI partition:
+200M
# Set the partition type as EFI File System (use l for list of types):
t
1
# Create the next partition and select default for both start and end sectors:
n
# Write changes to the disk
w
```
```bash
# Format a partition with the ext4 filesystem:
mkfs.ext4 </dev/partition>
```
```bash
# Format a partition with the FAT32 filesystem
mkfs.fat -F32 </dev/partition>
```
```bash
# Mount file systems such as the previously created EFI and root:
# If first install, create the boot directory:
mkdir /mnt/boot
# Mount the first partition
mount /dev/sda2 /mnt
# Mount the efi partition to /mnt/boot:
mount /dev/sda1 /mnt/boot
```

### O/S Updates:

```powershell
# Windows Server 1709+
# Detect Windows updates that are not installed:
(Invoke-CimMethod -Namespace root/microsoft/windows/windowsupdate -ClassName MSFT_WUOperations -MethodName ScanForUpdates -Arguments @{SearchCriteria="IsInstalled=0"}).Updates
# Install Windows updates that are not installed and reboot immediately:
$Updates = Invoke-CimMethod -Namespace root/microsoft/windows/windowsupdate -ClassName MSFT_WUOperations -MethodName ScanForUpdates -Arguments @{SearchCriteria="IsInstalled=0"}
$UpdatesInstall = Invoke-CimMethod -Namespace root/microsoft/windows/windowsupdate -ClassName MSFT_WUOperations -MethodName InstallUpdates -Arguments @{Updates = $Updates.Updates}
if ($UpdatesInstall.RebootRequired -EQ $True) {Restart-Computer -Force}
# Using the WindowsUpdateProvider module:
# Detect Windows updates that are not installed. Does not include admin hidden or non-AU provided updates:
Start-WUScan -SearchCriteria "IsInstalled=0 AND IsHidden=0 AND IsAssigned=1"
# Install Windows updates that are not installed. Does not include admin hidden or non-AU provided updates and reboot immediately:
Install-WUUpdates -Updates (Start-WUScan -SearchCriteria "IsInstalled=0 AND IsHidden=0 AND IsAssigned=1")
if (Get-WUIsPendingReboot) {Restart-Computer -Force} 
# Download updates only:
Install-WUUpdates -DownloadOnly -Updates (Start-WUScan -SearchCriteria "IsInstalled=0 AND IsHidden=0 AND IsAssigned=1")
```
```bat
REM WinNT 6.3 and below
REM This entirley depends on the AU settings configured on the host machine at HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate or HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WindowsUpdate
REM Immediately detected Windows update settings:
wuauclt /DetectNow /ReportNow
REM Install the 
```

## <ins>Roles:</ins>

### ADDS:

```powershell
# Get number of group members in AD group:
(Get-ADGroupMember -Identity <name|DN> | Measure-Object).Count
```
```powershell
# Count AD computers in domain: 
(Get-ADComputer -Filter * | Measure-Object).Count
```
```powershell
# Expand on MemberOf attribute:
Get-ADUser <> -Properties Memberof | ForEach-Object -Process {$_.MemberOf}
```
```powershell
# Get ACL on AD object, ensure AD psdrive is mounted with (Import-Module ActiveDirectory):
(Get-Acl -Path 'AD:,<DN>').Access | Select-Object -Property AccessControlType,IdentityReference,ActiveDirectoryRights | Format-Table -AutoSize
``` 

### DNS:

```bat
REM Create a new DNS delegation:
dnscmd /recordadd <zone> <sub_domain_name> NS [<authoriative_server_FQDN>]
```
```bat
REM Remove DNS delegation:
dnscmd /nodedelete <zone> <sub_domain_name>
```
```bat
REM Add authoriative server into DNS delegation:
dnscmd /recordadd <domainname> <sub_domain_name> NS <authoriative_server_FQDN>
```
```bat
REM Remove server zone, primary, seconday, forwarder, or stub:
dnscmd <DNS_server> /zonedelete <zonename>
```
```bat
REM Delete authoriative server from DNS delegation:
dnscmd /recorddelete <domainname> <sub_domain_name> NS <authoriative_server_FQDN>
```
```bat
REM Enumerate domains:
dnscmd /enumzones
```
```bat
REM Enumerate records within domain apex:
dnscmd /enumrecords <zonename> .
```
```bat
REM Enumerate records in delegated DNS domain:
dnscmd /enumrecords <zonename> <childdomain> /type NS
```
```bat
REM Change DNS scavenging periods:
dnscmd <DNS_server> /config /ScavengingInterval <value#>
```
```powershell
# Disable root hints:
Set-DnsServerForwarder -UseRootHints $False
```
```bat
REM or
dnscmd <DNS_server> /config isslave 1
```
```bat
REM Change DNS server forwarders with a timeout of # seconds:
dnscmd <dns_server> /resetforwarders <ip.addr> <ip.addr> /timeout <seconds#> /noslave
```
```bat
REM Add DS-intergrated PTR zone:
dnscmd /ZoneAdd 105.30.172.in-addr.arpa /DsPrimary
```
```bat
REM Add file-backed primary zone:
dnscmd /ZoneAdd <zonename> /Primary /file <domainname.dns> 
```
```bat
REM Add PTR record of 172.30.44.22:
dnscmd /recordadd 44.30.172.in-addr.arpa 22 PTR <qualified_name>
```
```powershell
# Disable XFR:
Set-DnsServerPrimaryZone -Name <zonename> -SecureSecondaries NoTransfer
```
```bat
REM or 
dnscmd /zoneresetsecondaries <zonename> /noxfr
```
```bat
REM Move legacy Windows 2000 DS-intergated zones to at least server 2003:
dnscmd /zoneresettype <zonename> /overwrite_ds /directorypartition <DomainDNS.NamingContext>
```
```bat
REM Add an A record:
dnscmd /RecordAdd <zonename> <QNAME> <QTYPE> <Rdata>
REM Add an A record as test:
dnscmd /RecordAdd <zonename> test A <Rdata>
```
```bat
REM Delete an A record:
dnscmd /RecordDelete <zonename> <QNAME> <QTYPE>
```
```bat
REM Add a CNAME record as test to test.com:
dnscmd /RecordAdd <zonename> test CNAME test.com
```

### Storage Replica:

```powershell
# Enable Storage Replica (Syncronous Topology: Server to Server):
# Install Storage-Replica features:
Install-WindowsFeature Storage-Replica -IncludeAllSubFeature -IncludeManagementTools -Restart
# Clear any existing reps:
Get-srpartnership | Remove-SRPartnership;Get-SRGroup | % { Remove-SRGroup -Name $_.name }
# Create new SR partnership:
New-SRPartnership -SourceComputerName {nb/ip.addr} -SourceRGName {NB name} -SourceVolumeName {mount point} -SourceLogVolumeName {mount point} -DestinationComputerName {nb/ip.addr} -DestinationRGName {NB name} -DestinationVolumeName {mount point (shold be same as source} -destinationlogvolumename {mount point} -LogSizeInBytes 1gb -ReplicationMode <mode>
```

### Virtualisation and Hypervisors:

#### Hyper-V:

```powershell
# VMnetwork Adapter VM Queing Weight (default was 100):
Set-VMNetworkAdapter -ManagementOS -Name <switch_name> -VMQWeight 0 
```
```powershell
# Create a new Hyper-V VM:
New-VM -Name <hostname> -MemoryStartupBytes <bytes> -Generation <1|2> -NewVHDPath 'path\to\.vhdx' -NewVHDSizeBytes <bytes> -SwitchName <switchname> -Path <path\to\vmfiles>
```
```powershell
# Create a DVD device and attach the .ISO to the VM:
Add-VMDvdDrive -VMName <VMname> -ControllerLocation <#> -ControllerNumber <#> -Path <path\to\.iso>
```
```powershell
# Disable dynamic memory:
Set-VM -Name <VMname> -StaticMemory
```
```powershell
# Mount an ISO and configure it for boot:
$VMDVDDrive = Get-VMDvdDrive -VMName <vmname>
Set-VMFirmware -VMName <vmname> -EnableSecureBoot Off -FirstBootDevice $VMDVDDrive
```

#### KVM-quemu / libvirt:

```bash
# Install KVM using RHEL yum groups:
yum group install 'Virtualization Host' 'Virtualization Client'
```
```bash
# Confirm module loaded:
cat /proc/cpuinfo | grep -E 'vmx|svm'
# or
lsmod | grep kvm
```

#### virsh:

```bash
# Create new domain using virt-install:
virt-install --name=tester1.example.com --ram=1024 --vcpus=2 --disk=/var/lib/libvirt/images/test1.example.com.img,size=16 --graphics=spice --location=ftp://192.168.1 22.1/pub/inst --os-type=Linux --os-variant=rhel7
```
```bash
# Using an ISO for O/S deployment:
virt-install --name=tester1.example.com --ram=1024 --vcpus=2 --disk=/var/lib/libvirt/images/test1.example.com.img,size=10 --location=/var/lib/libvirt/images/rhel-server-7.6-x86_64-dvd.iso --graphics=spice --os-type=Linux --os-variant=rhel7 
```
```bash
# Don't wait for O/S installation:
virt-install --name=tester1.example.com --ram=1024 --vcpus=2 --disk=/var/lib/libvirt/images/test1.example.com.img,size=10 --location=/var/lib/libvirt/images/rhel-server-7.6-x86_64-dvd.iso --graphics=spice --os-type=Linux --os-variant=rhel7 --noautoconsole --initrd-inject can be used with URL of .KS file.
```
```bash
# Shutdown a domain gracefully:
virsh shutdown <domain_name>
```
```bash
# Terminate domain session:
virsh destroy 'domain_name' --graceful
# Non-gracefull:
virsh destroy 'domain_name'
```
```bash
# Remove domain but leave storage:
virsh undefine tester1.example.com
```
```bash
# Remove domain and associated storage
virsh undefine tester1.example.com --remove-all-storage
```
```bash
# Using extra arguments:
# Specifying the kirkstart URI:
virt-install --name=tester1.example.com --ram=1024 --vcpus=2 --disk=/var/lib/libvirt/images/test1.example.com.img,size=10 --location=/var/lib/libvirt/images/rhel-server-7.6-x86_64-dvd.iso --graphics=spice --os-type=Linux --os-variant=rhel7 --extra-args='ks=http://myserver/my.ks'
```
```bash
# Enable / disable domain autostart at host boot:
virsh autostart <domain_name>
virsh autostart <domain_name> --disable
```

#### DFS-N/R:

```powershell
# DFS-R backlog:
dfsrdiag backlog /rgname:"rep group name" /rfname:"volume/directory folder" /smem:hostname /rmem:hostname
```

#### Windows Nano server:

```powershell
# Provisioning Nanos Servers. 
# Interface number, setupUI, driver paths are examples only and may not be required for Hyper-v etc:
New-NanoServerImage -MediaPath <'path or label'> -Edition <'Standard|Datacenter'> -DeploymentType <Guest|host> -TargetPath <'path_to_.vhdx'> -MaxSize <uint64> -EnableRemoteManagementPort -InterfaceNameOrIndex '1' -Ipv4Address <'IP.addr'> -Ipv4Dns <'IP_addr'> -Ipv4SubnetMask <'SNM'> -Ipv4Gateway <'IP.addr'> -SetupUI ('NanoServer.DNS') -DriverPath ('C:\temp\Drivers\pvscsi\Win8\pvscsi.inf', 'C:\temp\Drivers\pvscsi\Vista\pvscsi.inf', 'C:\temp\Drivers\vmxnet3\NDIS6\vmxnet3.inf') -ComputerName <'hostname'> -SetupCompleteCommand ('tzutil.exe /s "GMT Standard Time"') -LogPath '\\\\\\NanoServerImageBuilder\Logs\2017-05-08 16-20'
```
```powershell
# Installing roles on nano:
Install-PackageProvider -Name <nanoserverpackage>
```
```powershell
# Find nano server package 
Find-NanoServerPackage
```
```powershell
# Install nano server package
Install-NanoServerPackage <pacakge_name> -Culture en-us
```

## Debugging:

### Windbg / kd:
 
###### Display function table (use knL to see return address):

.fnent {address}
 
###### List nearest symbols (use .fnent to get chained being address):

ln nt+{beging address}
  
###### List module info:

lmvm {module_name}

###### Update symbol path:

.sympath SRV*<lcd>*http://msdl.microsoft.com/download/symbols
.reload

###### Verbose symbol info:

!sym noisy
 
### Live Debugging:

###### List all process on system

.tlist

###### ist user-mode process info on attached or .dmp:

!peb
 
###### Analyze X86 process on X64 system:

.load wow64exts
.effmach x86

### DotNET:

###### Keep debuggee running after CLR exceptions found:

sxe -c "!pe;!clrstack;gc" clr

### Dump files (no kd):

###### List processes in .dmp

!dml_proc
 
###### Get info on loaded .dmp file:

!DumpInfo or !Di
 
###### List all logon sessions:

!logonsession 0 or !kdexts.logonsession 0
 
###### Show specific logon session:

!logonsession <LUID>
 
###### View various system info:

!sysinfo
 
###### Kernel info:

vertarget

###### Display data strucutres. Use -r to recurse substructures 

dt <structure*>

## Applications:

### Exchange On-Premise:

```powershell
# Create multiple distributions lists from .csv. Can actually be used for anything else:
# Create CSV file with attributes on one line:
# Name SamAccountName
# name1 sam1
# name2 sam2
Import-CSV -Path <\path.csv> | ForEach-Object -Process {New-DistributionGroup -Name $_.Name -SamAccountName $_.SamAccountName 
-Type $_.Type -Alias $_.Alias -DisplayName $_.DisplayName -OrganizationalUnit $_.OrganizationalUnit -ManagedBy $_.ManagedBy}
```
```powershell
# List Public Folders:
Get-PublicFolder -Identity '<\:Path>' -Recurse | Select-Object -Property Identity | Select-String "string"
```
```powershell
## Search Public Folders
PS C:\>Get-PublicFolder -Identity '<\:Path>' -Recurse | Select-Object -Property Identity | Select-String "string
```
```powershell
# List Public Folder access:
Get-PublicFolderClientPermission -Identity <path> -Server <servername>
```

Event IDs and further info are at:

https://docs.microsoft.com/en-us/exchange/mail-flow/transport-logs/message-tracking?view=exchserver-2019#event-types-in-the-message-tracking-log

```powershell
# Get Message Tracking Log deliver to mailbox:
Get-MessageTrackingLog -Sender <smtp> -EventID 'DELIVER' -Start "xx/xx/xxxx xx:xx:xx"
```
```powershell
# Get Message Tracking Log received by smtp connector:
Get-MessageTrackingLog -Sender <smtp> -EventID 'RECEIVE' -Start "xx/xx/xxxx xx:xx:xx"
```
```powershell
# Get all remoteipranges of an exchange server specific receive connector:
Get-ReceiveConnector <r/c name> | Select-Object -Expand remoteipranges | Select-Object -Property lowerbound,upperbound
```
```powershell
##View ActiveSync devices info:
$UserList = Get-CASMailbox -Filter {hasactivesyncdevicepartnership -eq $true -and -not displayname -like "CAS_{*"} | Get-Mailbox;$UserList | foreach { Get-ActiveSyncDeviceStatistics -Mailbox $_} | Out-GridView
```

### VMware and PowerCLI:

```powershell
# PowerCLI Connect to datacenter:
Connect-VIServer -Server <hostname,...> -Credential <PSCredential>
```
```powershell
# PowerCLI get all VM snapshots on the DC:
Get-VM | Get-Snapshot | Select-Object -Property Name,VM | Format-List
```
```powershell
# PowerCLI get all VM snapshots on the DC:
Get-VM -VM <vmname>| Get-Snapshot | Select-Object -Property Name,VM | Format-List
```
```bash
#Install VMware tools on Linux from media:
yum -y install perl
mkdir /mnt/cdrom
mount /dev/cdrom /mnt/cdrom
cp /mnt/cdrom/VMwareTools-*.tar.gz /tmp
umount /mnt/cdrom
tar -zxf /tmp/VMwareTools-*.tar.gz -C /tmp
cd /
./tmp/vmware-tools-distrib/vmware-install.pl --default
rm -f /tmp/VMwareTools-*.tar.gz
rm -rf /tmp/vmware-tools-distrib
```

### Elasticsearch commands using curl:
 
```bash
# Get running elasticsearch config settings (this will also give node name):
curl -s "localhost:9200/_nodes/_local/settings?pretty=true"
```
```bash 
# List all indexes and info:
curl 'localhost:9200/_cat/indices?v'
```
```bash 
# Elasticsearch Cluster status:
curl -XGET 'http://localhost:9200/_cluster/health?pretty=true'
```
```bash 
# View shard/index status:
curl -XGET http://localhost:9200/_cat/shards
```
```bash 
# Get all un-allocated primary shards:
curl -XGET http://localhost:9200/_cat/shards | grep -i "p UNASSIGNED"
```
```bash
# Get cluster name:
curl -XGET 'http://localhost:9200/_nodes' | grep -i "cluster_name"
```
```bash
# Remove an index:
curl -XDELETE 'http://localhost:9200/<index_name>/'
```
```bash 
#List templates:
curl -XGET localhost:9200/_template/
```
```bash
# Delete template:
curl -XDELETE localhost:9200/_template/<template_name>
```
```bash 
# Count documents in indice, remove <induce_name> for all documents:
curl localhost :9200/_cat/count/<indice_name>
```

### MySQL:

```bash
# Enter CLI:
mysql -u {account} -p
```
```bash 
# List DBs:
show databases;
``` 
```bash
# List users:
SELECT User FROM mysql.user;
```
```bash
# Get update timestamps on table:
show table status from {db_name} like '{table_name}';
```
```bash
# Show grants per use:
show grants for {account}@localhost;
```
```bash 
# Give grants:
grant all privileges on {db}.* to {user}@localhost identified by 'pass';
``` 
```bash
# You can view logs at /var/log/mysql/error.log and /var/log/mysql/mysql.log
tail -f /var/log/mysql/mysql.log
```
```bash
# Turn on logging by editting /etc/mysql/my.cnf
general_log_file        = /var/log/mysql/mysql.log
general_log             = 1
```
```bash 
# Remove user:
DROP USER 'username'@'localhost';
```
```bash 
# Set root password (unset):
UPDATE mysql.user SET password = PASSWORD('goes_here') WHERE user = "root"; flush privileges;
``` 

### PostgreSQL:

```bash
# Postgres dump DB to .sql:
pg_dump -U <username> <DB_name> > dir\db.sql
```

### MS-SQL:

```bat
REM Query all connections to SQL server and extra information from sp_who
select * from sys.dm_exec_connections
exec sp_who
```
```bat
REM View AG listen IP
select * from sys.availability_group_listener_ip_addresses
```

### SFTP:

```bash 
# Make a new user part of the sftpusers group and place in the chroot jail:
usermod -g sftpusers -d /incoming -s /sbin/nologin {jail}
```

### System Center Configuration Manager:

```powershell
# Get CCM device:
Get-CMDevice -Name <deviceID>
```

### PsExec:

```bat
REM Execute Powershell command via psexec:
psexec \\<target> cmd /c "echo . | powershell <command>"
```

### HTTP Manipulation:

```bash
# curl allow insecure server connections when using SSL
curl -k <URI>
```
```bash
# Curl output file to location:
curl -L -o <destination.filetype> URI
```
```powershelll
# or
Invoke-WebRequest -Uri -OutFile 
```
```bash
# Curl Display only headers from HTTP response:
curl -L --HEAD <URI>
```
```powershell
# or
PS C:\>(Invoke-WebRequest -Uri <URI>).Headers
```
```bash
# Curl don't recurse redirects:
curl <URI>
```
```powershell
# or
Invoke-WebRequest -Uri -MaximumRedirection <#>
```
```bash
# curl step through redirects:
curl -L --max-redirs <#> <URI>
```
```powershell
# Query the VRM lookup API for DVLA and DVSA vehicle data (change the REG...):
Invoke-RestMethod -Uri 'https://vsapp.vehiclesmart.com/rest/vehicleData?reg=<registration>&appid=vs5Dszb7SzN15JlKv71QxGv-aq1VcK6G20-S9v4hbdsb5' -Method GET | ConvertTo-Json
```

### Certificates, OpenSSL and CryptoAPI:

```bash
# Conversations with OpenSSL
# Create PKCS12 archive from a private key and signed PEM certificate:
openssl pkcs12 -in certificate.pem -inkey private.key -export -out certificate.pfx
# Convert from PKCS#12 to PEM format and export private key to a combined PEM file:
openssl pkcs12 -in certificate.pfx -out certificate.pem -nodes
# Convert from PKCS#12 to PEM format and export unencrypted private key only:
openssl pkcs12 -in certificate.pfx -nocerts -out private.key -nodes
# Convert from PKCS#12 to PEM format but do no export the private key:
openssl pkcs12 -in certificate.pfx -nokeys -out certificate.pem 
# Convert a signed PEM (Base64) certificate to DER (binary) format:
openssl x509 -in certificate.pem -out certificate.der -outform DER
# Convert a signed DER (binary) certificate to PEM (Base64) format:
openssl x509 -inform DER -in certificate.der -out certificate.pem
# Create PKCS7 P7B archive archive from a PKCS12 archive, via PEM conversation without importing a CRL file:
openssl pkcs12 -in certificate.pfx -out certificate.pem -nodes
openssl crl2pkcs7 -certfile certificate.pem -out certificate.p7b -nocrl
```
```bash
# Reading certificates with OpenSSL
# PKCS12
# Read the contents, including the chain and keys from a PKCS12 PFX file:
openssl pkcs12 -in certificate.pfx
# Without the private key:
openssl pkcs12 -in certificate.pfx -nokeys
# Without the private key, CA certs and only the client certs:
openssl pkcs12 -in certificate.pfx -nokeys -clcerts
# Without the private key and only the CA certs:
openssl pkcs12 -in certificate.pfx -nokeys -cacerts
# PKCS7
# Read the contents of a P7B file without the cert data:
openssl pkcs7 -in certificate.p7b -print -noout
# X509
# Read the contents of an X509 signed certificate without the cert data:
openssl x509 -in certificate.pem -noout -text
# Display only the fingerprint digest in SHA256 (default is SHA1):
openssl x509 -in certificate.pem -fingerprint -sha256 -noout
# PKCS10 (CSR)
# Read the contents of a CSR file without the cert data:
openssl req -in request.csr -text -noout
# View just the subject of the request:
openssl req -in request.csr -noout -subject
```
```bash
# Generate a new private key (RSA 1024):
openssl genpkey -algorithm rsa -out private.key
# Generate a new private key (RSA 4096):
openssl genpkey -algorithm rsa -out private.key -pkeyopt rsa_keygen_bits:4096
# Generate an encrypted private key with a Triple-DES passphrase:
openssl genpkey -algorithm rsa -out private.key -pkeyopt rsa_keygen_bits:2048 -des3
# Generate an encrypted private key with a AES256 passphrase (generally preferred):
openssl genpkey -algorithm rsa -out private.key -pkeyopt rsa_keygen_bits:2048 -aes256
```
```bash
# Decrypt a private Key:
openssl pkey -in private.key -out private2.key
```
```bash
# Generate a private / public key pair using an existing key:
openssl pkey -in private.key -out public.key -pubout
```
```bash
# Read a private key file (enter passphrase if required):
openssl rsa -in private.key -noout -text
# or:
openssl pkey -in privkey.key -text
# Read a public key:
openssl pkey -in pubkey.key -pubin -text
```
```bash
# Create a CSR based on an existing key:
# Follow the interactive guide, use "." to leave certain fields blank
openssl req -key private.key -new -out website.csr
# Create a CSR with a new private key:
# Follow the interactive guide, use "." to leave certain fields blank:
openssl req -newkey rsa:2048 -keyout private.key -nodes -out website.csr
```
```bash
# Create a self-signed certificate with an existing private key:
# Follow the interactive guide, use "." to leave certain fields blank
openssl req -x509 -new -key private.key -days 365 -out website.pem
# Create a self-signed certificate with a new private key:
# Follow the interactive guide, use "." to leave certain fields blank
openssl req -x509 -new -newkey rsa:2048 -nodes -keyout private.key -days 365 -out website.pem
# Omit the "-nodes" switch to prompt for passphrase.
openssl req -x509 -new -newkey rsa:2048 -keyout private.key -days 365 -out website.pem
```
```bash
# Generate a self signed certificate based on an existing CSR and private key:
openssl x509 -signkey private.key -in request.csr -req -days 365 -out website.cer
```
```bash
# Read and verify an existing CSR:
openssl req -in request.csr -text -noout -verify
```
```bash
# Generte file hashes using dgst:
# SHA256
openssl dgst -sha256 inputfile
# MD5
openssl dgst -md5 inputfile
```
```bash
# Verify private and public key relationship:
# Using an existing private and public key pair, generate a signature using the private key and verify using a file:
openssl dgst -sha1 -sign private.key -out signiture.sig <file>
# Verify against public key:
openssl dgst -sha1 -verify public.key -signature signiture.sig <file>
```
```bash
# Verify a certificate is associated with a known private key
# Confirm the modulus of both the private key and the signed certificate are the same:
openssl rsa -modulus -noout -in private.key
openssl x509 -modulus -noout -in website.cer
# Each modulus string can be verified with dgst for simplicity if required:
openssl rsa -modulus -noout -in private.key | openssl dgst -sha256
openssl x509 -modulus -noout -in website.cer | openssl dgst -sha256
```
```bash
# Sign and encrypt a file for sharing between user A => user B:
# Create a private/public key pair for user A:
openssl genpkey -algorithm rsa -pkeyopt rsa_keygen_bits:2048 -out private-a.key
openssl pkey -in private-a.key -pubout -out public-a.key
# Create a private/public key pair for user B:
openssl genpkey -algorithm rsa -pkeyopt rsa_keygen_bits:2048 -out private-b.key
openssl pkey -in private-b.key -pubout -out public-b.key
# Share the public keys of each user with each other.
# Create a file with some text like "Example text" with user A.
# Encrypt the hash value using the senders private key.
# Generate a hash value of this file:
openssl dgst -sha1 message.txt 
# Encrypt hash value using user A's private key and output the digital signature:
openssl dgst -sha1 -sign private-a.key -out signiture.bin message.txt
# Signature to be sent to user B for verification and decryption with user A's public key.
# Encrypt file with public key of user B:
openssl pkeyutl -encrypt -in message.txt -pubin -inkey public-b.key -out ciphertext.bin
# Cipher text and signiture to be sent to user B.
# As user B:
# Decrypt ciphertext.bin:
openssl pkeyutl -decrypt -in ciphertext.bin -inkey private-b.key -out received-message.txt
# Verify received-message is the same as message.txt, either by hash or content.
# Verify signiture:
openssl dgst -sha1 -verify public-a.key -signature signiture.bin received-message.txt
```
```bash 
# OpenSSL connect to host and display certificate info:
openssl s_client -connect host:443
```
```bash 
# Displays only info on valid dates:
openssl s_client -connect host:443 | openssl x509 -noout -dates
```
```bash
# Connect to StartTLS smtp port (can change protocol):
openssl s_client -connect host:25 -starttls smtp
```
```bat
REM Discover CA/SubCA details of ADCS:
certutil -config - -ping
```
```bat
REM List certificate templates available on ADCS:
certutil -CAtemplates -config CA.FQDN\CA_name
```
```bat
# Open certificates using certutil:
certutil -dump <certificate>
```
```bat
REM Request X509 certificate from an ADCS template using a .INF file:
REM .INI file example:
[NewRequest]
Subject = "CN=,DC=,DC=,DC=company,DC=co,DC=uk"
MachineKeySet = True
KeyLength = 2048
KeySpec = 1
Exportable = True

REM Create the CSR:
certreq -new <PolicyFileIn.inf> <RequestFileOut.req>

REM Submit the CSR to the ADCS instance:
certreq -submit -config <"CA"> -attrib "CertificateTemplate:<Template-Name>" <RequestFileOut.req> <output.cer>

REM If the certificate requires approval, approve the request in ADCS:
```

### SMTP:

```console
# SMTP send (Telnet) telnet to SMTP server on either 25 or 587:
>telnet
Telnet>ehlo
Telnet>mail from:<smtp_addr>
Telnet>rcpt to:<Recipient_smtp>
Telnet>Subject: (exit with .)
Telnet>body (exit with .)
```
```powershell
# or
Send-MailMessage -From <smtp> -To <smtp> -Subject <subject_string> -SmtpServer <smtp_mta>
```
```bash
# Mutt connect to to POP and IMAP mailboxes and send mail.
mutt -f pop://mail@host.com
```
```bash
# Send mail to SMTP address:
mail -s 'Subject' ~/file
```

### Packet capturing:

```bat
REM ETW netsh tracing:
netsh trace start persistent=<yes | no> capture=yes tracefile=<path_to_tracefile.etl>
```
```bat 
REM Stop running ETW trace:
REM Either view in Microsoft Network Monitor, Microsoft Message Analyzer, etl2pcapng to export and use with Wireshark.
netsh trace stop
```
```bat
REM Convert .etl to .pcapng with etl2pcapng:
etl2pcapng <input.etl> <output.pcapng>
```
```bat
REM List interfaces with dumpcap, tshark and tcpdump:
dumpcap -D
tshark -D
```
**or**
```bash
tcpdump -D
```
```bath
REM Capture to console with DNS query display filter:
tshark -i <int#> -Y "dns.flags.opcode == 0"
```
```bat
REM Capture with tshark with the same filter but display only the source IP addresses:
tshark -i <int#> -Y "dns.flags.opcode == 0" -T fields -e ip.src
```
```bat
REM Analyse multiple capture files recursively:
for /F %c in ('dir /b <dir\*.pcapng>') do tshark -r %c
```
```bat
REM Capture with dumpcap to capture file, split a new cap at 1GB with max files of 2:
dumpcap -i <int#> -b filesize:524288 -b files:2 -w <output.pcapng>
```
```bash
# Capture just UDP 514 and verbose:
tcpdump -n UDP 514 -v
```
```bash
# Capture just UDP 514 and TCP|UDP 80 and verbose:
tcpdump -n UDP 514 or port 80 -v
```
```bash
# Capture port 514 UDP|TCP on all interfaces and verbose:
tcpdump -i ANY port 514
```
```bash
# Capture from source IP address:
tcpdump -n src host <ip.addr>
```

## Public Cloud:

### Microsoft Azure:

```bash
# List available VM extensions in the Azure CLI:
az vm extension image list --latest
```
```powershell
# Reset AzureAD user password:
Set-AzureADUserPassword -ObjectId 0fd2daaf-64af-47a7-91c3-d38840eee2c5 -Password (Read-Host -AsSecureString)
```
```bash
# or
# Use --force-change-password-next-login to expire new password
az ad user update --id <objectID> --password <password_string>
```
```bash
# Listing role assignments
# Per scope:
az role assignment list --assignee <objectid> --scope <resourceID>
# Current subscription:
az role assignment list --all --assignee <objectid>
# Without assignee:
az role assignment list --all
```
```powershell
# or
# Current subscription:
Get-AzRoleAssignment
# Per scope:
Get-AzRoleAssignment -ObjectId <objectID> -Scope <resourceID>
# List all assignments for a specific user:
Get-AzRoleAssignment -SignInName <UPN>
```
```powershell
# List AzureAD roles:
Get-AzureADDirectoryRole
```
```powershell
# List deny assignments at a specific resource:
Get-AzDenyAssignment -Scope <ResourceID>
```
```powershell
# List deny assignments at the current subscription:
Get-AzDenyAssignment
```
```powershell
# Grant Reader at subscription scope:
New-AzRoleAssignment -ObjectId <objectId> -RoleDefinitionName 'Reader' -Scope <subscriptionID>
```
```powershell
# Grant owner to a resource:
New-AzRoleAssignment -ObjectId <objectId> -RoleDefinitionName 'Owner' -Scope <ResourceID>
```
```bash
# or
az role assignment create --assignee <assignee> --role --scope 
```
```powershell
# Remove the reader role assignment assigned to user with ID 0fd2daaf-64af-47a7-91c3-d38840eee2c5 from a subscription known as /subscriptions/07b6991f-c067-4870-b1d3-de8663bf38d:
New-AzRoleAssignment -Scope /subscriptions/07b6991f-c067-4870-b1d3-de8663bf38d6 -ObjectId 0fd2daaf-64af-47a7-91c3-d38840eee2c5 -RoleDefinitionName Reader
```
```bash
# Remove the reader role assignment assigned to user with ID 0fd2daaf-64af-47a7-91c3-d38840eee2c5 from a subscription known as /subscriptions/07b6991f-c067-4870-b1d3-de8663bf38d:
az role assignment delete --scope /subscriptions/07b6991f-c067-4870-b1d3-de8663bf38d6 --role Reader --assignee 0fd2daaf-64af-47a7-91c3-d38840eee2c5
```
```powershell
# Gets all custom role definitions in the directory:
Get-AzRoleDefinition -Custom
```
```powershell
# Gets a custom role with the specified ID:
Get-AzRoleDefinition -Id <ID>
```
```powershell
# Gets a custom role by display name:
Get-AzRoleDefinition -Name <'Display Name'>
```
```bash
# Gets all custom role definitions in the directory:
az role definition list --custom
```
```bash
# Gets a custom role by display name:
az role definition list  --name "Billing Reader Plus"
```
```powershell
# Create custom role definition:
# Needs the Az.Resources module
Import-Module -Name Az.Resources
# Define the first role:
$role = New-Object -TypeName Microsoft.Azure.Commands.Resources.Models.Authorization.PSRoleDefinition
$role.Name = 'Virtual Machine Command Injector'
$role.IsCustom = $True
$role.Description = 'Permits commands to be sent to the VM using the Run command operation and viewing of VM resources'
$role.AssignableScopes = '/subscriptions/07b6991f-c067-4870-b1d3-de8663bf38d6/resourceGroups/rg-test-lab-001'
$role.Actions = 'Microsoft.Compute/virtualMachines/runCommand/action'
# Create the first role definition:
New-AzRoleDefinition -Role $role
# Create the second role:
Clear-Variable -Name role
# Define the second role:
$role = New-Object -TypeName Microsoft.Azure.Commands.Resources.Models.Authorization.PSRoleDefinition
$role.Name = 'Virtual Machine Command Reader'
$role.IsCustom = $True
$role.Description = 'Permits reading of Run Commands on virtual machines'
$role.AssignableScopes = '/subscriptions/07b6991f-c067-4870-b1d3-de8663bf38d6'
$role.Actions = 'Microsoft.Compute/virtualMachines/read'
$role.Actions += 'Microsoft.Compute/locations/runCommands/read'
# Create the second role:
New-AzRoleDefinition -Role $role
```