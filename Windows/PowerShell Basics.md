# PowerShell Basics

PowerShell is a task automation and configuration management framework from Microsoft.

## 🚀 Getting Started

### What is PowerShell?
- Cross-platform (Windows, Linux, macOS)
- Object-oriented (not text-based like traditional shells)
- Built on .NET
- Verb-Noun command structure

### Versions
- **Windows PowerShell**: 5.1 (built into Windows)
- **PowerShell Core**: 7+ (cross-platform)

### Opening PowerShell
```powershell
# Windows
Win + X → Windows PowerShell
# Or search for "PowerShell"

# Run as Administrator
Right-click → Run as Administrator
```

## 📝 Basic Syntax

### Cmdlets (Commands)
PowerShell commands follow a Verb-Noun pattern:
```powershell
Get-Command                 # List all commands
Get-Help                    # Get help
Get-Process                 # Get running processes
Get-Service                 # Get services
```

### Common Verbs
- **Get**: Retrieve information
- **Set**: Modify settings
- **New**: Create new items
- **Remove**: Delete items
- **Start**: Start processes/services
- **Stop**: Stop processes/services
- **Restart**: Restart processes/services
- **Test**: Test conditions
- **Enable/Disable**: Enable or disable features

## 🔍 Getting Help

```powershell
# Get help for a command
Get-Help Get-Process
Get-Help Get-Process -Full
Get-Help Get-Process -Examples
Get-Help Get-Process -Online

# Update help files
Update-Help

# Search for commands
Get-Command *process*
Get-Command -Verb Get
Get-Command -Noun Service
```

## 📁 File System Operations

### Navigation
```powershell
# Get current location
Get-Location
# Alias: pwd, gl

# Change directory
Set-Location C:\Users
# Alias: cd, chdir, sl

# List files and directories
Get-ChildItem
# Alias: ls, dir, gci

Get-ChildItem -Path C:\Windows -Recurse  # Recursive
Get-ChildItem -Filter *.txt              # Filter by extension
Get-ChildItem -File                      # Files only
Get-ChildItem -Directory                 # Directories only
Get-ChildItem -Hidden                    # Hidden items
```

### File Operations
```powershell
# Create new file
New-Item -Path file.txt -ItemType File
New-Item -Path C:\Temp -ItemType Directory

# Copy files
Copy-Item source.txt destination.txt
Copy-Item C:\Source C:\Dest -Recurse    # Copy directory

# Move files
Move-Item file.txt C:\Destination\

# Remove files
Remove-Item file.txt
Remove-Item C:\Temp -Recurse -Force     # Remove directory

# Rename
Rename-Item oldname.txt newname.txt

# Get file content
Get-Content file.txt
Get-Content file.txt -Head 10           # First 10 lines
Get-Content file.txt -Tail 10           # Last 10 lines
Get-Content file.txt -Wait              # Monitor file (like tail -f)

# Set file content
Set-Content file.txt "New content"
Add-Content file.txt "Append this"

# File properties
Get-Item file.txt
Get-ItemProperty file.txt
Test-Path C:\file.txt                   # Check if exists
```

## 🔧 System Information

```powershell
# Computer info
Get-ComputerInfo
Get-ComputerInfo | Select-Object CsName, OsArchitecture, WindowsVersion

# System information
systeminfo                              # Classic command

# Hostname
hostname
$env:COMPUTERNAME

# OS version
Get-CimInstance Win32_OperatingSystem
[System.Environment]::OSVersion

# BIOS info
Get-CimInstance Win32_BIOS

# CPU info
Get-CimInstance Win32_Processor

# Memory info
Get-CimInstance Win32_PhysicalMemory
Get-CimInstance Win32_ComputerSystem | Select-Object TotalPhysicalMemory

# Disk info
Get-PSDrive
Get-Disk
Get-Volume
Get-CimInstance Win32_LogicalDisk
```

## 🔄 Process Management

```powershell
# List processes
Get-Process
Get-Process | Sort-Object CPU -Descending
Get-Process -Name chrome
Get-Process | Where-Object {$_.CPU -gt 100}

# Start process
Start-Process notepad
Start-Process -FilePath "C:\Program.exe" -ArgumentList "/arg1", "/arg2"

# Stop process
Stop-Process -Name notepad
Stop-Process -Id 1234
Stop-Process -Name chrome -Force

# Process details
Get-Process chrome | Select-Object *
```

## 🛠️ Service Management

```powershell
# List services
Get-Service
Get-Service | Where-Object {$_.Status -eq 'Running'}
Get-Service -Name wuauserv

# Start/Stop/Restart service
Start-Service -Name wuauserv
Stop-Service -Name wuauserv
Restart-Service -Name wuauserv

# Service status
Get-Service -Name wuauserv | Select-Object Name, Status, StartType

# Set service startup type
Set-Service -Name wuauserv -StartupType Automatic
# Options: Automatic, Manual, Disabled
```

## 🌐 Network Commands

```powershell
# IP configuration
Get-NetIPAddress
Get-NetIPConfiguration
ipconfig                                # Classic command
ipconfig /all

# Test connection (ping)
Test-Connection google.com
Test-Connection -ComputerName server01 -Count 4

# DNS lookup
Resolve-DnsName google.com
nslookup google.com                     # Classic command

# Network adapters
Get-NetAdapter
Get-NetAdapter | Select-Object Name, Status, LinkSpeed

# Routing table
Get-NetRoute
route print                             # Classic command

# Port scanning
Test-NetConnection -ComputerName server01 -Port 80
Test-NetConnection google.com -TraceRoute

# Active connections
Get-NetTCPConnection
Get-NetTCPConnection -State Established
netstat -ano                            # Classic command
```

## 👥 User Management

```powershell
# Current user
$env:USERNAME
whoami

# Local users
Get-LocalUser
Get-LocalUser -Name Administrator

# Create user
New-LocalUser -Name "John" -Description "Test User"
New-LocalUser -Name "John" -Password (ConvertTo-SecureString "P@ssw0rd" -AsPlainText -Force)

# Remove user
Remove-LocalUser -Name "John"

# Local groups
Get-LocalGroup
Get-LocalGroupMember -Group "Administrators"

# Add user to group
Add-LocalGroupMember -Group "Administrators" -Member "John"
```

## 📊 Variables and Data Types

```powershell
# Variables
$name = "John"
$age = 30
$price = 19.99
$isActive = $true

# Arrays
$array = @(1, 2, 3, 4, 5)
$array = @("Apple", "Banana", "Orange")
$array[0]                               # Access element
$array.Length                           # Array length

# Hash tables (Dictionaries)
$hash = @{
    Name = "John"
    Age = 30
    City = "New York"
}
$hash["Name"]                           # Access value
$hash.Age                               # Alternative syntax

# Environment variables
$env:PATH
$env:USERPROFILE
$env:COMPUTERNAME
```

## 🔀 Operators

```powershell
# Comparison operators
-eq     # Equal
-ne     # Not equal
-gt     # Greater than
-ge     # Greater than or equal
-lt     # Less than
-le     # Less than or equal
-like   # Wildcard comparison
-match  # Regex match

# Logical operators
-and    # Logical AND
-or     # Logical OR
-not    # Logical NOT
!       # NOT (alternative)

# Examples
5 -eq 5                                 # True
"hello" -like "hel*"                    # True
"test@email.com" -match "^\w+@\w+\.\w+$"  # True
```

## 🔄 Pipeline

```powershell
# Pipeline basics
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10

# Where-Object (filtering)
Get-Process | Where-Object {$_.CPU -gt 100}
Get-Service | Where-Object {$_.Status -eq 'Running'}

# Select-Object (selecting properties)
Get-Process | Select-Object Name, CPU, Memory
Get-Process | Select-Object -First 5
Get-Process | Select-Object -Unique Name

# ForEach-Object (iteration)
Get-Process | ForEach-Object {$_.Name.ToUpper()}
1..10 | ForEach-Object {$_ * 2}

# Measure-Object (statistics)
Get-ChildItem | Measure-Object -Property Length -Sum -Average -Maximum
Get-Process | Measure-Object -Property CPU -Sum

# Group-Object
Get-Process | Group-Object -Property ProcessName
Get-Service | Group-Object -Property Status
```

## 📝 Output and Formatting

```powershell
# Format as table
Get-Process | Format-Table Name, CPU, Memory
Get-Process | ft Name, CPU                  # Alias

# Format as list
Get-Process chrome | Format-List *
Get-Process chrome | fl *                    # Alias

# Format as wide
Get-Process | Format-Wide Name
Get-Process | fw Name                        # Alias

# Export to file
Get-Process | Export-Csv processes.csv
Get-Process | Export-CliXml processes.xml
Get-Process | ConvertTo-Json | Out-File processes.json

# Import from file
Import-Csv processes.csv
Import-CliXml processes.xml
Get-Content processes.json | ConvertFrom-Json

# Output to file
Get-Process | Out-File processes.txt
Get-Process > processes.txt                  # Redirect
Get-Process >> processes.txt                 # Append
```

## 🎯 Useful Aliases

PowerShell includes many aliases for common commands:

```powershell
# Get all aliases
Get-Alias

# Common aliases
ls      → Get-ChildItem
dir     → Get-ChildItem
cd      → Set-Location
pwd     → Get-Location
cat     → Get-Content
cp      → Copy-Item
mv      → Move-Item
rm      → Remove-Item
cls     → Clear-Host
man     → Get-Help

# Create custom alias
New-Alias -Name np -Value notepad
Set-Alias -Name list -Value Get-ChildItem
```

## 🔧 Execution Policy

```powershell
# Check execution policy
Get-ExecutionPolicy

# Set execution policy
Set-ExecutionPolicy RemoteSigned
# Options:
# - Restricted (default on Windows client)
# - AllSigned
# - RemoteSigned (recommended)
# - Unrestricted
# - Bypass

# Set for current user only
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```

## 💡 Tips and Best Practices

1. **Use Tab Completion**: Press Tab to autocomplete commands and parameters
2. **Use Up/Down arrows**: Navigate command history
3. **Use ISE**: PowerShell ISE provides a GUI for script development
4. **Use VS Code**: Modern alternative with PowerShell extension
5. **Comment your code**: Use `#` for single-line comments
6. **Use proper naming**: Follow Verb-Noun convention for functions

## Related Topics

- [[PowerShell Commands]]
- [[PowerShell Scripting]]
- [[Cheat Sheets/PowerShell Commands|PowerShell Commands Cheat Sheet]]
- [[Windows Basics]]

---

#powershell #windows #fundamentals #sysadmin
