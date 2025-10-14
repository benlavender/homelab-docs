# <ins>CheatSheet</ins>

## <ins>Command-line reference:</ins>

### System reference documentation:

#### Man pages:

1 Executable programs or shell commands

2 System calls (functions provided by the kernel)

3 Library calls (functions within program libraries)

4 Special files (usually found in /dev)

5 File formats and conventions, e.g. /etc/passwd

6 Games

7 Miscellaneous (including macro packages and conventions), e.g. man(7), groff(7), man-pages(7)

8 System administration commands (usually only for root)

9 Kernel routines [Non standard]

```bash
# Open a man page by name:
man <name>
# Open a man page by the section:
man <#> <name>
# Search for a man page by its description:
man -k <string>
# or:
apropos <string>
# Update the man database:
sudo mandb
# Show search paths for man pages:
manpath
```

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
# Displays character information of a text file:
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

Default mode is command mode, then use i for insert mode.

##### Insert mode:

###### write
:w  

###### write and overwrite:                              
:w!

###### Write and exit:
:wq 

###### Write, overwrite and exit:
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

### Bootloaders and boot managers:

#### efibootmgr:

> ℹ️ **Note:** Requires EFI firmware support.

```bash
# Show the current EFI boot order (use -v for verbose output):
efibootmgr
```
```bash
# Change the BootOrder value for permanent boot order:
efibootmgr -o <#>,<#>
```
```bash
# Change the BootNext value for next boot only:
efibootmgr -n <#>
```
```bash
# Deactivate a boot option:
efibootmgr -b <#> -A
```
```bash
# Activate a boot option:
efibootmgr -b <#> -a
```
```bash
# Create a new boot option:
efibootmgr -c -d <dev> -p <part#> -L <LABEL> -l <path/to/.efi>
```
```bash
# Delete a boot option:
efibootmgr -b <#> -B
```

### Loginctl:

```bash
# List current sessions:
loginctl list-sessions
```
```bash
# Show login manager properties:
loginctl show-session
# Show properties for a specific session:
loginctl show-session <id>
# Show 
```
```bash
# Activate screen lock for a specific session:
loginctl lock-session <id>
# Activate screen unlock on all supporting sessions:
loginctl lock-sessions
# Deactivate screen lock for a specific session:
loginctl unlock-session <id>
```
```bash
# If no session is specified the current session running the command will be used.
# Terminate an active session:
loginctl terminate-session <id>
# Kill an active session (sends SIGTERM to units):
loginctl kill-session <id>
```
```bash
# List current logged in users:
loginctl list-users
```
```bash
# Show properties of a logged in user:
loginctl show-user <id|name>
```
```bash
# If no user is specified the current user running the command will be used.
# Terminate all sessions for a user:
loginctl terminate-user <id|name>
# Kill all processes of a user (sends SIGTERM):
loginctl kill-user <id|name>
```

### Timeshift: 

> ℹ️ **Note:** Commands usually require elevation.

> ℹ️ **Note:** Commands assume `/etc/timeshift/timeshift.json` is configured.

```bash
# Listing snapshots and devices.
# List devices for backup:
timeshift --list-devices
# List all snapshots:
timeshift --list
# List snapshots for a specific device:
timeshift --list --snapshot-device <dev>
```
```bash
# Backing up with Timeshift.
# Create a snapshot only if scheduled:
timeshift --check
# Create a snapshot only if scheduled on a specific device:
timeshift --check --snapshot-device <dev>
# Create a snapshot:
timeshift --create
# Create a snapshot with a custom description:
timeshift --create --comments '<description>'
```
```bash
# Deleting snapshots.
# Delete a specific snapshot.
# # Follow the interactive guide and select a snapshot:
timeshift --delete
# or
timeshift --delete --snapshot <name>
# Delete all snapshots:
timeshift --delete-all
```
```bash
# Restoring from snapshots.
# Restore from a snapshot.
# Follow the interactive guide and select a snapshot:
timeshift --restore
# Restore from a specific snapshot.
# Follow the interactive guide:
timeshift --restore --snapshot <name>
# Restore from a snapshot but ignore the GRUB2 option.
# Follow the interactive guide and select a snapshot:
timeshift --restore --skip-grub
```

### BlueZ (bluetooth):

> ℹ️ **Note:** `bluez-utils` package is required for the following commands.

```bash
# List available controllers:
bluetoothctl list
```
```bash
# Show controller information:
bluetoothctl show <controller>
```
```bash
# Usage of bluetoothctl.
# Follow the interactive guide and type help to provide subcommands.
# Device becomes immediately pairable when in interactive mode.
# Change controller power state:
power <on|off>
# Scan for nearby devices:
scan on
# List available devices:
devices
# Show device info:
info <mac>
# Pair with a device:
pair <mac>
# Connect to a device:
connect <mac>
# Trust for auto-connect:
trust <mac>
# Disconnect from a device:
disconnect <mac>
# Remove a device:
remove <mac>
```

### PulseAudio/PipeWire:

> ℹ️ **Note:** Refer to [PipeWire](../guides/Linux/guides/pipewire.md) for terminology and concepts.

```bash
# Show current configuration:
pw-config list
# How all all configuration files to be used:
pw-config
```
```bash
# List all running node and device statistics:
pw-top
# Show properties on a specific node:
pw-cli info <ID>
```
```bash
# Show PulseAudio server information:
pactl info
```
```bash
# List all sinks, clients, modules and sources etc:
pactl list
```
```bash
# Append short for brief output.
# List all sinks:
pactl list sinks
# List all clients:
pactl list clients
# List all cards:
pactl list cards
# List all modules/plugins:
pactl list modules
```
```bash
# Get default sink:
pactl get-default-sink
```
```bash
# Change the default sink:
pactl set-default-sink <sink_name>
```
```bash
# Change sink volume:
pactl set-sink-volume <sink_name> <#%>
# or
pactl set-sink-volume @DEFAULT_SINK@ <#%>
```
```bash
# Play a sample sound file.
# Type needs to be understood by libsndfile.
# First upload the file to the sample cache:
pactl upload-sample <path/to/sample>
# Find the sample in the cache:
pactl list samples
# Play the sample out of the default sink.
# Use sink-name to play on a specific sink:
pactl play-sample <sample_name>
```

### WirePlumber:

```bash
# Show WirePlumber version and device information:
wpctl status
```
```bash
# Show all properties on an object:
wpctl inspect <id>
```
```bash
# Get a sink volume:
wpctl get-volume <id>
# or 
wpctl get-volume @DEFAULT_SINK@
```
```bash
# Use the --save switch to save the configuration.
# Show all WirePlumber settings:
wpctl settings
# Get a specific setting:
wpctl settings <Name>
# Set a specific setting:
wpctl settings <Name> <Value>
```

### RH subscription manager:

> ℹ️ **Note:** Commands usually require elevation.

```bash
# Show all subscription-manager configs:
subscription-manager config --list
# Delete a config value:
subscription-manager config --remove=<section.name>
# Adds a config value:
subscription-manager config --<section.name=value>
```
```bash
# Show system registration status:
subscription-manager status
```
```bash
# Show system registration status and product information:
subscription-manager list
# List available subscriptions for the system not currently attached:
subscription-manager list --available
# List all available subscriptions for the system:
subscription-manager list --all --available
# List all subscriptions matching installed products:
subscription-manager list --available --match-installed
# List all subscriptions currently attached to the system:
subscription-manager list --consumed
# List products installed on the system, regardless if subscribed or not:
subscription-manager list --installed
```
```bash
# Registering to RedHat customer portal (use --force to force re-registration).
# Register a system using a username:
subscription-manager register --username=<username>
# or with inline password:
subscription-manager register --username=<username> --password=<password>
# Register a system to RH customer portal using an activation key:
subscription-manager register --activationkey=<SKU>
# Register a system using a username and password then attach best matched subscriptions:
subscription-manager register --auto-attach
```
```bash
# Unregister a system from the RH customer portal:
subscription-manager unregister
# Remove all subscriptions and un-register the system:
subscription-manager clean
```
```bash
# Attach a subscription to the system:
subscription-manager attach --pool=<id>
# Attach best matched subscriptions automatically:
subscription-manager attach --auto
# Checks auto-attach status (Runs every 4 hours):
subscription-manager auto-attach --show
# Enable auto-attach:
subscription-manager auto-attach --enable
# Disable auto-attach:
subscription-manager auto-attach --disable
```
```bash
# Remove a subscription from the system by pool ID:
subscription-manager remove --pool=<id>
# or via serial:
subscription-manager remove --serial=<serial>
# Or straight out remove all:
subscription-manager remove --all
```
```bash
# List all the organizations associated with an account:
subscription-manager orgs
```
```bash
# Show all available system releases:
subscription-manager release --list
# Set a stick release for use with packages and updates:
subscription-manager release --set=<value>
# Removes current sticky release:
subscription-manager release --unset
```
```bash
# List all available repos provided by the CDN:
subscription-manager repos --list
# Disable a specific repo:
subscription-manager repos --disable=<Repo-ID>
# Enable a specific repo:
subscription-manager repos --enable=<Repo-ID>
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
# Show reverse dependency information for a package:
apt-cache rdepends <package>
```

#### Winget:

```bat
REM List Windows Package Manager info:
winget --info
```
```bat
REM List packages managed by WPM:
winget list
```
```bat
REM List all packages available to install with WPM:
winget search
REM append package name for search:
winget search <package>
```
```bat
REM Search for a package in a specific source, such as winget:
winget search --source <winget> <packagename>
```
```bat
REM show package info (Use --id for specific package):
winget show <package>
```
```bat
REM List all configured package sources:
winget source list
```
```bat
REM Update all configured sources:
winget source update
```

#### Pacman / Arch Linux:

```bash
# Syncing packages from remote repositories.
# Pacman refresh and update packages:
pacman -Syu --noconfirm
# Install package:
pacman -S <packagename>
# Print the download targets only instead of downloading and installing:
pacman -S --print <package>
# Download a package but do not install:
pacman -Sw <package>
# Query packages:
pacman -Ss <packagename>
# View package group members:
pacman -Sg <package>
# Query package information:
pacman -Si <package>
```
```bash
# Remove any non-installed cached packages from /var/cache/pacman/pkg:
# Use -Scc to clear all regardless1
pacman -Sc
# or (use the -u switch to target only uninstalled packages):
paccache -rk0
```
```bash
# Upgrading and adding packages locally or from a remote source.
# Install a package from a local file:
pacman -U </path/to/package.pkg.tar.zst>
# Install a package from a URI:
pacman -U <URI>
```
```bash
# Removing packages.
# Remove an installed package, use -Rs to remove depends:
pacman -R <packagename> 
# Remove all cached packages older than -3x versions earlier (use the -u switch to target only uninstalled packages):
paccache -r
# Remove -1x versions earlier (use the -u switch to target only uninstalled packages):
paccache -rk1
```
```bash
# Querying the local package database and files.
# Query installed packages:
pacman -Q <packagename>
# Query installed package with info:
pacman -Qi <packagename>
# Query list of files installed within a package:
pacman -Ql <packagename>
```
```bash
# Query files in the synced database.
# List all files that are part of a remote package (add -y to update the databases):
pacman -Fl <packagename>
# List which remote package this file belongs to (add -y to update the databases):
pacman -F <filename or /path/to/file>
````
```bash
# Query orphaned packages that no longer are depends:
pacman -Qtd
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
# Get last 10 security logs (oldest):
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

```bash
# Basic searching with find command.
# ‘.' is assumed as starting point if not specified or `pwd` can be used for current directory.
# Find a file with a specific name:
find <dir> -type f -name <'filename'>
# Find a file as an expression:
find <dir> -type f -name <'*name*'>
# Find a files with a specific extension:
find <dir> -type f -name <'*.ext'>
# Find a directory with a specific name:
find <dir> -type d -name <'filename'>
# Find a file as an expression:
find <dir> -type d -name <'*name*'>
# Print the full path of a file:
find <dir> -type f -name <'filename'> -print
# Print the full path of a directory:
find <dir> -type d -name <'filename'> -print
# Only search the current directory:
find <dir> -type f -name <'filename'> -maxdepth 1
# or:
find <dir> -type d -name <'filename'> -maxdepth 1
# Don't recurse into mounted directories for files and directories:
find <dir> -mount -name <'filename'>
```
```bash
# Chronological file searching with find command.
# 'n' = Exact.
# '+n' = After.
# '-n' = Before.
# Find files modified in the last +24 hours:
find <dir> -type f -mtime -1
# Find files modified in the last +48 hours:
find <dir> -type f -mtime -2
# Find files modified after +48 hours:
find <dir> -type f -mtime +1
# Find files changed in the last +24 hours:
find <dir> -type f -ctime -1
# Find files changed in the last +48 hours:
find <dir> -type f -ctime -2
# Find files changed after +48 hours:
find <dir> -type f -ctime +1
# Find files accessed in the last +24 hours:
find <dir> -type f -atime -1
# Find files accessed in the last +48 hours:
find <dir> -type f -atime -2
# Find files accessed after +48 hours:
find <dir> -type f -atime +1
# Find files modified in the last n minutes:
find <dir> -type f -mmin -<n>
# Find files modified after n minutes:
find <dir> -type f -mmin +<n>
# Find files changed in the last n minutes:
find <dir> -type f -cmin -<n>
# Find files changed after n minutes:
find <dir> -type f -cmin +<n>
# Find files accessed in the last n minutes:
find <dir> -type f -amin -<n>
# Find files accessed after n minutes:
find <dir> -type f -amin +<n>
```
```bash
# Find files and directories with based on permissions.
# Default should be octal notation but non-POSIX symbollics can be used, i.e u=<rwx>,g=<rwx>,o=<rwx>.
# Find files and directories with an exact permission set:
find <dir> -perm <octal>
# Find files and directories with 0777:
find <dir> -perm 0777
# Find files and directories where only the owner has read permissions:
find <dir> -perm 0400
# Find files and directories with a minimum permission set:
find <dir> -perm -<octal>
# Find files and directories where others have write permissions, regardless of user and owner:
find <dir> -perm -0006
# Find files and directories where others have read write and execute permissions:
find <dir> -perm -0007
# Find files and directories where others have read permissions regardless of user and owner:
find <dir> -perm -0004
# Find files and directories where others have execute permissions regardless of user and owner:
find <dir> -perm -0001
# Find files and directories where owner and group have read and write permissions:
find <dir> -perm -0660
# Find files and directories which contains a permission set, either user, group or other:
find <dir> -perm /<octal>
# Find files and directories where someone has read and write permissions:
find <dir> -perm /0666
# Find files and directories where either the owner or group has write permissions:
find <dir> -perm /0440
# Find files and directories where anyone other than the owner and group has write permissions:
find <dir> -perm /0044
```
```bash
# Execution with find command.
# With "-exec <command> {} \;" the command is executed for each file found, in order and per line.
# With "-exec <command> {} +" the command is executed for all files found in one go. 
# Use the -print switch to display the files found.
# Find all files that begin with the name picture and delete them (use -mount to avoid traversing mounted directories):
find <dir> -type f -name 'picture*' -exec rm {} \;
# Find all .txt files in the current directory and delete them:
find -type f -name '*.txt' -exec rm {} \;
# Find all files in the current directory and delete them:
find -type f -exec rm -f {} \;
# Find all files where other users have execute permissions and remove that permission (use -mount to avoid traversing mounted directories):
find <dir> -type f -perm -0001 -exec chmod o-x {} \;
```
```bash
# To find the largest 10 directories:
find <dir> -type f -print0 | xargs -0 du | sort -n | tail -10 | cut -f2 | xargs -I{} du -sh {}
# or
# Only difference is -type {d:f}.
find <dir> -type d -print0 | xargs -0 du | sort -n | tail -10 | cut -f2 | xargs -I{} du -sh {}
# or
du -a <dir> | sort -n -r | head -n 10
```
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
# Reading files with dd.
# Use status=progress to print progress.
# Read a file and output to stdout:
dd if=<file>
# Read a file and convert to all to uppercase:
dd if=<file> conv=ucase
# Read a file and convert to all to lowercase:
dd if=<file> conv=lcase
# Read a file but limit to specific block(s) of 512 bytes:
dd if=<file> count=#
```
```bash
# Copying files with dd.
# Use status=progress to print progress.
# Read a file and output to another file (can be used to copy a file):
dd if=<file> of=<file>
# Copy an entire partition to another partition (ensure target parition is not mounted):
dd if=<dev> of=<dev> 
# Copy an entire disk to another disk:
dd if=<dev> of=<dev> 
# Create an image file of an entire disk:
dd if=<dev> of=<image.img>
# Create a bootable USB from an ISO image:
dd if=<image.iso> of=</dev>
# Create an ISO image for a CD/DVD mount of either a file or device:
dd if=<file | dev> of=<file | dev>
# Copy either a file or device but do not stop on errors:
dd if=<file | dev> of=<file | dev> conv=noerror
# Copy either a file or device with syncronous writes (slower but safer for data integrity):
dd if=<file | dev> of=<file | dev> oflag=sync
# Copy either a file or device with direct I/O (bypass kernel read/write caches):
dd if=<file | dev> of=<file | dev> oflag=direct
# Copy either a file or device with but do not overwrite any targets:
dd if=<file | dev> of=<file | dev> conv=notrunc
# Copy either a file or device with full data syncronization:
dd if=<file | dev> of=<file | dev> conv=fdatasync
# or with +metadata:
dd if=<file | dev> of=<file | dev> conv=fsync
# Copy either a file or device and change the input/output block size in bytes or k or M (default is 512).
# 512 bytes is typical of spindle disks, 4k is typical of SSDs so the default is somewhat out of data. A typical value of 64k can be used:
dd if=<file | dev> of=<file | dev> bs=<#>
```
```bash
# Wiping block devices with dd.
# Use status=progress to print progress.
# Wipe an entire device with dd:
dd if=/dev/zero of=<dev>
# Randomize an entire device (useful for security if ran before dd if=/dev/zero):
dd if=/dev/urandom of=<dev>
```

#### GNU Stow:

> ℹ️ **Note:**
> - Stow will use the working directory as the stow directory unless changed.
> - The term `package` refers to the `directory` containing the files to be managed. The stow `directory` must match the target tree for `defaults` to work.
> - The term `target` refers to the directory where the files will be symlinked from, at default this is the parent directory of the package.

```bash
# Stow a package:
stow <package>
# Stow a package with verbose output:
stow <package> --verbose
# Stow a package as a dry run only:
stow <package> --verbose --simulate
# Stow a package even if the target files exist (files will be overwritten in the stow directory):
stow <package> --adopt
# Stow a package but do not merge/un-merge directories with a similar structure:
stow <package> --no-folding
# Stow a package with a specific target directory:
stow <package> --target <target>
# Stow a package with a specific target directory and a specific stow directory:
stow <package> --target <target> --dir <directory>
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
# Get directory size:
du -h -c dir
```

### Archiving and Compression:

```bash
# Opening archives with tar
# Omit the -v to remove verbosity
# Open a tarball with tar:
tar -tvf <tarball.tar>
# Emmit the -v to list extended file info like permissions etc:
tar -tzvf <tarball.tar>
```
```bash
# Archiving with tar
# Omit the -v to remove verbosity
# Create a tarball using tar:
tar -cvf <archive.tar> <file1> <file2> <file3>
# Create a tarball using tar and check in to the directory first (minimal directory structure):
tar -cvf <archive.tar> -C <directory> .
# Create a tarball using tar and maintain directory structure:
tar -cvf <archive.tar> <directory>
# Create a tarball with wildcard expression like archiving all .JSON files in the current directory:
tar -cvf archive.tar *.json
# Create a compressed tarball using tar and gzip:
tar -czvf <archive.tar.gz> <file1> <file2> <file3>
# Again but maintaining directory structure:
tar -czvf <archive.tar> <directory>
# Create a tarball but no not recurse child objects: 
tar --no-recursion -cvf <archive.tar> <directory>
```
```powershell
# Create a compressed archive with piped input files:
Get-ChildItem -Path <'filename'> | Compress-Archive -DestinationPath <'archive.zip'>
# Create a compressed archive using a whole directory:
Compress-Archive -Path <'directory'> -DestinationPath <'archive.zip'>
# Create a compressed archive using only the child items in the directory:
Compress-Archive -Path <'directory\*'> -DestinationPath <'archive.zip'>
# Add a file to an existing compressed archive:
Compress-Archive -Path <'file'> -DestinationPath <'archive.zip'> -Update
```
```powershell
# Extract an archive to a new directory in the current working directory named as the archive base level:
Expand-Archive -Path <archive.zip>
# Extract an archive to a directory named specifically:
Expand-Archive -Path <archive.zip> -DestinationPath <directory>
```
```bash
# Extracting archives with tar
# Omit the -v to remove verbosity
# Extract a tarball with tar to the current directory:
tar -xvf <archive.tar>
# Extract a single file from a tarball:
tar -xvf <archive.tar> <file_in_archive.file>
# Extract a tarball with tar to different existing directory:
tar -xvf <archive.tar> --directory <directory>
# Extract a tarball with tar to the current directory but remove # number of parent directories.
# If the archive is less than the number of directories to be stripped, then it will remove the data:
tar --strip-components=<#> -xvf <archive.tar> 
# Extract a compressed tarball to the current directory:
tar -zxvf <archive.tar.gz>
# Uncompress and extract to a directory named the same as the archive base level:
tar --one-top-level -zxvf <archive.tar.gz>
# Uncompress and extract to a directory named as specified:
tar --one-top-level=<directory> -zxvf <archive.tar.gz>
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

### Time management:

#### systemd-timesyncd:

```bash
# Show time sync status:
timedatectl status
# Show time sync configuration:
timedatectl show
# Show info of systemd-timesyncd, including NTP servers and stratum etc:
timedatectl timesync-status
# Show configuration of systemd-timesyncd:
timedatectl show-timesync
# List all supported timezones:
timedatectl list-timezones
```
```bash
# Set the timezone:
timedatectl set-timezone <ZONE>
# Enable or disable NTP syncronisation:
sudo timedatectl set-ntp <true|false>
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

#### NetworkManager:

> ℹ️ **Note:** Ensure the NetworkManager.service unit is active.

```bash
# Show NetworkManager status:
nmcli general status
```
```bash
# Get network connectivity state:
nmcli networking connectivity
```
```bash
# Show all NetworkManager activity logs in realtime:
nmcli monitor
# Show activity logs for a specific device:
nmcli device monitor
# Show activity logs for a specific connection:
nmcli connection monitor <con-name>
```
```bash
# Show all device status:
nmcli device status
# Show detailed information for all devices:
nmcli device show
# Show detailed information for a specific device:
nmcli device show <name>
```
```bash
# Down a device:
nmcli device down <name>
# Bring up a device:
nmcli device up <name>
```
```bash
# Modify properties of a device. 
# Use nm-settings-nmcli(5) for properties.
# 
nmcli device modify lo ...
```
```bash
# Working with connections using NetworkManager.
# 
```


#### systemd-networkd:

> ℹ️ **Note:** Ensure the systemd-networkd.service unit is active. See [systemd-networkd - Network manager](./Linux/guides/systemd-networkd.md) for more information.

```bash
# List all links on system:
networkctl list
# List specific link information:
networkctl list <link|idx>
```
```bash
# Show all links on system with service logs:
networkctl status
# Show the service status:
systemctl status systemd-networkd
# Show all link status information:
networkctl status --all
# Show specific link information:
networkctl status <link|idx>
```
```bash
# Reload .network and .netdev files:
networkctl reload
```
```bash
# Bring a link down:
networkctl down <link|idx>
# Bring a link up:
networkctl up <link|idx>
```
```bash
# Renew DHCP lease on a link:
networkctl renew <link|idx>
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
```bash
# List all block devices:
lsblk -a
```
```bash
# List FS info on all block devices:
lsblk -f
# or
df -T
```
```bash
# List filesystems with human readable figures:
df -h
```
```bash
# Scan for potential LVM psychical volume disks:
lvmdiskscan
```
```bash
# List information on LVM physical volumes:
pvdisplay
# Display specific PV name:
pvdisplay </dev/device>
```
```bash
# List information on LVM volume groups:
vgs -a
# or:
vgdisplay
# Display information on specific volume group:
vgdisplay <VG Name>
```
```bash
# Display information on LVM logical volumes:
lvdisplay -v
# Display information on specific logical volumes:
vdisplay -v <LV path>
```
```bash
# Add additional LVM physical volume from a new disk and add to volume group
# Confirm disk has no partitions:
lsblk -f </dev/disk>
# Add new PV to volume group, which creates PV automatically:
vgextend <VG name> </dev/disk>
# Or create new PV from the new disk manually:
pvcreate </dev/disk>
```
```bash
# Increase logical volume and extend FS on sole disk
# Add additional primary partition on disk:
fdisk </dev/disk>
# Create new partition:
n
# Create primary partition:
p
# Enter partition number, enter for default and select default for both start and end sectors
# Write changes to disk:
w
# Create new physical volume from new partition:
pvcreate </dev/newpart>
# Extend volume group:
vgextend <VG Name> </dev/new_pv>
# Confirm vg is now extended with "Free  PE / Size":
vgdisplay <VG Name>
# Extend logical volume and FS:
lvextend -r -l +100%FREE <LV Path>
# Optionally you can increase the FS (XFS in this case) without using the -r switch in the previous command:
xfs_growfs <FS>
```

#### nvme-cli:

> ℹ️ **Note:** Requires `nvme-cli` package.

> ℹ️ **Note:** `ctrl` refers to the NVMe controller (`/dev/nvme0`) where `dev` refers to the device with namespace ID (`/dev/nvme0n1`) for example.

> ℹ️ **Note:** Use `nvme id-ctrl` to find controller capabilities such as `NS Management` or `Sanitize` for example as some of the commands may not be supported.

```bash
# List nvme subsystems
nvme list-subsys
```
```bash
# Show controller information assoicated with an NVMe device:
nvme id-ctrl <ctrl>
# Show controller information assoicated with an NVMe device with human readable output:
nvme id-ctrl <ctrl> --human-readable
```
```bash
# Show a topology of an NVMe device:
nvme show-topology <dev>
```
```bash
# List all NVMe devices and namespaces:
nvme list
# List all namespaces on a device:
nvme list-ns <ctrl>
# Get namespace IDs on a specific block device:
nvme get-ns-id <dev>
# Show detailed information on a specific namespace of a device (remove --human-readable to show in hex):
nvme id-ns <ctrl> --namespace-id=<#> --human-readable
# Show detailed information on all namespaces of a device (remove --human-readable to show in hex):
nvme id-ns <dev> --human-readable
```
```bash
# Format a namespace.
# Warning: This will erase all data on the namespace within 10 seconds.
# Use --force to skip the 10 second wait.
nvme format <ctrl> --namespace-id=<#> --reset
```
```bash
# Format a namespace with secure erase.
# Warning: This will erase all data on the namespace within 10 seconds.
# Use --force to skip the 10 second wait.
# Perform a block secure erase on the namespace:
nvme format <ctrl> --namespace-id=<#> --ses=1 --reset
# Perform a crypto secure erase on the namespace:
nvme format <ctrl> --namespace-id=<#> --ses=2 --reset
```
```bash
# Sanitize a device.
# Perform a block sanitize on the device:
nvme sanitize <ctrl> --sanact=2
# Perform a crypto sanitize on the device:
nvme sanitize <ctrl> --sanact=4
```
```bash
# Get the firmware versions of all the slots including the active slot.
# Use --output-format=json for human readable:
nvme fw-log <ctrl>
# Get the device error log.
# Use --log-entries=<#> to limit the number of entries returned.
# Use --output-format=json for human readable:
nvme error-log <ctrl>
# Show SMART log for an NVMe device:
nvme smart-log <ctrl> --human-readable
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

### Fonts:

#### Fontconfig:

> **Note**: Manual local user fonts are stored in `~/.local/share/fonts` and manaual system fonts are stored in `/usr/local/share/fonts`. Package fonts are stored in `/usr/share/fonts`.

```bash
# Use --verbose to show more information.
# List available fonts:
fc-list
# List available fonts supporting a specific style, such as Bold or Regular:
fc-list :style=<style>
# List available fonts supporting a specific language:
fc-list :lang=<lang>
# List available fonts supporting a specific fontformat:
fc-list :fontformat=<fontformat>
```
```bash
# Use --verbose to show more information.
# Rescan font directories:
fc-cache
# Rescan only system font directories:
fc-cache --system-only
# Rescan font directories and force update timestamps:
fc-cache --force
# Erase cache and rescan font directories:
fc-cache --really-force
```
```bash
# Scan a font file for information:
fc-scan <filename>
# Scan a directory containing font files for information:
fc-scan <directory>
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

#### resolvconf:

> ℹ️ **Note:** Requires systemd-resolved.

```bash
# Show detailed server information:
resolvectl show-server-state <domain>
```
```bash
# Show current DNS settings for all interfaces:
resolvectl status
# Show DNS settings for a specific interface:
resolvectl dns <interface>
```
```bash
# Set DNS servers for a specific interface:
resolvectl dns <interface> <dns1> | <dns2>
```
```bash
# Show DNS search listsfor a specific interface:
resolvectl domain <interface>
```
```bash
# Set DNS search domains for a specific interface:
resolvectl domain <interface> <domain1> | <domain2>
```
```bash
# Reset DNS settings for a specific interface:
resolvectl revert <interface>
```
```bash
# Show active local DNS queries and their responses:
resolvectl monitor
```
```bash
# Resolve an A and or AAAA record:
resolvectl query <domain>
# Resolve a specific RRType:
resolvectl query --type=<RRType> <domain>
```
```bash
# Show the stub resolver cache:
resolvectl show-cache
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
```powershell
# Get VHD information:
Get-VHD -Path <path\to\vhd>
```
```powershell
# Resize VHD
Resize-VHD -Path <path\to\vhd> -SizeBytes <bytes>
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

### Flatpak:

> ℹ️ **Note:** All commands are system-wide unless `--user` is specified.

> ℹ️ **Note:** If the user is in `wheel` and **not** on a PTS session then elevation is usually required.

```bash
# Working with repositories.
# List installed repositories:
flatpak remotes
# Add a new repository:
flatpak remote-add <custom_name> <URI>
# Remove a repository:
flatpak remote-delete <custom_name>
# List all applications and runtimes in a repository:
flatpak remote-ls <custom_name>
```
```bash
# Search for applications:
flatpak search <string>
# Search for applications and display only a specific field:
flatpak search spotify --columns=<FIELD>
```
```bash
# Working with flatpak applications and runtimes.
# Update all applications and runtimes:
flatpak update
# Update a specific application or runtime:
flatpak update <id>
# List installed applications:
flatpak list
# Show information about a runtime or application:
flatpak remote-info <repo> <ref|id>
# Run a flatpak application:
flatpak run <id>
# Show running flatpak applications:
flatpak ps
# Kill a running flatpak application:
flatpak kill <instance|id>
```
```bash
# Installing and removing flatpak applications and runtimes.
# Use --noninteractive to skip prompts.
# Install an application or runtime:
flatpak install <repo> <ref|id>
# Install an application or runtime with just the name (may give conflicts):
flatpak install <name>
# Remove an application or runtime:
flatpak uninstall <ref|id>
```
```bash
# Show activity log of flatpak:
flatpak history
```

### video4linux:

```bash
# List all v4l devices:
v4l2-ctl --list-devices
```
```bash
# Show driver info for a specific device:
v4l2-ctl --device <device> --info
# Show detailed device information:
v4l2-ctl --device <device> --all
```
```bash
# List controls available for a specific device:
v4l2-ctl --device <device> --list-ctrls
```

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

### Yubikey Manager:

```bash
# List connected YubiKey devices:
ykman list
# Show full information of connected YubiKey devices:
ykman show
# Show full information of specific connected YubiKey device:
ykman --device <sku> info
# List applications enabled by interface:
ykman config <interface> --list
# Enable all applications of an interface:
ykman config <interface> --enable-all
# Disable an application of an interface:
ykan config <interface> --disable <application> 
# Create an application lock code to protect the configs (use --generate to generate a code):
ykman config set-lock-code
# Clear an existing lock code:
ykman config set-lock-code --clear
```
```bash
# Manage FIDO2 application.
# Set or change a FIDO2 pin:
ykman fido access change-pin
# Verify PIN is correct of a YubiKey:
ykman fido access verify-pin
# Rest a FIDO2 pin (clears all FIDO2 entries).
# Follow scripted advice:
ykman fido reset
# List FIDO2 credentials:
ykman fido credentials list
# Delete a FIDO2 credential:
ykman fido credentials delete <Credential ID>
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

### Sysinternals:

```bat
REM psexec
REM Execute Powershell command via psexec:
psexec \\<target> cmd /c "echo . | powershell <command>"
```

### HTTP Commands:

```bash
# curl allow insecure server connections when using SSL
curl -k <URI>
```
```bash
# Curl output file to location:
curl -L -o <destination.filetype> URI
```
```bash
# Curl static DNS mapping, can be used with A and CNAME RRs (Useful for TLS SNI):
curl --resolve <DNS:port:IP.addr/DNS_record> <URL>
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

### DNS Commands:

```bash
# Query the root hints:
dig . NS
```
```bash
# Attempt an XFR pull:
dig AXFR <zonename> <NS>
```
```bash
# Perform an iterative query and report back the results. Use @nameserver to verify the hinters using a specific server:
dig +trace <QNAME>
```

### Certificates, Key-based Authentication, and Encryption:

#### OpenSSL:

```bash
# Creating and converting with OpenSSL
# Create PKCS12 archive from a private key and signed PEM certificate:
openssl pkcs12 -in certificate.pem -inkey private.key -export -out certificate.pfx
# Convert from PKCS#12 to PEM format and export private key to a combined PEM file:
openssl pkcs12 -in certificate.pfx -out certificate.pem -nodes
# Omit the -nodes to export an encrypted private key
openssl pkcs12 -in certificate.pfx -out certificate.pem
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
# View only certificate information:
openssl pkcs12 -in certificate.pfx -info -nokeys -noout
# View only the encrypted private key:
openssl pkcs12 -in certificate.pfx -nocerts
# View only the private key:
openssl pkcs12 -in certificate.pfx -nocerts -nodes
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
# Working with public and private keys in OpenSSL.
# Read a private key file (enter passphrase if required):
openssl rsa -in private.key -noout -text
# or:
openssl pkey -in privkey.key -text
# Read a public key:
openssl pkey -in pubkey.key -pubin -text
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
# Generate a private / public key pair using an existing key:
openssl pkey -in private.key -out public.key -pubout
```
```bash
# Encrypt an existing private key using the 3DES cipher:
openssl pkey -in private.key -des3 -out private2.key
# Or with the AES128 chiper:
openssl pkey -in private.key -aes128 -out private2.key
# Or with the AES256 chiper:
openssl pkey -in private.key -aes256 -out private2.key
# Or with the AES256-CBC cipher:
openssl pkey -in private.key -out private2.key -aes-256-cbc
```
```bash
# Decrypt a private Key:
openssl pkey -in private.key -out private2.key
```
```bash
# Working with Certificate Signing Requests (PKCS10) in OpenSSL.
# Create a CSR based on an existing key:
# Follow the interactive guide, use "." to leave certain fields blank
openssl req -key private.key -new -out website.csr
# Create a CSR with a new private key:
# Follow the interactive guide, use "." to leave certain fields blank
openssl req -newkey rsa:2048 -keyout private.key -nodes -out website.csr
# Generate a new CSR with with a new private key and optional extensions like X509v3 Subject Alternative Name (requires 1.1.1 min):
# Follow the interactive guide, use "." to leave certain fields blank
openssl req -newkey rsa:2048 -keyout private.key -nodes -addext "subjectAltName = DNS:fqdn" -out website.csr
# Generate a new CSR with a new private key and optional extensions like X509v3 Basic Constraints. Set the CA boolean to TRUE or FALSE:
# Follow the interactive guide, use "." to leave certain fields blank
openssl req -newkey rsa:2048 -keyout private.key -nodes -addext "basicConstraints = CA:FALSE" -out website.csr
# Generate a new CSR with a new private key and optional extensions like X509v3 Extended Key Usage:
# Follow the interactive guide, use "." to leave certain fields blank
openssl req -newkey rsa:2048 -keyout private.key -nodes -addext "extendedKeyUsage = serverAuth" -out website.csr
# Generate a new CSR based on a new private key, without using the wizard:
openssl req -newkey rsa:2048 -keyout private.key -nodes -subj "/C=AU/ST=Some-State/O=Internet Widgits Pty Ltd/CN=fqdn" -out website.csr
# With subject and SAN extensions:
openssl req -newkey rsa:2048 -keyout private.key -nodes -subj "/C=AU/ST=Some-State/O=Internet Widgits Pty Ltd/CN=fqdn" -addext "subjectAltName = DNS:fqdn" -out website.csr
# or:
openssl req -newkey rsa:2048 -keyout private.key -nodes -subj "/CN=fqdn" -out website.csr
# or 
openssl req -newkey rsa:2048 -keyout private.key -nodes -subj "/CN=fqdn" -addext "subjectAltName = DNS:fqdn" -out website.csr
# With basic constraints. Set the CA boolean to TRUE or FALSE:
openssl req -newkey rsa:2048 -keyout private.key -nodes -subj "/CN=fqdn" -addext "basicConstraints = CA:FALSE" -out website.csr
# With extended key usage:
openssl req -newkey rsa:2048 -keyout private.key -nodes -subj "/CN=fqdn" -addext "extendedKeyUsage = serverAuth" -out website.csr
```
```bash
# Read and verify an existing CSR:
openssl req -in request.csr -text -noout -verify
```
```bash
# Creating self-signed certificates in OpenSSL.
# Create a self-signed certificate with an existing private key:
# Follow the interactive guide, use "." to leave certain fields blank
openssl req -x509 -new -key private.key -days 365 -out website.pem
# Create a self-signed certificate with a new private key:
# Follow the interactive guide, use "." to leave certain fields blank
openssl req -x509 -new -newkey rsa:2048 -nodes -keyout private.key -days 365 -out website.pem
# Omit the "-nodes" switch to prompt for passphrase.
openssl req -x509 -new -newkey rsa:2048 -keyout private.key -days 365 -out website.pem
# Generate a new CSR based on a new private key, without using the wizard:
openssl req -x509 -new -newkey rsa:2048 -nodes -keyout private.key -subj "/CN=fqdn" -days 365 -out website.pem
# With subject, SAN extensions, basic constraints and extended key usage:
openssl req -x509 -new -newkey rsa:2048 -nodes -keyout private.key -subj "/CN=fqdn" -addext "subjectAltName = DNS:fqdn" -addext "basicConstraints = CA:FALSE" -addext "extendedKeyUsage = serverAuth" -days 365 -out website.pem
```
```bash
# Generate a self signed certificate based on an existing CSR and private key:
openssl x509 -signkey private.key -in request.csr -req -days 365 -out website.cer
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
# Verify against public key:1
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
# Sign and encrypt a file for sharing between user A => user B using OpenSSL:
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
# Load a SSL/TLS server for debugging:
openssl s_server -port 443 -cert certificate.pem -key private.key -status 
# Support HTTP by adding -www to return a / on HTTP GET:
openssl s_server -port 443 -cert certificate.pem -key private.key -www
```
```bash
# OpenSSL s_client for client to server connections
# OpenSSL connect to host and display connection and certificate info:
openssl s_client -connect host:443
# Displays only info on valid dates:
openssl s_client -connect host:443 | openssl x509  -noout -dates
# Show all certs sent by the server, including any CA certs:
openssl s_client -connect host:443 -showcerts 
# Show brief TLS connection settings with certificate validation only:
openssl s_client -connect host:443 -brief
# Configure alternative server name for SNI:
openssl s_client -connect host:443 -servername fqdn
# Show TLS/SSL states during connect:
openssl s_client -connect host:443 -state
# Connect to StartTLS smtp port (can change protocol):
openssl s_client -connect host:25 -starttls smtp
```
```powershell
# Reading certificate stores with PowerShell
# This leverages the Windows Certificate Store functions using the PowerShell Certificate provider
# List certificate locations:
Get-ChildItem -Path 'Cert:'
# List certificate stores and locations:
Get-ChildItem -Path 'Cert:' -Recurse | Where-Object -Property PSIsContainer -EQ $true
# List all store names in the LocalMachine store:
Get-ChildItem -Path 'Cert:\LocalMachine\'
# List all certificates in all stores:
Get-ChildItem -Path 'Cert:' -Recurse | Where-Object -Property PSIsContainer -EQ $false
# List all CA root certificates in the LocalMachine store:
Get-ChildItem -Path 'Cert:\LocalMachine\Root'
# List basic X509 attributes of a specific certificate with subject:
Get-ChildItem -Path 'Cert:' -Recurse | Where-Object -Property Subject -EQ <'id-at-commonName'>
# List basic X509 attributes of a specific certificate with SHA1 fingerprint:
Get-ChildItem -Path 'Cert:' -Recurse | Where-Object -Property Thumbprint -EQ <'string'>
# or for known path:
Get-ChildItem -Path 'Cert:<\path\thumbprint>'
# List basic X509 attributes of a specific certificate with a SAN DNS attribute:
Get-Childitem -Path 'Cert:' -Recurse -DnsName <'FQDN'>
# Get all available X509 attributes and PS object types of a specific certificate with subject:
Get-ChildItem -Path 'Cert:' -Recurse | Where-Object -Property Subject -EQ <'id-at-commonName'> | Format-List *
# Get all certificates expiring in x number of days (includes expired):
Get-ChildItem -Path 'Cert:' -Recurse -ExpiringInDays <'days'>
# Or:
Get-ChildItem -Path 'Cert:' -Recurse | Where-Object {$_.PSIsContainer -EQ $false -and $_.NotAfter -le (Get-Date).AddDays(<'days'>)}
# Get all certificates expiring in x number of days:
Get-ChildItem -Path 'Cert:' -Recurse | Where-Object {$_.PSIsContainer -EQ $false -and $_.NotAfter -ge (Get-Date) -and $_.NotAfter -le (Get-Date).AddDays(<'days'>)}
# Get all certificates expiring in the next month:
Get-ChildItem -Path 'Cert:' -Recurse | Where-Object {$_.PSIsContainer -EQ $false -and $_.NotAfter -ge (Get-Date) -and $_.NotAfter -le (Get-Date).AddMonths('1')}
# Get all expired certificates:
Get-ChildItem -Path 'Cert:' -Recurse | Where-Object {$_.PSIsContainer -EQ $false -and $_.NotAfter -le (Get-Date)}
# Get all expired certificates from the system personal store:
Get-ChildItem -Path 'Cert:\LocalMachine\My\' -Recurse | Where-Object {$_.PSIsContainer -EQ $false -and $_.NotAfter -le (Get-Date)}
# Get all non-expired certificates:
Get-ChildItem -Path 'Cert:' -Recurse | Where-Object {$_.PSIsContainer -EQ $false -and $_.NotAfter -ge (Get-Date)}
```
```powershell
# Managing certificate stores with PowerShell
# This leverages the Windows Certificate Store functions using the PowerShell Certificate provider
# Create a new certificate store in the local computer store:
New-Item -Path 'Cert:\LocalMachine\<storename>\'
# Remove a certificate:
Remove-Item -Path 'Cert:<\path\thumbprint>'
# Remove all expired certificates from a specific store:
Get-ChildItem -Path 'Cert:\<path>' -Recurse | Where-Object {$_.PSIsContainer -EQ $false -and $_.NotAfter -le (Get-Date)} | Remove-Item -DeleteKey
# Move a certificate from one store to another (only local store):
Move-Item -Path 'Cert:<\path\thumbprint>' -Destination 'Cert:\<store\path>'
```
```bat 
REM Conversations with certutil:
REM Convert DER format to Base64:
certutil -encode certificate.der certificate.pem
REM Convert Base64 PEM format to DER:
certutil -decode certificate.pem certificate.der
```
```bat
REM PKCS12
REM Create PKCS12 archive from a pair of PEM certificates. Ensure both certificates have the same file name where the certificate is .cer and the private key is .key if exist:
CertUtil -MergePFX certificate.cer certificate.pfx
```
```bat
REM Reading certificates with certutil
REM PKCS12
REM Read PFX PKCS12 arhvive and private key:
certutil -dumpPFX -privatekey -p <password> certificate.pfx
REM Read PEM formats:
certutil -dump certificate.key
REM or:
certutil -dump certificate.cer
REM or:
certutil -dump certificate.cer
REM DER binary format:
certutil -dump certificate.der
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

#### OpenPGP/GnuPG:

> ℹ️ **Note:** All gpg commands are based on gpg 2.4.x.

```bash
# List all keys in the public keyring:
gpg --list-keys
# List all keys based on a specific user ID in the public keyring:
gpg --list-keys <USER-ID>
# List all keys from the secret keyring:
gpg --list-secret-keys
# Show the fingerprint of a key:
gpg --fingerprint <USER-ID>
```
```bash
# Generate a new key pair.
# Follow the interactive guide:
gpg --generate-key
# Generate a new key pair with a specific key type and validity.
# Follow the interactive guide:
gpg --full-generate-key
# Generate a new key pair with only the USER-ID field.
# Follow the interactive guide:
gpg --quick-generate-key <USER-ID>
```
```bash
# Generate a revocation certificate so that it can be used to revoke a key.
# Follow the interactive guide:
gpg --output <file.asc> --gen-revoke <USER-ID | fingerprint> 
```
```bash
# Remove a secret key from the secret keyring:
gpg --delete-secret-keys <USER-ID | fingerprint>
# Remove a public key from the public keyring:
gpg --delete-keys <USER-ID | fingerprint>
# Remove both the secret and public key from the keyrings:
gpg --delete-secret-and-public-key <USER-ID | fingerprint>
```
```bash
# Export a public key from the public keyring to stdout:
gpg --export <USER-ID | fingerprint>
# Export a public key from the public keyring to stdout as Base64:
gpg --export --armor <USER-ID | fingerprint>
# Export a public key from the public keyring to a file as Base64:
gpg --export --output <file> --armour <USER-ID | fingerprint>
# Export a secret key from the secret keyring to stdout:
gpg --export-secret-keys <USER-ID | fingerprint>
# Export a secret key from the secret keyring to stdout as Base64:
gpg --export-secret-keys --armor <USER-ID | fingerprint>
# Export a secret key from the secret keyring to a file as Base64:
gpg --export-secret-keys --output <file> --armour <USER-ID | fingerprint>
```
```bash
# Import a key(s) into the keyrings:
gpg --import <file>
# Perform a dry run import of a key(s) into the keyrings:
gpg --import --import-options show-only <file>
# Import a key(s) into the keyrings and update existing keys only, no new keys will be imported:
gpg --import --import-options merge-only <file>
```
```bash
# Edit a key.
# Follow the interactive guide and use ? or help to provide subcommands:
gpg --edit-key <USER-ID | fingerprint>
# Add additional USER-ID:
adduid
save
# Delete a USER-ID:
uid <#>
deluid
save
# Change expiration date.
# Follow the interactive guide:
expire
save
# Sign a key that has been imported.
# Follow the interactive guide:
sign
save
```
```bash
# Encryption.
# Use --version to show supported ciphers.
# Symmetrically encrypt a file with a passphrase using AES256.
# Enter passphrase when prompted:
gpg --symmetric --output <cypher_file> <file_to_encrypt>
# Symmetrically encrypt a file with a passphrase using AES256 where cipher spec is Base64):
# Enter passphrase when prompted:
gpg --symmetric --armor --output <cypher_file> <file_to_encrypt>
# Symmetrically encrypt a file with a passphrase using AES128.
# Enter passphrase when prompted:
gpg --symmetric --cipher-algo AES128 --output <cypher_file> <file_to_encrypt>
# Symmetrically encrypt a file with a passphrase using CAMELLIA256.
# Enter passphrase when prompted:
gpg --symmetric --cipher-algo CAMELLIA256 --output <cypher_file> <file_to_encrypt>
# Encrypt a message from stdin using AES256 to a file with the default secret key and associated with a specific public key(s).
# Use Ctrl+D to end input:
gpg --encrypt --output <cypher_file> --recipient <USER-ID | fingerprint>
# Encrypt a file using AES256 with the default secret key and associated with a specific public key(s):
gpg --encrypt --output <cypher_file> --recipient <USER-ID | fingerprint> <file_to_encrypt>
# Encrypt a file using AES128 with the default secret key and associated with a specific public key(s):
gpg --encrypt --cipher-algo AES128 --output <cypher_file> --recipient <USER-ID | fingerprint> <file_to_encrypt>
```
```bash
# Signing.
# Sign an existing public key in the public keyring with the default private key:
gpg --sign-key <USER-ID | fingerprint>
# Sign an existing public key in the public keyring with a specific private key:
gpg --sign-key <USER-ID | fingerprint> --default-key <USER-ID | fingerprint>
# Locally sign an existing public key in the public keyring with the default private key:
gpg --lsign-key <USER-ID | fingerprint>
# Sign and compress a file with a private key and export to a binary signature file.
# Enter passphrase if prompted:
gpg --output <signed_file> --sign <file_to_sign>
# Clearsign a file with a private key and export to file.
# Enter passphrase if prompted:
gpg --output <signed_file> --clearsign <file_to_sign>
# Sign a file with a private key and export to a detached signature file.
# Enter passphrase if prompted:
gpg --output <signed_file> --detach-sign <file_to_sign>
# Verify a signed signature file:
gpg --verify <signed_file>
# Verify a signed signature file and its corresponding detached signature file:
gpg --verify <signed_file> <detached_signature_file>
```
```bash
# Decryption.
# Use --version to show supported ciphers.
# Decrypt a symmetrically encrypted file to stdout:
gpg --decrypt <cypher_file>
# Decrypt a symmetrically encrypted file to a file:
gpg --output <filename> --decrypt <cypher_file>
# Decrypt a file using the public key associated with the encrypted file to stdout:
gpg --decrypt <cypher_file>
# Decrypt a signed signature file and output original to a file:
gpg --output <filename> --decrypt <signed_file>
# Decrypt a file using the public key associated with the encrypted file:
gpg --output <filename> --decrypt <cypher_file>
```
```bash
# Working with Web Key Directorys (WKD).
# Query a WKD server for Web Key Service (WKS) support:
gpg-wks-client --verbose --supported <domain>
# Check if a public key exists for an email address:
gpg-wks-client --verbose --check <email>
# Find USER-ID and corresponding mailboxes for an email address:
gpg-wks-client --print-wkd-hash <email>
# Show the URIs used to fetch the public key for an email address:
gpg-wks-client --print-wkd-url <email>
```

#### OpenSSH:

```bash
# Creating and managing OpenSSH keys with ssh-keygen.
# This requires the openssh package and it's dependencies.
# Create an OpenSSH RSA key pair with comments (default will be $USER@$NAME):
ssh-keygen -t rsa -C <'Comments'>
# Create an OpenSSH RSA key pair with a large key length than default (3072)
# Follow the prompts select the default options:
ssh-keygen -t rsa -b 4072
# Create an OpenSSH Ed25519 key pair
# Follow the prompts select the default options:
ssh-keygen -t ed25519
# Create an OpenSSH RSA key pair and specify a file location.
# Follow the prompts select the default options:
ssh-keygen -t rsa -f </path/.ssh/filename>
# Create an OpenSSH RSA key pair and specify a file location and passphrase switch.
# Follow the prompts select the default options:
ssh-keygen -t rsa -f </path/.ssh/filename> -P <"passphrase">
# Change the passphrase for an existing private key
# Follow the prompts to change the key:
ssh-keygen -p -f </path/.ssh/privatekey_file> 
# Or (no prompt):
ssh-keygen -p -f </path/.ssh/privatekey_file> -P <"OldPassphrase"> -N <"NewPassphrase">
# Print corresponding public key by providing the relevant OpenSSH private key:
ssh-keygen -y -f </path/.ssh/privatekey_file>
# Print corresponding public key by providing the relevant OpenSSH private key with passphrase switch:
ssh-keygen -y -f </path/.ssh/privatekey_file> -P <"passphrase">
```
```bash
# Copying OpenSSH keys to remote systems.
# This requires the openssh package and it's dependencies.
# Transfer the public key to the target OpenSSH server using a specific public key file:
ssh-copy-id -i </path/to/public_key_file.pub> <username>@<host>
# Transfer the public key to the target OpenSSH server running on an alternate port:
ssh-copy-id -p 25 <username>@<host>
```
```bash
# Create and configure a new public key pair for authentication using OpenSSH on a new system.
# Create the ssh dot file and configure the permissions:
mkdir ~/.ssh
chmod 700 ~/.ssh
# Create a new RSA key pair
# Follow the prompts select the default options:
ssh-keygen -t rsa
# Transfer the public key to the target OpenSSH server.
# Follow the prompts select yes when prompted for connection and enter the target account password:
ssh-copy-id <username>@<host>
# Or if username is the same on the client as the remote server:
ssh-copy-id <host>
```
```bash
# Configure OpenSSH keys on a new target system without ssh-copy-id (presumes client already configured).
# Create a new OpenSSH key pair.
# Follow the prompts select the default options:
ssh-keygen -t rsa
# Configure the .ssh dot file location on the sshd server:
mkdir ~/.ssh
chmod 700 ~/.ssh
# Using ssh-keygen copy the public key file contents and create the ~/.ssh/authorized_keys file.
# On the client print the OpenSSH public key:
ssh-keygen -y -f </path/.ssh/privatekey_file>
# On the sshd server create the ~/.ssh/authorized_keys file and paste the public key contents:
vi ~/.ssh/authorized_keys
# Set the appropriate permissions:
chmod 600 ~/.ssh/authorized_keys
```
```bash
# Authenticating with ssh-agent.
# This requires the openssh package and it's dependencies.
# ssh-agent doesn't usually run without configuration so it needs to be called initially:
ssh-agent
# Use eval to create a command using the process arguments:
eval $(ssh-agent)
# Add an private key to the ssh-agent cache (enter passphrase if prompted):
ssh-add </path/.ssh/privatekey_file>
```
```bash
# Managing keys used by ssh-agent
# View all the keys stored in the ssh-agent cache:
ssh-add -l
# Remove a key from the cache:
ssh-add -d </path/.ssh/privatekey_file>
# Or use -D to remove all:
ssh-add -D
```

#### ACME:

##### Certbot:

> **Note** Defaults to Let's Encrypt.

```bash
# Show ACME account:
certbot show_account
```
```bash
# Register a new ACME account (use --agree-tos to agree to the TOS):
certbot register --email <smtp>
# Unregister an ACME account:
certbot unregister --account <ACCOUNT_ID> 
```
```bash
# Update the e-mail address on the ACME account:
certbot update_account 
# or:
certbot update_account --email <smtp>
```
```bash
# List working certificates:
certbot certificates
```
```bash
# Generate a new certificate on a server with no current web server software. This uses certbots own web server:
certbot certonly --standalone --preferred-challenges http -d <domain> -m <notice_smtp>
```
```bash
# Generate a new certificate from an alternative machine and perform validation setup manually:
certbot certonly --manual -d <domain> -m <notice_smtp>
# Or using an alternative challenge type like DNS-01:
certbot certonly --manual -d <domain> -m <notice_smtp> --preferred-challenges dns
```
```bash
# Generate a new certificate from an existing CSR:
certbot certonly --manual --csr <path/to/csr> -m <notice_smtp>
# Generate a new certificate from an existing CSR using DNS-01 challenge:
certbot certonly --manual --preferred-challenges dns --csr <path/to/csr> -m <notice_smtp>
```
```bash
# Renew all certificates known by certbot:
certbot renew
# Renew a particular certificate:
certbot renew --cert-name <Certificate Name> 
```
```bash
# Renew and modify existing certificate with --manual and the dns-01 challenge:
certbot certonly --manual --cert-name <> --preferred-challenges dns
# Renew and modify existing certificate with the same or different domains:
certbot certonly --force-renewal -d <existing.com,example.com,newdomain.com> --cert-name <Certificate Name> 
# Renew a particular certificate:
certbot certonly --force-renewal --cert-name <Certificate Name> 
# Renew a particular certificate on an alternate machine
certbot certonly --manual --force-renewal --cert-name <Certificate Name> 
# Renew a particular certificate on an alternate machine with the same or different domains:
certbot certonly --manual --force-renewal -d <existing.com,example.com,newdomain.com> --cert-name <Certificate Name>
# Duplicate an existing certificate which creates a separate certificate file:
certbot certonly --duplicate --cert-name <Certificate Name> 
# Duplicate an existing certificate which creates a separate certificate using an alternate challenge type on an alternate machine: 
certbot certonly --manual --duplicate --cert-name <Certificate Name> --preferred-challenges dns
```
```bash
# Update an existing certificate but with the same plus new domains (does not create the certs alongside the existing one as with --force-renewal) on an alternate machine:
certbot certonly --manual --expand -d <existing.com,example.com,newdomain.com> --cert-name <Certificate Name>
# Update an existing certificate but with the same plus new domains (does not create the certs alongside the existing one as with --force-renewal):
certbot certonly --expand -d <existing.com,example.com,newdomain.com> --cert-name <Certificate Name>
```
```bash
# Revoke a certificate managed by certbot by certificate name:
certbot revoke --cert-name <Certificate Name>
# Revoke by using the private key:
certbot revoke --cert-path <path/to/cert/cert.pem> --key-path <path/to/privkey.pem>
# Generally use a revocation reason:
certbot revoke --cert-name <Certificate Name> --reason {unspecified,keycompromise,affiliationchanged,superseded,cessationofoperation}
# Revoke a certificate managed by certbot and delete:
certbot revoke --cert-name <Certificate Name> --delete-after-revoke
```
```bash
# Delete a certificate managed by certbot (usually for expired certificates only):
certbot delete --cert-name <Certificate Name>
```

##### Posh-ACME:

```powershell
# Set the ACME environement to either staging or production (see https://poshac.me/docs/latest/Tutorial/#picking-a-server for list of shortcuts):
Set-PAServer <'name'>
```
```powershell
# Show ACME account:
Get-PAAccount
```
```powershell
# Update the e-mail address on an ACME account:
Set-PAAccount -ID <'ID'> -Contact <'smtp'>
# or if only one account is registered:
Set-PAAccount -Contact <'smtp'>
```
```powershell
# Create a new LE account:
New-PAAccount -Contact <smtp>
```
```powershell
# Generate a new order:
New-PAOrder -Domain <domain> 
```
```powershell
# Remove an order:
Remove-PAOrder -Name <'name'>
```
```powershell
# These commands automatically manage associated orders and handle the validation process if required.
# Generate a new certificate with a single domain:
New-PACertificate -Domain <'domain'> -Contact <'smtp'>
# Generate a new certificate with multiple domains.
# The first domain in the -Domain list will be the name in the X509 subject field:
New-PACertificate -Domain <'domain1','domain2'> -Contact <'smtp'>
```
```powershell
# Renew a certificate for an existing order (if required) with no DNS plugin:
Get-PAOrder | Submit-Renewal -NoSkipManualDns
# Renew a certificate for an existing order regardless of expiry with no DNS plugin:
Get-PAOrder | Submit-Renewal -NoSkipManualDns -Force
# Renew all certificates for all orders (if required) with no DNS plugin:
Submit-Renewal -AllOrders
```
```powershell
# Review https://poshac.me/docs/v4/Functions/Revoke-PACertificate/#-reason for revocation reasons.
# Revoke certificate(s) for an existing order:
Get-PAOrder -Name <'name'> | Revoke-PACertificate -Reason <RevocationReasons>
# Revoke a certificate by cert file:
Revoke-PACertificate -CertFile <'path\cert.cer'> -Reason <RevocationReasons>
```
```powershell
# Create a new order with a single domain:
New-PAOrder -Domain <'domain'>
# Create a new order with multiple domains.
# The first domain in the -Domain list will be the name in the X509 subject field:
New-PAOrder -Domain <'domain1','domain2'>
# Create a new order with a custom name:
New-PAOrder -Name <'name'> -Domain <'domain'>
# Create a new order with a custom name from an existing CSR:
New-PAOrder -Name <'name'> -CSRPath <'path\csr.csr'>
# Create a new order with a custom name and a specific certificate validity:
New-PAOrder -Name <'name'> -Domain <'domain'> -LifetimeDays <days>
# Create a new order with a custom name where all certificates will install to local machine personal store (requires elevation):
New-PAOrder -Name <'name'> -Domain <'domain'> -Install
# Create a new order with a custom name and change the archive encryption password:
New-PAOrder -Name <'name'> -Domain <'domain'> -PfxPass <password>
```
```powershell
# Get authorisation status of an order, its domains and verification methods:
Get-PAOrder -Name <'name'> | Get-PAAuthorization
# Get HTTP-01 token name for an order:
Get-PAOrder | Get-PAAuthorization | Select-Object -Property HTTP01Token
# Get HTTP-01 token value for an order:
Get-KeyAuthorization -Token (Get-PAOrder -Name <'name'> | Get-PAAuthorization).HTTP01Token
# Get DNS-01 authorisation value (RDATA) for an order:
Get-KeyAuthorization -Token (Get-PAOrder -Name <'name'> | Get-PAAuthorization).DNS01Token -ForDNS
```
```powershell
# Revoke ACME-cached authorisations for an order. This will require re-authorisation for these domains:
Get-PAorder -Name <'name'> | Revoke-PAAuthorization -Force
```
```powershell
# Request the ACME server verify the authorisation methods setup for an order using HTTP-01:
Send-ChallengeAck -ChallengeUrl (Get-PAOrder -Name <'name'> | Get-PAAuthorization).HTTP01Url
# Request the ACME server verify the authorisation methods setup for an order using DNS-01:
Send-ChallengeAck -ChallengeUrl (Get-PAOrder -Name <'name'> | Get-PAAuthorization).DNS01Url
```
```powershell
# Refresh the status of all orders:
Get-PAOrder -Refresh
# Refresh the status of a specific order:
Get-PAOrder -Name <'name'> -Refresh
```
```powershell
# Submit a verified order to the ACME server:
Submit-OrderFinalize -Order (Get-PAOrder -Name <'name'>)
```
```powershell
# Export certificate files to storage:
Complete-PAOrder -Order (Get-PAOrder -Name <'name'>)
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

### Network Analysis:

#### Nmap:

```bash
# Perform host discovery only of a host or range of hosts:
# ICMP echo request, TCP SYN to port 443, TCP ACK to port 80, and an ICMP timestamp request by default.
# Only ARP is done with on-link networks.
nmap -sn <host/CIDR>
```
```bash
# Disable all host discovery (useful if you already know the host is up):
nmap -Pn <Host> 
```
```bash
# Discover and scan a net range but exclude a unicast address or addresses:
nmap <CIDR> --excude <IP.addr IP.addr>
```
```bash
# Discover and scan a host for a specific TCP port:
nmap -sS -p <Int> <Host>
# Combine with UDP ports:
nmap -sS -sU -p <Int> <Host>
```
```bash
# Discover and scan open TCP ports to determine service/version info:
nmap -sS -sV <Host>
# Combine with UDP ports:
# UDP scanning becomes more accurate with the service/version flag
nmap -sS -sU -sV <Host>
```
```bash
# Discover, scan and additionally display decision reasoning on results (useful for UDP scanning):
nmap --reason <host>
```
```bash
# Discover, scan and display only the open ports discovered 
nmap --open <host>
```
```bash
# Additionally fingerprint the OS and provide verbose info:
nmap -o <host> -v
```
```bash
# Show supported IP protocols of a host: 
nmap -sO <host>
```
```bash
# View certificate details of a TLS/SSL connection:
nmap -sS -Pn --script=ssl-cert -p <TLS/SSL_Port> <Host>
```
```bash
# View extended commands available on an SMTP server:
nmap -sS -Pn --script=smtp-commands -p <SMTP_Port> <Host>
# This can be used to verify if STARTTLS is available:
nmap -Pn -sS -p 25 --script=smtp-commands <Host>
```
```bash
# View available ciphers for negotiation of a remote host and offers scoring:
nmap --script=ssl-enum-ciphers -Pn -sS -p <Int> <Host>
```
```bash
# View available scripts
nmap --script-help=*
```
```bash
# Search for a specific script, i.e certificates:
nmap --script-help=*cert*
```
```bash
# Update the script database and show verbosity:
nmap --script-updatedb -v
```
```bash
# Scan with output verbosity
nmap <Host> -v
# Increase this verbosity with -vv
nmap <Host> -vv
```
```bash
# Scan with debug mode:
nmap <Host> -d
# Scan with extra debugging verbosity:
nmap <Host> -dd
```

#### Shodan:

```bash
# Searching Shodan.
# Search for hosts related to Apache:
shodan search Apache
# Search for hosts related to both Apache and Windows:
shodan search Apache Windows
# Search for hosts related to Apache2.2:
shodan search Apache2.2
# Search for hosts related to Apache but only output the IP address and hostname:
shodan search Apache2.2 --fields ip_str,hostnames
# Search for hosts related to Apache but only output the IP address, hostname and OS:
shodan search Apache2.2 --fields ip_str,hostnames,os
# Search for hosts that have a particular string in the hostname:
shodan search hostname:<FQDN/string>
# Search for hosts that relate to a service name of Apache:
shodan search product:Apache
# Search for hosts that relate to a service name of MySQL:
shodan search product:MySQL
# Search for hosts that relate to a service name of cPanel:
shodan search product:cPanel
# Download results to a compressed JSON file:
shodan download <filename.json.gz> <query>
# Use shodan parse to show the results:
shodan parse <filename.json.gz>
```

### Git SCM:

> **Note**: All commands as of version 2.43.x

#### Git config paths:

`[path]/etc/gitconfig`: Applies to all users of the local system (applies to --system)

`~/.gitconfig` or `~/.config/git/config`: Applies to the current user only (applies to --global)

`.git/config`: Applies to the current repository only (applies to --local)

```bash
# Configuring git.
# View all settings and their respective file locations:
git config --list --show-origin
# Get all settings that apply to the user:
git config --list --global
# Get all settings that apply to all users of the system:
git config --list --system
# View all keys and values assigned in all config files:
git config --list
# View a particulular key and value:
git config --get <setting>
# Show the location of the setting Git used:
git config --show-origin --get <setting> 
# If key is multi-valued, use --get-all:
git config --get-all <setting>
# Update a particular key value pair for the current user:
git config --global <setting> <value>
# Create a custom git alias:
git config --global alias.<name> <'git command to be ran'>
```
```bash
# Get help from Git:
git <verb> -h
# Get help from Git via the web:
git help <verb>
# Get help from the man pages (UNIX-like):
man git-<verb>
```
```bash
# Working with repositories in Git.
# Create a new local repository:
git init
# Create a new local repository for sharing with no working tree or .git directory:
git init --bare
# Clone a remote repository:
git clone <PATH>
# Clone a remote repository to a specified location:
git clone <PATH> <filepath>
# Clone a remote repository with more verbosity:
git clone <PATH> --verbose
# Clone a remote repository for sharing with no working tree or .git directory:
git clone <PATH> --bare
# Clone a remote repository with a custom remote name:
git clone <PATH> --origin <name>
# Show all remotes and their URI:
git remote -v
# Add a remote with a custom name:
git remote add <name> <URI>
# Show verbose information on a remote:
git remote show <name>
# Change the custom name of a remote:
git remote rename <name> <new_name>
# Remove a stale remote:
git remote prune <name>
# Remove a remote:
git remote remove <name>
# Fetch all updates from the origin:
get fetch
# Fetch updates from a specific remote:
git fetch <name>
# Fetch updates from all remotes:
git fetch --all
# Push all commits to a remote from the current branch:
git push <remote> HEAD
# Push all commits to a remote from a specific branch:
git push <remote> <branch>
# Push all commits to a remote from a specific local branch but with a different name:
git push <remote> <local_branch>:<remote_branch>
```
```bash
# Working with files and stages in Git.
# View current working tree status:
git status
# View current working tree status in-brief:
git status --short
# Stage or track a file or directory:
git add <name>
# Stage or track multiple files:
git add <name> <name> <name>
# Stage or track multiple files with an expression:
git add <*name*>
# Unstage a file from the staging area and put it back in the working tree:
git restore --staged <file>
# Remove tracked changes from the working tree:
git restore <file>
# Remove all tracked changes from the working tree:
git restore .
# Remove a tracked file:
git rm <name>
# Remove a file from the staging area:
git rm --cached <name> 
# Rename a file:
git mv <file> <newname>
```
```bash
# Working with commits in Git.
# Commit changes in the staging area with the default editor:
git commit
# Commit changes in the staging area with an inline commit message:
git commit -m <"commit_log">
# View diff associated with this commit along with commit message editor:
git commit -v
# Commit all tracked files regardless if they're in the staging area or not:
git commit -a
# or with inline commit message:
git commit -am <"commit_log">
# Amend the last commit with changes in the staging area (use -m for inline commit message). This will replace the last commit:
git commit --amend
# View commit history:
git log
# View latest number of logs:
git log -<#>
# View all commits:
git log --all
# View commit logs that relate only to merge commits:
git log --merges
# View all commits over the last number of weeks:
git log --since=<#>.weeks
# View all commits by author:
git log --author <SMTP | 'name'>
# Search commit messages for a single (use -i for case insensitive or use --no-merges to ignore merge commits):
git log --grep <'string'>
# Search commit messages for an expression (use -i for case insensitive or use --no-merges to ignore merge commits):
git log --grep -E <expression>
# View commit logs when the number of occurrences of the specific string changes, I.e if a new function was added or removed (use --all to search logs across all branches or use --no-merges to ignore merge commits):
git log -S <string>
# View commit logs where a specific string changes (use --all to search logs across all branches or use --no-merges to ignore merge commits):
git log -G <string>
# View commits that changed a specific file:
git log -- <path/to/file>
# View the patch (diffs) with each commit log:
git log --patch
# View a basic stat of what how many files were interacted with:
git log --stat
# View commits on oneline with basic info:
git log --pretty=oneline
# View a graph showing the commit and branch merges:
git log --graph
# View branches pointing to commit:
git log --oneline --decorate
# View full ref name of branches pointing to commit:
git log --oneline --decorate=full
```
```bash
# Working with branches in Git.
# List all local branches:
git branch
# List both remote-tracking and local branches:
git branch -a
# List last commit message on local branch:
git branch -v
# List both remote-tracking and local branches with their last commit message:
git branch -av
# List both remote-tracking and local branches with their last commit message as well as the upstream branch:
git branch -avv
# List branches that havn't yet been merged with HEAD:
git branch --no-merged
# List branches that have been merged with HEAD:
git branch --merged
# List branches that have been merged with a specific branch:
git branch --merged <branch>
# List branches that havn't yet been merged with a specific branch:
git branch --no-merged <branch>
# Create a new local branch: 
git branch <name>
# Create a new local branch and switch to it:
git checkout -b <name>
# If remote tracking branch is available then create a local branch and switch to it:
git checkout <branch>
# Create a new local branch from a remote tracking branch and switch to it:
git checkout -b <localbranch> <remote/branch>
# Set the upstream branch for the current branch:
git branch --set-upstream-to <remote/branch>
# Rename a local branch (use -m for short):
git branch --move <oldname> <newname>
# Rename a local branch (use -m for short) and push to remote:
git branch --move <oldname> <newname>
git push --set-upstream <remote> <branch>
# Push a local branch to a remote and set the remote as upstream:
git push --set-upstream <remote> <branch>
# Delete a local branch:
git branch -d <name>
# Delete a local branch forcefully:
git branch -D <name>
# Delete a remote branch:
git push <remote> --delete <branch>
# Delete a remote tracking branch:
git branch -dr <remote/branch>
```
```bash
# Merging with Git.
# Merge changes between branches:
git merge <src_branch> <branch>
# Merge changes into the current branch (HEAD):
git merge <branch>
# Merge changes into the current branch (HEAD) via a single commit (no preservation of history):
git merge --squash <branch>
git commit
# Merge changes into the current branch (HEAD) and force a merge commit:
git merge --no-ff <branch>
# Merge changes into the current branch (HEAD) from a remote tracking branch:
git merge <remote/branch>
# Perform a dry-run merge where a merge is completed but not committed (does not apply to fast-forward merges):
git merge --no-commit <branch>
# Perform a dry-run merge where a merge is completed but not committed (forces no fast-forward merges):
git merge --no-ff --no-commit <branch>
# Cancel a merge in progress:
git merge --abort
# Viewing merges in an alternate tool.
# View merges in current set mergetool:
git mergetool -y 
# View available tools to use:
git mergetool --tool-help
# View merge in a specific mergetool:
git mergetool --tool=<toolname>
```
```bash
# Working with tags in Git.
# List all tags:
git tag -l
# List specific tag names:
git tag -l <string>
# Show a tag:
git show <tag>
# Create an annotated tag (use -m for inline message):
git tag -a <tag>
# Create a lightweight tag:
git tag <tag>
# Tag a commit directly:
git tag -a <tag> <md5>
# Push tag to remote:
git push <remote> <tag>
# Push all tags to remote:
git push <remote> --tags
# Delete a tag in the local repository:
git tag -d <tag>
# Delete a tag in the remote repository:
git push <remote> --delete <tag>
# Checkout and view files that relate to a specific tag:
git checkout <tag>
```
```bash
# Working with diffs in Git.
# View diff between staging area (index) and working tree:
git diff
# View changes that are staged for the next commit:
git diff --staged
# Viewing diffs in an alternate tool.
# View diff in current set difftool:
git difftool -y
# View available tools to use:
git difftool --tool-help
# View diff in a specific difftool:
git difftool --tool=<toolname>
```

## Containerization:

### 🐳 Docker: 

```bash
# Show Docker version information:
docker version
# Show Docker system information:
docker info
```
```bash
# Working with Docker registries.
# Login to a container registry, default being Docker Hub.
# Using default credsStore:
docker login
# Login using a specific username:
docker login --username <username>
# Also specify the password inline:
docker login --username <username> --password <password>
# Read a password from stdin:
echo ~/my_password.txt | docker login --username <username | smtp> --password-stdin
# Login to a local registry:
docker login --username <username> <hostname:port>
```
```bash
# Search Docker Hub for images:
docker search <string>
# Do not shorten the DESCRIPTION field:
docker search <string> --no-trunc
# Limit the results:
docker search <string> --limit <#>
# Search Docker Hub for images and filter by minimum stars:
docker search <string> --filter stars=<#>
# Search Docker Hub for images and filter for only official images:
docker search <string> --filter is-official=true
```
```bash
# Download / upload Docker images.
# All image locations are from Docker Hub by default.
# Download a Docker image by name (defaults to latest):
docker pull <name>
# Download a Docker image by name and tag:
docker pull <name:tag>
# Download a Docker image by the SHA2 digest:
docker pull <name@sha256:digest>
# Download a Docker image from an alternative registry:
docker pull <FQDN:port/name>
# Pull all Docker images:
docker pull <name> --all-tags
# Push a Docker image to a registry repository:
docker push <name>
```
```bash
# Managing Docker images.
# List all local Docker images:
docker image ls
# Remove a docker image:
docker image rm <name>
# Remove dangling Docker images:
docker image prune
# Remove all unused and dangling Docker images:
docker image prune --all
# Export a docker image to a tarball:
docker image save <name> --output <tarball.tar>
# View detailed information on a Docker image:
docker image inspect <name>
# View in JSON format:
docker image inspect <name> --format json
# List all dangling images:
docker image ls --filter=unused=trueA
# Show detailed information on an existing container:
docker inspect <name>
```
```bash
# Managing Docker containers.
# Show all running containers:
docker ps
# Show all containers:
docker ps --all
# Show last created container:
docker ps --latest
# Show container file size:
docker ps --size <name>
# Show all containers and their file sizes:
docker ps --all --size
# Remove a container:
docker rm <name>
# Stop a container:
docker stop <containerID | name>
# Copy files and directories between containers.
# Copy files and directories from local host to a container:
docker cp <src_path> <containerID | name:dst_path>
# Copy files and directories from a container to local host:
docker cp <containerID | name:src_path> <dst_path>
```
```bash
# Building Docker containers.
# Create a number of base starter files to assist with building a container, optionally select a project type for those starter files.
# Follow the interactive guide:
docker init 
# Create a new container from a Dockerfile (path must contain a Dockerfile):
docker build <path>
# Create a new container from a Dockerfile (path must contain a Dockerfile) with a tag:
docker build --tag <name> <path>
# Create a new image from a container's changes:
docker commit <containerID | name> <name>
# Create a new image from a container's changes with a message/comment:
docker commit --message <containerID | name> <name>
# Create a new image from a container's changes and apply Dockerfile instruction to the created image:
docker commit --change <instruction> <containerID | name> <name>
# Show history changes of an image:
docker history <containerID | name>
```
```bash
# Tag an existing Docker container:
docker tag <containerID | name> <name:tag>
# Or 
docker tag <containerID | name> <tag>
```
```bash
# Running Docker containers.
# If an image is not present locally it will download before running.
# Run a Docker container based on an image:
docker run <image>
# Run a Docker container based on an image with it's own name:
docker run --name <name> <image> 
# Run a Docker container based on an image with an allocated pseudo-TTY in interactive mode (will stop once TTY is exited):
docker run --interactive --tty <image>
# or:
docker run -it <image>
# Run a container based on an image in the background:
docker run --detach <image>
# or:
docker run -d <image>
# Run a container in the background if the ENTRYPOINT process exists:
docker run --detach --interactive --tty <image>
# Run a container based on an image but remove once exited:
docker run --interactive --tty --rm <image>
# Run a container based on an image with a TCP port mapping to the host:
docker run --publish <host_port:container_port> <image>
# or:
docker run -p <host_port:container_port> <image>
# Run a container based on an image with a UDP port mapping:
docker run --publish <host_port/<UDP>:container_port/<UDP>> <image>
# Run a container based on an image with a specific network:
docker run --network <network> <image>
# Run a container based on an image and associate with a current containers network stack:
docker run --network container:<name|id> <image>
# Run a container based on a image using the same IP stack as the host. Port mappings are then direct on the host and --publish/-p is ignored.:
docker run --network host <image>
# Run a container based on an image with an external TCP port mapping (defaults to all interfaces):
docker run --publish <host_port:container_port> <image>
# Run a container based on an image with an external UDP port mapping:
docker run --publish <host_port:container_port/udp> <image>
# Run a container based on an image with an external TCP port mapping to a specific local IP on the host:
docker run --publish <inet_addr:host_port:container_port> <image>
# Run a container based on an image with an external UDP port mapping to a specific local IP on the host:
docker run --publish <inet_addr:host_port:container_port/udp> <image>
# Run a container based on an image with an externalTCP port mapping to localhost only:
docker run --publish 127.0.0.1:host_port:container_port <image>
# Run a container based on an image with a static IP address:
docker run --ip <ip.addr> --network <name> <image>
# Run a container based on an image with a specific hostname:
docker run --hostname <hostname> <image>
# Run a container based on an image with a specific DNS resolver:
docker run --dns <ip.addr> <image>
# Run a container based on an image with a specific DNS search list:
docker run --dns-search <domain> <image>
# Run a container based on an image with and set an environment variable:
docker run --env <variable=value> <image>
# Show all published ports of a running container:
docker port <containerID | name>
# Attach to a running container:
docker attach <ID>
# Start an existing container in detached mode:
docker start <ID | name>
# Start an existing container in interactive mode (will stop once TTY is exited): 
docker start --interactive <containerID | name>
# or:
docker start -i <containerID | name>
```

#### Docker network driver types:

`bridge:` The default network driver. If you don't specify a driver, this is the type of network you are creating. Bridge networks are commonly used when your application runs in a container that needs to communicate with other containers on the same host.

`host:` Remove network isolation between the container and the Docker host, and use the host's networking directly.

`none:` Completely isolate a container from the host and other containers. none is not available for Swarm services.

`overlay:` Overlay networks connect multiple Docker daemons together and enable Swarm services and containers to communicate across nodes. This strategy removes the need to do OS-level routing.

`ipvlan:` IPvlan networks give users total control over both IPv4 and IPv6 addressing. The VLAN driver builds on top of that in giving operators complete control of layer 2 VLAN tagging and even IPvlan L3 routing for users interested in underlay network integration.

`macvlan:` Macvlan networks allow you to assign a MAC address to a container, making it appear as a physical device on your network. The Docker daemon routes traffic to containers by their MAC addresses. Using the macvlan driver is sometimes the best choice when dealing with legacy applications that expect to be directly connected to the physical network, rather than routed through the Docker host's network stack.

```bash
# Docker networking.
# Show all networks:
docker network ls
# Display detailed information on a Docker network:
docker network inspect <networkID>
# Connect a container to a network:
docker network connect <networkID> <containerID>
# Disconnect a container from a network:
docker network disconnect <networkID> <containerID>
# Connect a container to a network and specificy its IPv4 address:
docker network connect --ip <ip.addr> <networkID> <containerID>
# Disconnect a container from a network:
docker network disconnect <networkID> <containerID>
# Create a new docker network with the default bridge driver:
docker network create <name>
# Create a new docker network with a specific driver:
docker network create --driver <driver> <name>
# Create a network with a specific subnet:
docker network create --subnet <cidr> <name>
# Create an l2 IPvlan network:
docker network create --driver ipvlan --subnet <cidr> --gateway <ip.addr> --opt parent=<int> <name>
# Remove a docker network:
docker network rm <networkID>
```

## Desired State Configuration:

### Terraform:

```bash
# Initialize a repository with terraform configuration files:
terraform init
```
```bash
# Validate a repository of terraform files for syntax validity:
terraform validate
```
```bash
# Show the current plan of changes based on the state file (if any):
terraform plan
```
```bash
# Show terraform infrastructure as per the desired state in the local terraform.tfstate file:
terraform show
# Show terraform infrastructure as per the desired state in a specific terraform.tfstate file:
terraform show <filepath>
```
```bash
# Apply changes to infrastructure based on the repository changes (if any):
terraform apply
# With auto approve:
terraform apply -auto-approve
# Apply changes to infrastructure based on the repository changes (if any) and override a variable:
terraform apply -var <"variable=value">
```
```bash
# Import an existing resource into the state file:
terraform import <provider/resource> <ID>
```
```bash
# Show terraform destroy changes:
terraform plan -destroy
# Destroy nfrastructure based on the repository changes (if any):
terraform destroy
# With auto approve:
terraform destroy -auto-approve
```
```bash
# View any output variables after apply:
terraform output
```

## Public Cloud:

### Microsoft 365:

All commands use the [Microsoft Graph PowerShell SDK (Microsoft.Graph)](https://learn.microsoft.com/en-us/powershell/microsoftgraph/get-started?view=graph-powershell-1.0)

**Note**: There are two types of login, **Delegated** or **App-only**. Delegated permits the SDK to interact with the Graph API on your behalf. Where app-only requests an existing registered application to interact with the Graph API. **Delegated** is the default authentication used here, where a default app named Microsoft Graph Command Line Tools will be created automatically.

```powershell
# Finding commands.
# Find a command by name:
Find-MgGraphCommand -Command <Name>
# Find a command by expression:
Find-MgGraphCommand -Command <*name*>
# Find a command by the API URI:
Find-MgGraphCommand -Uri <'.*users.*'>
# Show requires permissions for a specific command:
Find-MgGraphCommand -Command <command> | Select-Object -ExpandProperty Permissions
# Find a command only from the v1 and not the beta API:
Find-MgGraphCommand -Command <Name> -ApiVersion 'v1.0'
```
```powershell
# Finding permissions.
# Find all delegated and application permissions:
Find-MgGraphPermission -All
# Find all delegated permissions:
Find-MgGraphPermission -All -PermissionType Delegated
# Find permissions related to a specific domain, such as either device, user, or application:
Find-MgGraphPermission <'domain'>
# Find the identifier for a specific permission, such as User.ReadWrite.All:
Find-MgGraphPermission <permission>
```
```powershell
# Sign in interactively:
Connect-MgGraph
# Sign in interactively and define a set of permission scopes:
Connect-MgGraph -Scopes <'scope', 'scope'>
# Sign in with a specific application:
Connect-MgGraph -ClientId <app_ID> -TenantId <tenantID>
# Sign in with a device code:
Connect-MgGraph -UseDeviceAuthentication
# View context information about the current user:
Get-MgContext
# View current permissions from the current context:
Get-MgContext | Select-Object -ExpandProperty Scopes
# Disconnect from the Graph API:
Disconnect-MgGraph
```
```powershell
# Invoke API calls directly:
Invoke-MgGraphRequest -Method <method> <URI>
```

#### Microsoft Entra ID:

```powershell
# Requires: User.Read.All
# Get all users:
Get-MgUser
# Get a user by Id:
Get-MgUser -UserId '<Id | UPN>'
```

### Microsoft Azure:

#### Azure Active Directory:

```powershell
# Reset AzureAD user password:
Set-AzureADUserPassword -ObjectId 0fd2daaf-64af-47a7-91c3-d38840eee2c5 -Password (Read-Host -AsSecureString)
```
```bash
# or
# Use --force-change-password-next-login to expire new password
az ad user update --id <objectID> --password <password_string>
```

#### Azure DevOps Services:

```bash
# Create a new Azure AD service connection based on certificate authentication and then associate with a new Azure DevOps service connection in an existing project.
# Create a self-signed certificate with a new private key:
# Follow the interactive guide, use "." to leave certain fields blank.
openssl req -x509 -new -newkey rsa:2048 -nodes -keyout private.key -days 365 -out certificate.pem
# Create PKCS12 archive from a private key and signed PEM certificate:
openssl pkcs12 -in certificate.pem -inkey private.key -export -out certificate.pfx
# Convert from PKCS#12 to PEM format and export private key to a combined PEM file:
openssl pkcs12 -in certificate.pfx -out certificate.pem -nodes
# Create a new AzureAD service principal based on RBAC. Omit both --role and --scope if no assignments are required:
az ad sp create-for-rbac --name <'Name'> --role <definition> --scope <resourceID> --cert <'@certificate.pem'>
# Create a new service connection in an existing Azure DevOps project:
az devops service-endpoint azurerm create --azure-rm-service-principal-id <AppId> --azure-rm-subscription-id <subscriptionId> --azure-rm-subscription-name <subscriptionName> --azure-rm-tenant-id <tenantId> --name <Service Connection Name> --azure-rm-service-principal-certificate-path certificate.pem --project <projectName> --organization <URI>
```

#### Azure Resource Manager:

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
```bash
# Create a new resource group:
az group create --location <Region> --name <ResourceGroupName>
```

#### Azure Policy:

```bash
# List all policy definitions:
az policy definition list
# List all policy definitions installed at a specific scope:
az policy definition list --management-group '<name>'
# Get a policy definition by name:
az policy definition show --name <'name-UID'>
# Get a policy definition by name and scope (if applicable):
az policy definition show --name <'name-UID'> --management-group '<name>'
```

#### Virtual Network:

```bash
# Create a new network security group with default rules:
az network nsg create --name <name> --resource-group <ResourceGroupName> --location <Region>
# # Create a new network security group with default rules with tags:
az network nsg create --name <name> --resource-group <ResourceGroupName> --location <Region> --tags --tags <name=value>
# Show all rules in an NSG:
az network nsg rule list --nsg-name <NSGName> --resource-group <ResourceGroupName> --include-default
# List a specific rule by it's name:
az network nsg rule show --name <RuleName> --nsg-name <NSGName> --resource-group <ResourceGroupName>
# Create a new NSG rule:
az network nsg rule create --nsg-name <NSGName> --resource-group <ResourceGroupName> --Name <Rule_Name> --priority <int> --description <"Desc"> --destination-address-prefixes <cidr> --destination-port-ranges <int> --direction <inbound|outbound> --protocol <proto> --source-address-prefixes <cidr> --access <allow|deny>
# Create an inbound NSG rule that permits destination port TCP 22 to a unicast IP address with priority 100 from the internet: 
az network nsg rule create --name AllowTcpPort22 --nsg-name <NSGName> --resource-group <ResourceGroupName> --priority 100 --access allow --description "Permit TCP port 22" --protocol TCP --destination-port-ranges 22 --destination-address-prefixes <CIDR> --direction inbound
# Create an inbound NSG rule that permits destination port TCP 22 to a unicast IP address with priority 100 from a unicast source IP address: 
az network nsg rule create --name AllowTcpPort22 --nsg-name <NSGName> --resource-group <ResourceGroupName> --priority 100 --access allow --description "Permit TCP port 22" --protocol TCP --source-address-prefixes <CIDR> --destination-port-ranges 22 --destination-address-prefixes <CIDR> --direction inbound
# Create an outbound NSG rule that blocks all traffic to the internet:
az network nsg rule create --name DenyAllInternet --nsg-name <NSGName> --resource-group <ResourceGroupName> --priority <int> --access Deny --description "Drop all internet" --protocol * --source-address-prefixes * --destination-address-prefixes Internet --direction outbound --destination-port-ranges *
# Create an outbound NSG rule that blocks all traffic:
az network nsg rule create --name DenyAll --nsg-name <NSGName> --resource-group <ResourceGroupName> --priority <int> --access Deny --description "Drop all" --protocol * --source-address-prefixes * --destination-address-prefixes * --direction outbound --destination-port-ranges *
# Create an inbound NSG rule that permits traffic to the Azure Storage service tag from all hosts in the VNET:
az network nsg rule create --name AllowAzureStorage --nsg-name <NSGName> --resource-group <ResourceGroupName> --priority <int> --access allow --description "Permit VNET to Azure Storage" --source-address-prefixes * --protocol Tcp --source-address-prefixes VirtualNetwork --destination-address-prefixes Storage --direction outbound --destination-port-ranges *
```
```bash
# Change the priority of an existing NSG rule:
az network nsg rule update --name <RuleName> --nsg-name <NSGName> --resource-group <ResourceGroupName> --priority <int>
# Add an additional unicast IP address into the destination:
az network nsg rule update --name <RuleName> --nsg-name <NSGName> --resource-group <ResourceGroupName> --destination-address-prefixes <CIDR CIDR> # Ensure existing entries are included!
```
```bash
# Create a new static public IP address and configure PTR:
az network public-ip create --name PIP-PRD-UKWEST-001 --resource-group RG-SMTP-PRD-001 --allocation-method static --location ukwest --sku basic --tags Workload=SMTP --tier Regional --version IPv4
```
```bash
az network vnet create --name VNET-SMTP-PRD-UKWEST-001 --resource-group RG-SMTP-PRD-001 --address-prefix 192.168.0.0/16 --subnet-name SNET-SMTP-PRD-001 --subnet-prefix 192.168.0.0/24 --location ukwest --tags Workload=SMTP --network-security-group NSG-SMTP-PRD-UKWEST-001
```
```bash
# Create a new NIC and associate with an existing public IP address:
az network nic create --name NIC-PRD-UKWEST-001 --resource-group RG-SMTP-PRD-001 --vnet-name VNET-SMTP-PRD-UKWEST-001 --subnet SNET-SMTP-PRD-001 --location ukwest
```
```bash
# Peer two vnets, allow vnet traffic as well as forwarded vnet traffic:
az network vnet peering create --name <peering_name> --resource-group <ResourceGroupName> --vnet-name <name> --remote-vnet 'id' --allow-vnet-access --allow-forwarded-traffic
az network vnet peering create --name <peering_name> --resource-group <ResourceGroupName> --vnet-name <name> --remote-vnet 'id' --allow-vnet-access --allow-forwarded-traffic
```
```bash
# Remove a vnet peering between two vnets:
az network vnet peering delete --name <peering_name> --resource-group <ResourceGroupName> --vnet-name <source_vnet_name>
az network vnet peering delete --name <peering_name> --resource-group <ResourceGroupName> --vnet-name <source_vnet_name>
```
```powershell
# Validate effective routes for an existing NIC:
Get-AzEffectiveRouteTable -NetworkInterfaceName <NIC> -ResourceGroupName <ResourceGroupName>
```

#### Azure Private Link:

```bash
# List all private endpoints in the active subscription:
az network private-endpoint list
```
```bash
# Get a private endpoint by name:
az network private-endpoint show --name <name> --resource-group <ResourceGroupName>
# Get a private endpoint by id:
az network private-endpoint show  --ids <id>
```
```bash
# Private link groups (this is needed for any --group-id parameters).
# List all private link groups for an existing resource that supports private link:
az network private-link-resource list --name <resource_name> --resource-group <ResourceGroupName> --type <provider/namespace>
# List all private link groups for an existing resource that supports private link by resourceID:
az network private-link-resource list --id <resourceId>
```
```bash
# Create a new private endpoint and associate it with an existing storage account blob endpoint:
az network private-endpoint create --connection-name <name> --name <endpointName> --private-connection-resource-id <resourceId> --resource-group <ResourceGroupName> --subnet <subnetId> --nic-name <name> --group-id blob
# Create a new private endpoint and associate it with an existing key vault:
az network private-endpoint create --connection-name <name> --name <endpointName> --private-connection-resource-id <resourceId> --resource-group <ResourceGroupName> --subnet <subnetId> --nic-name <name> --group-id vault
```
```bash
# List all DNS zone groups associated with a private endpoint:
az network private-endpoint dns-zone-group list --endpoint-name <endpointName> --resource-group <ResourceGroupName>
```
```bash
# Create a new DNS zone group and associate with an existing private endpoint and Azure private DNS zone:
az network private-endpoint dns-zone-group create --endpoint-name <endpointName>  --name <name> --zone-name <resourceId> --private-dns-zone <partition> --resource-group <ResourceGroupName>
```
```bash
# Add an existing private endpoint DNS zone group into an existing Azure private DNS zone:
az network private-endpoint dns-zone-group add --endpoint-name <endpointName> --name <name> --zone-name <resourceId> --private-dns-zone <partition> --resource-group <ResourceGroupName>
```
```bash
# Remove an a private endpoint DNS zone group from an Azure private DNS zone:
az network private-endpoint dns-zone-group delete --endpoint-name <endpointName> --name <name> --resource-group <ResourceGroupName>
```

#### Azure Load Balancer:

> **Note**: All load balancers are standard SKU.

```bash
# Create an external zone-redudant load balancer with an existing public IP address and custom frontend and backend pool names (ensure --zone includes all zones in the region):
az network public-ip create --name <name> --resource-group <ResourceGroupName> --location <region> --sku standard --zone <# # #>
az network lb create --name <name> --resource-group <ResourceGroupName> --location <region> --sku standard --frontend-ip-name <name> --backend-pool-name <name> --public-ip-address <resource_name>
```
```bash
# Create an external zonal load balancer with an existing public IP address and custom frontend and backend pool names:
az network public-ip create --name <name> --resource-group <ResourceGroupName> --location <region> --sku standard --zone <#>
az network lb create --name <name> --resource-group <ResourceGroupName> --location <region> --sku standard --frontend-ip-name <name> --backend-pool-name <name> --public-ip-address <resource_name>
```
```bash
# Create an external non-zonal load balancer with an existing public IP address and custom frontend and backend pool names:
az network public-ip create --name <name> --resource-group <ResourceGroupName> --location <region> --sku standard
az network lb create --name <name> --resource-group <ResourceGroupName> --location <region> --sku standard --frontend-ip-name <name> --backend-pool-name <name> --public-ip-address <resource_name>
```
```bash
# Create an internal non-zonal load balancer with a dynamic private IP address from an existing VNET and custom frontend and backend pool names:
az network lb create --name <name> --resource-group <ResourceGroupName> --sku standard --frontend-ip-name <name> --backend-pool-name <name> --subnet <subnet_id>
# Create an internal zonal load balancer with a dynamic private IP address from an existing VNET and custom frontend and backend pool names:
az network lb create --name <name> --resource-group <ResourceGroupName> --sku standard --frontend-ip-name <name> --backend-pool-name <name> --subnet <subnet_id> --frontend-ip-zone <#>
```
```bash
# Delete a load balancer:
az network lb delete --name <name> --resource-group <ResourceGroupName>
# Delete a load balancer by ID:
az network lb delete --ids <resource_id>
```
```bash
# Create a new front end pool on an existing external load balancer with an existing public IP address:
az network lb frontend-ip create --name <name> --lb-name <name> --resource-group <ResourceGroupName> --public-ip-address <resource_name>
```

#### Azure DNS:

```bash
# Create a new Azure DNS zone:
az network dns zone create --name <fqdn> --resource-group <ResourceGroupName>
# Create a new Azure DNS zone that is a delegation of an existing zone in Azure:
az network dns zone create --name <fqdn> --parent-name <fqdn | id> --resource-group <ResourceGroupName>
# Remove an Azure DNS zone:
az network dns zone delete --name <fqdn> --resource-group <ResourceGroupName>
```
```bash
# View an Azure DNS zone:
az network dns zone show --name <fqdn> --resource-group <ResourceGroupName>
# List all records sets in an Azure DNS zone:
az network dns record-set list --zone-name <fqdn> --resource-group <ResourceGroupName>
# List all A record sets within an Azure DNS zone:
az network dns record-set a list --zone-name <fqdn> --resource-group <ResourceGroupName>
# List all CNAME record sets within an Azure DNS zone:
az network dns record-set cname list --zone-name <fqdn> --resource-group <ResourceGroupName>
# Show a CNAME record set within an Azure DNS zone:
az network dns record-set cname show --name <recordset> --zone-name <fqdn> --resource-group <ResourceGroupName>
# List all MX record sets within an Azure DNS zone:
az network dns record-set mx list --zone-name <fqdn> --resource-group <ResourceGroupName>
# List all TXT record sets within an Azure DNS zone:
az network dns record-set txt txt --zone-name <fqdn> --resource-group <ResourceGroupName>
# Show a TXT record set within an Azure DNS zone:
az network dns record-set txt show --name <recordset> --zone-name <fqdn> --resource-group <ResourceGroupName>
# Create a new TXT record set within an Azure DNS zone:
az network dns record-set txt add-record --record-set-name <recordset> --value <string> --zone-name <fqdn> --resource-group <ResourceGroupName>
# Create a new TXT record set within an Azure DNS zone at the zone apex:
az network dns record-set txt add-record --record-set-name @ --value <string> --zone-name <fqdn> --resource-group <ResourceGroupName>
# Create a new MX record set within an Azure DNS zone at the zone apex:
az network dns record-set mx add-record --record-set-name @ --zone-name <fqdn> --resource-group <ResourceGroupName> --exchange <fqdn> --preference <int>
# Create a new A record set within an Azure DNS zone: 
az network dns record-set a add-record --record-set-name <recordset> --ipv4-address <IPV4> --zone-name <fqdn> --resource-group <ResourceGroupName>
# Remove an A record set from an Azure DNS zone:
az network dns record-set a delete --name <recordset> --zone-name <fqdn> --resource-group <ResourceGroupName>
# Remove a CNAME record set from an Azure DNS zone:
az network dns record-set cname delete --name <recordset> --zone-name <fqdn> --resource-group <ResourceGroupName>
# Remove a TXT record set from an Azure DNS zone at the apex:
az network dns record-set txt delete --name @ --zone-name <fqdn> --resource-group <ResourceGroupName>
```

```bash
# Create a new private DNS zone:
az network private-dns zone create --name <fqdn> --resource-group <ResourceGroupName>
# Link an existing private DNS zone to a VNET:
az network private-dns link vnet create --name <link_name> --zone-name <fqdn> --resource-group <ResourceGroupName> --virtual-network <vnetID> --registration-enabled false
# Show all associated VNET links:
az network private-dns link vnet list --zone-name <fqdn> --resource-group <ResourceGroupName>
# Remove an existing VNET link:
az network private-dns link vnet delete --name <link_name> --zone-name <fqdn> --resource-group <ResourceGroupName>
# Remove private DNS zone (ensure all links are removed):
az network private-dns zone delete --name <fqdn> --resource-group <ResourceGroupName>
```
```bash
# List all record sets within a private DNS zone:
az network private-dns record-set list --zone-name <fqdn> --resource-group <ResourceGroupName>
# List all A record sets within a private DNS zone:
az network private-dns record-set a list --zone-name <fqdn> --resource-group <ResourceGroupName>
# List all CNAME record sets within a private DNS zone:
az network private-dns record-set cname list --zone-name <fqdn> --resource-group <ResourceGroupName>
# Show a CNAME record set within a private DNS zone:
az network private-dns record-set cname show --name <recordset> --zone-name <fqdn> --resource-group <ResourceGroupName>
# List all MX record sets within a private DNS zone:
az network private-dns record-set mx list --zone-name <fqdn> --resource-group <ResourceGroupName>
# List all TXT record sets within a private DNS zone:
az network private-dns record-set txt list --zone-name <fqdn> --resource-group <ResourceGroupName>
# Show a TXT record set within a private DNS zone:
az network private-dns record-set txt show --name <recordset> --zone-name <fqdn> --resource-group <ResourceGroupName>
# Create a new TXT record set within a private DNS zone:
az network private-dns record-set txt add-record --record-set-name <recordset> --value <string> --zone-name <fqdn> --resource-group <ResourceGroupName>
# Create a new TXT record set within a private DNS zone at the zone apex:
az network private-dns record-set txt add-record --record-set-name @ --value <string> --zone-name <fqdn> --resource-group <ResourceGroupName>
# Create a new MX record set within a private DNS zone at the zone apex:
az network private-dns record-set mx add-record --record-set-name @ --zone-name <fqdn> --resource-group <ResourceGroupName> --exchange <fqdn> --preference <int>
# Create a new A record set within a private DNS zone: 
az network private-dns record-set a add-record --record-set-name <recordset> --ipv4-address <IPV4> --zone-name <fqdn> --resource-group <ResourceGroupName>
# Remove an A record set from a private DNS zone:
az network private-dns record-set a delete --name <recordset> --zone-name <fqdn> --resource-group <ResourceGroupName>
# Remove a CNAME record set from a private DNS zone:
az network private-dns record-set cname delete --name <recordset> --zone-name <fqdn> --resource-group <ResourceGroupName>
# Remove a TXT record set from a private DNS zone at the apex:
az network private-dns record-set txt delete --name @ --zone-name <fqdn> --resource-group <ResourceGroupName>
```

#### Storage:

> ℹ️ **Note:** --upload-size-bytes requires a value that is +512 bytes of a value divisible by 1024.

```bash
# Create a disk for the purpose of uploading from an alternate source.
# This disk is a standard disk and not an OS disk.
# Create the disk:
az disk create --name <DiskName> --resource-group <ResourceGroupName> --upload-type Upload --location <Region> --sku <SKU> --upload-size-bytes <FileSize>
# Permit access to upload to this disk:
az disk grant-access --name <DiskName> --resource-group <ResourceGroupName> --access-level Write --duration-in-seconds <Int>
# Upload the VHD to the accessSas URI Azcopy is easies:
azcopy copy <path/to/vhd> <"accessSas"> --blob-type PageBlob
# Finish by revoking the access to the disk:
az disk revoke-access --name <DiskName> --resource-group <ResourceGroupName>
```

#### Virtual Machines:

```bash
# Create a Linux VM from the marketplace using an existing NIC, Public IP, and subnet:
az vm create --name <vmName> --resource-group <resourceGroupName> --image <UrnAlias> --generate-ssh-keys --size <vmSize> --os-disk-name <name> --location <location> --enable-agent true --computer-name <computerName> --nics nic-prd-ukwest-001 --tags <name=value> 
```
```bash
# Create a Linux VM from the marketplace using an existing NIC, subnet, with agent installed, an MSI and using Premium LRS for the managed OS disk. If this VM is deleted it will delete the NIC, OS disk but detach and leave any data disks:
az vm create --name <vmName> --resource-group <resourceGroupName> --image <UrnAlias> --generate-ssh-keys --size <vmSize> --os-disk-name <name> --location <location> --enable-agent true --computer-name <computerName> --nics nic-prd-ukwest-001 --tags <name=value> --assign-identity --nic-delete-option Delete --os-disk-delete-option Delete --storage-sku Premium_LRS --data-disk-delete-option Detach
```
```bash
# Deploy a Debian 11 Linux VM using the latest gen2 image with key-based authentication, agent enabled, hostname pre-configured, with a specific sized OS disk of 31GB and to an existing VNET subnet:
az vm create --name <vmname> --resource-group <resourceGroupName> --image Debian:debian-11-daily:11-gen2:latest --generate-ssh-keys --location <region> --enable-agent true --computer-name <hostname> --subnet <subnetID> --os-disk-size-gb 31
# Deploy the same VM but with two extra data disks of 1GB each and with a specific cheap VM size:
az vm create --name <vmname> --resource-group <resourceGroupName> --image Debian:debian-11-daily:11-gen2:latest --generate-ssh-keys --location <region> --enable-agent true --computer-name <hostname> --subnet <subnetID> --os-disk-size-gb 31 --data-disk-sizes-gb 1 1 --size Standard_B1ls
# Deploy the same VM but the OS and data disks and the NICs will be deleted with the VM:
az vm create --name <vmname> --resource-group <resourceGroupName> --image Debian:debian-11-daily:11-gen2:latest --generate-ssh-keys --location <region> --enable-agent true --computer-name <hostname> --subnet <subnetID> --os-disk-size-gb 31 --data-disk-sizes-gb 1 1 --size Standard_B1ls --os-disk-delete-option Delete --data-disk-delete-option Delete --nic-delete-option Delete
# Deploy a Debian Linux VM with a custom username and key-based authentication to a specific location:
az vm create --name <vmname> --resource-group <resourceGroupName> --image Debian:debian-11-daily:11-gen2:latest --generate-ssh-keys location uksouth --admin-username <username> --size Standard_B1ls 
# Deploy a Debian Linux VM and associate with an existing NIC with no public IP address:
az vm create --name <vmname> --resource-group <resourceGroupName> --image Debian:debian-11-daily:11-gen2:latest --location uksouth --public-ip-address "" --nic <NICname>
```
```bash
# Create a new auto shutdown configuration:
az vm auto-shutdown --resource-group <resourceGroupName> --name <vmName> --time <UTC> --email <"SMTP">
```
```bash
# List available VM extensions in the Azure CLI:
az vm extension image list --latest
```

##### VM Images, sizes and SKUs:

- **Publisher**: The organization that created the image. Examples: Canonical, MicrosoftWindowsServer
- **Offer**: The name of a group of related images created by a publisher. Examples: UbuntuServer, WindowsServer
- **SKU**: An instance of an offer, such as a major release of a distribution. Examples: 18.04-LTS, 2019-Datacenter
- **Version**: The version number of an image SKU.

```bash
# List all publishers available at a specific location:
az vm image list-publishers --location <region>
# List all publishers available at a specific location that contains case-sensitive specific string in the name:
az vm image list-publishers -l <location> --query "[?contains(name, <'string'>)]"
# List all offers at a specific location from the a publisher:
az vm image list-offers --publisher <name> --location <region>
# List all offers at a specific location from the RedHat publisher:
az vm image list-offers --publisher RedHat --location <region>
# List all SKUs available under a specific offer at a specific location:
az vm image list-skus --location <region> --publisher <Name> --offer <Name>
# List all SKUs available under a specific offer, ie RHEL at a specific location:
az vm image list-skus --location <region> --publisher RedHat --offer RHEL
# List all images based on a specific SKU from the online list at a specific locations
az vm image list --location <location> --sku <name> --all
# Get an image from the Marketplace on based on it's URN at a specific location:
az vm image show --urn <publisher:offer:sku:version> --location <region>
# Get an image from the Marketplace on based on it's URN at a specific location that is the latest:
az vm image show --urn <publisher:offer:sku:latest> --location <region>
# List images provided from a specific offer, such as CentOS or RHEL (usually latest) from specific location:
az vm image list --offer <offer> --location <region>
# List all images provided from a specific offer, such as CentOS or RHEL:
az vm image list --offer <offer> --all
# List all images provided from a specific offer, such as CentOS or RHEL from a specific location:
az vm image list --offer <offer> --all --location <region>
# List all images provided from a specific offer, such as CentOS or RHEL from a specific region
az vm image list --offer <offer> --location <region>
# List all images in the Marketplace from the online list:
az vm image list --all
# List all images in the Marketplace from a specific region from the online list:
az vm image list --location <region> --all
# List all images from a specific publisher:
az vm image list --publisher <Publisher>
# List all images from Debian in a specific region:
az vm image list --publisher Debian --location <region>
```
```bash
# List all VM sizes in a specific region:
az vm list-sizes --location <region>
```

```powershell
# Get all images in a specific region:
$Skus = Get-AzVMImagePublisher -Location <'region'> | Get-AzVMImageOffer | Get-AzVMImageSku
foreach ($sku in $skus) {
    $skus | Get-AzVMImage | Get-AzVMImage | Where-Object -Property PurchasePlan -NE $null | Select-Object -Property PublisherName,Offer,Skus,Name}
```

#### VM Extensions:

```bash
# List all VM extensions available globally:
az vm extension image list
# List all VM extensions available in a specific region (omit --latest to show all versions):
az vm extension image list --location <region> --latest
# List all VM extensions by publisher (omit --latest to show all versions):
az vm extension image list --publisher <name> --location <region> --latest
# List all VM extensions available by name (omit --latest to show all versions):
az vm extension image list --name <name> --latest
# List all VM extensions available by name in the current region (omit --latest to show all versions):
az vm extension image list --name <name> --location <region> --latest
```
```bash
# Show a VM extension:
az vm extension image show --name <Name> --publisher <Publisher> --version <#> --location <Region>
# Show a VM extension by ID:
az vm extension image show --ids <ID>
```
```bash
# List all VM extensions attached to a VM:
az vm extension list --resource-group <resourceGroupName> --vm-name <Name>
```
```bash
# Show a VM extension attached to a VM:
az vm extension show --resource-group <resourceGroupName> --vm-name <Name> --name <Name>
```
```bash
# Install a VM extension to a VM:
az vm extension set --name <Name> --publisher <Publisher> --resource-group <resourceGroupName> --vm-name <Name>
# Install a VM extension to a VM via VM resource ID:
az vm extension set --name <Name> --publisher <Publisher> --ids <ResourceID>
# Force an update of a VM extension:
az vm extension set --name <Name> --publisher <Publisher> --resource-group <resourceGroupName> --vm-name <Name> --force-update
```
```bash
# Uninstall a VM extension from a VM:
az vm extension delete --name <Name> --resource-group <resourceGroupName> --vm-name <Name>
```

#### Azure Key Vault:

```bash
# Create a new AKV:
az keyvault create --name <name> --resource-group <resourcegroupname>  --location <region>
# Create a new AKV with purge protection enabled: 
az keyvault create --name <name> --resource-group <resourcegroupname>  --location <region> --enable-purge-protection true
# Create a new AKV with AzureAD authorization and with 10 days data retention for deleted entries::  
az keyvault create --name <name> --resource-group <resourcegroupname>  --location <region> --enable-rbac-authorization true
az keyvault create --name <name> --resource-group <resourcegroupname>  --location <region> --enable-rbac-authorization true
az keyvault create --name <name> --resource-group <resourcegroupname>  --location <region> --enable-rbac-authorization true --retention-days 10
```