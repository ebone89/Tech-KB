# PowerShell Commands Cheat Sheet

Quick reference for essential PowerShell commands.

## 🚀 Basics

```powershell
# Get help
Get-Help Get-Process
Get-Help Get-Process -Examples
Get-Command *process*

# Execution policy
Get-ExecutionPolicy
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```

## 📁 File System

```powershell
# Navigation
Get-Location            # pwd, gl
Set-Location C:\Path    # cd, sl
Get-ChildItem           # ls, dir, gci
Get-ChildItem -Recurse -Filter *.txt

# File operations
New-Item file.txt -ItemType File
New-Item C:\Temp -ItemType Directory
Copy-Item source.txt dest.txt
Copy-Item C:\Source C:\Dest -Recurse
Move-Item file.txt C:\Dest\
Remove-Item file.txt
Remove-Item C:\Temp -Recurse -Force
Rename-Item old.txt new.txt

# File content
Get-Content file.txt
Get-Content file.txt -Head 10
Get-Content file.txt -Tail 10
Get-Content file.txt -Wait        # Like tail -f
Set-Content file.txt "Content"
Add-Content file.txt "More"
```

## 🔍 Search and Filter

```powershell
# Find files
Get-ChildItem -Path C:\ -Filter *.log -Recurse
Get-ChildItem | Where-Object {$_.Length -gt 1MB}
Get-ChildItem | Where-Object {$_.LastWriteTime -gt (Get-Date).AddDays(-7)}

# Search content
Select-String -Path *.txt -Pattern "error"
Get-Content file.txt | Select-String "pattern"
```

## 🔄 Process Management

```powershell
# View processes
Get-Process
Get-Process | Sort-Object CPU -Descending
Get-Process -Name chrome
Get-Process | Where-Object {$_.CPU -gt 100}

# Start/Stop processes
Start-Process notepad
Start-Process -FilePath program.exe -ArgumentList "/arg"
Stop-Process -Name notepad
Stop-Process -Id 1234 -Force
```

## 🛠️ Service Management

```powershell
# List services
Get-Service
Get-Service | Where-Object {$_.Status -eq 'Running'}
Get-Service -Name wuauserv

# Control services
Start-Service -Name wuauserv
Stop-Service -Name wuauserv
Restart-Service -Name wuauserv
Set-Service -Name wuauserv -StartupType Automatic
# Startup types: Automatic, Manual, Disabled
```

## 🌐 Network Commands

```powershell
# IP configuration
Get-NetIPAddress
Get-NetIPConfiguration
ipconfig /all

# Test connectivity
Test-Connection google.com
Test-Connection -ComputerName server01 -Count 4

# DNS lookup
Resolve-DnsName google.com
nslookup google.com

# Network adapters
Get-NetAdapter
Get-NetAdapter | Select-Object Name, Status, LinkSpeed

# Port testing
Test-NetConnection server01 -Port 80
Test-NetConnection google.com -TraceRoute

# Connections
Get-NetTCPConnection
Get-NetTCPConnection -State Established
netstat -ano
```

## 👥 User Management

```powershell
# Current user
$env:USERNAME
whoami

# Local users
Get-LocalUser
Get-LocalUser -Name Administrator
New-LocalUser -Name "John" -Description "Test User"
Remove-LocalUser -Name "John"

# Groups
Get-LocalGroup
Get-LocalGroupMember -Group "Administrators"
Add-LocalGroupMember -Group "Administrators" -Member "John"
```

## 📊 System Information

```powershell
# Computer info
Get-ComputerInfo
Get-ComputerInfo | Select-Object CsName, WindowsVersion

# OS version
Get-CimInstance Win32_OperatingSystem

# Hardware
Get-CimInstance Win32_Processor
Get-CimInstance Win32_PhysicalMemory
Get-CimInstance Win32_LogicalDisk

# Disk info
Get-PSDrive
Get-Disk
Get-Volume
```

## 📝 Variables and Objects

```powershell
# Variables
$name = "John"
$number = 42
$array = @(1, 2, 3, 4)
$hash = @{Name="John"; Age=30}

# Environment variables
$env:PATH
$env:USERPROFILE
$env:COMPUTERNAME

# Object properties
Get-Process | Select-Object Name, CPU, Memory
Get-Process | Get-Member
```

## 🔀 Pipeline and Filtering

```powershell
# Where-Object (filtering)
Get-Process | Where-Object {$_.CPU -gt 100}
Get-Service | Where-Object Status -eq 'Running'

# Select-Object
Get-Process | Select-Object Name, CPU
Get-Process | Select-Object -First 5
Get-Process | Select-Object -Unique Name

# ForEach-Object
Get-Process | ForEach-Object {$_.Name.ToUpper()}
1..10 | ForEach-Object {$_ * 2}

# Sort-Object
Get-Process | Sort-Object CPU -Descending

# Measure-Object
Get-ChildItem | Measure-Object -Property Length -Sum
Get-Process | Measure-Object -Property CPU -Average
```

## 📤 Export and Import

```powershell
# Export
Get-Process | Export-Csv processes.csv
Get-Process | Export-Clixml processes.xml
Get-Process | ConvertTo-Json | Out-File processes.json

# Import
Import-Csv processes.csv
Import-Clixml processes.xml
Get-Content processes.json | ConvertFrom-Json

# Output to file
Get-Process | Out-File processes.txt
Get-Process > processes.txt
Get-Process >> processes.txt    # Append
```

## 🔧 Registry

```powershell
# Navigate registry
Set-Location HKLM:\Software
Get-ChildItem HKCU:\Software

# Read registry value
Get-ItemProperty -Path "HKLM:\Software\Microsoft\Windows\CurrentVersion"

# Set registry value
Set-ItemProperty -Path "HKCU:\Software\MyApp" -Name "Setting" -Value "Value"

# Create registry key
New-Item -Path "HKCU:\Software\MyApp"
New-ItemProperty -Path "HKCU:\Software\MyApp" -Name "Setting" -Value "Value"
```

## 🔐 Security

```powershell
# Run as Administrator
Start-Process powershell -Verb RunAs

# Credentials
$cred = Get-Credential
$password = ConvertTo-SecureString "P@ssw0rd" -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("user", $password)

# Remote execution
Invoke-Command -ComputerName server01 -ScriptBlock {Get-Process}
Invoke-Command -ComputerName server01 -FilePath script.ps1
```

## 🗜️ Archive and Compression

```powershell
# Compress
Compress-Archive -Path C:\Source -DestinationPath archive.zip
Compress-Archive -Path C:\Files\* -DestinationPath files.zip -Update

# Extract
Expand-Archive -Path archive.zip -DestinationPath C:\Dest
Expand-Archive archive.zip -Force    # Overwrite
```

## 📅 Date and Time

```powershell
# Current date/time
Get-Date
Get-Date -Format "yyyy-MM-dd HH:mm:ss"

# Date calculations
(Get-Date).AddDays(-7)
(Get-Date).AddHours(5)

# Specific date
Get-Date "2024-01-01"
```

## 🔁 Loops and Conditions

```powershell
# If statement
if ($value -eq 5) {
    Write-Host "Value is 5"
} elseif ($value -gt 5) {
    Write-Host "Greater than 5"
} else {
    Write-Host "Less than 5"
}

# ForEach loop
foreach ($item in $array) {
    Write-Host $item
}

# For loop
for ($i = 0; $i -lt 10; $i++) {
    Write-Host $i
}

# While loop
while ($count -lt 10) {
    $count++
}
```

## 📋 Common Tasks

```powershell
# Find large files
Get-ChildItem C:\ -Recurse | Where-Object {$_.Length -gt 100MB} | Sort-Object Length -Descending

# Get running services
Get-Service | Where-Object {$_.Status -eq 'Running'} | Select-Object Name, DisplayName

# Free disk space
Get-PSDrive | Where-Object {$_.Free -ne $null} | Select-Object Name, @{L='FreeGB';E={[math]::Round($_.Free/1GB,2)}}

# List installed programs
Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* | Select-Object DisplayName, DisplayVersion

# Kill process by name
Get-Process chrome | Stop-Process -Force

# Check if port is listening
Test-NetConnection -ComputerName localhost -Port 80

# Get top CPU processes
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10 Name, CPU

# Clear temp files
Remove-Item $env:TEMP\* -Recurse -Force -ErrorAction SilentlyContinue
```

## 💡 Useful Aliases

```powershell
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

# Create custom alias
New-Alias -Name np -Value notepad
Set-Alias list Get-ChildItem
```

## 🎯 Operators

```powershell
# Comparison
-eq     # Equal
-ne     # Not equal
-gt     # Greater than
-ge     # Greater or equal
-lt     # Less than
-le     # Less or equal
-like   # Wildcard match
-match  # Regex match

# Logical
-and    # AND
-or     # OR
-not    # NOT
!       # NOT

# Examples
5 -eq 5                      # True
"test" -like "t*"            # True
"test@email.com" -match "^\w+@"  # True
```

## 🔍 Error Handling

```powershell
# Try/Catch
try {
    Get-Content C:\nonexistent.txt
} catch {
    Write-Host "Error: $_"
}

# Error action preference
$ErrorActionPreference = "Stop"        # Stop on error
$ErrorActionPreference = "Continue"    # Default
$ErrorActionPreference = "SilentlyContinue"

# Suppress errors
Get-ChildItem C:\Windows -Recurse -ErrorAction SilentlyContinue
```

## Related Topics

- [[Windows/PowerShell Basics|PowerShell Basics (Detailed)]]
- [[Windows/PowerShell Scripting|PowerShell Scripting]]
- [[Windows/Windows Basics|Windows Basics]]

---

#powershell #windows #cheatsheet #reference
