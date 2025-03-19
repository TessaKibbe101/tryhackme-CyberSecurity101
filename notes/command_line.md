# Command Line Guide

## Windows Command Line

### Advantages of Using a CLI
- **Speed and Efficiency**: Faster than GUI-based interactions.
- **Lower Resource Usage**: Does not consume as much memory or CPU.
- **Automation**: Enables scripting with batch files.
- **Remote Management**: Can be used to administer systems remotely.

### Using `cmd.exe`
#### Basic System Information Commands
```cmd
set   # Check environment variables
ver   # Display Windows OS version
systeminfo  # List OS details, processor, and memory
more  # View long outputs page by page
help  # Provides help information for a command
cls   # Clear screen
```

#### Network Troubleshooting Commands
```cmd
ipconfig      # Show network configuration
ipconfig /all # Detailed network configuration
ping example.com  # Test network connectivity
tracert example.com  # Trace route to target server
```

#### More Networking Commands
```cmd
nslookup example.com   # Lookup domain's IP address
netstat -a  # Show active network connections
netstat -b  # Show programs associated with each connection
netstat -o  # Show process ID (PID) with connection
```

#### File and Disk Management Commands
```cmd
cd \target_directory   # Change directory
dir /s  # Display directory structure
tree    # Display hierarchical directory structure
mkdir new_folder  # Create directory
rmdir folder_name  # Delete directory
type file.txt  # Display file contents
copy file1.txt file2.txt  # Copy files
move file1.txt C:\NewFolder  # Move files
del file.txt  # Delete file
```

#### Task and Process Management
```cmd
tasklist  # List running processes
taskkill /PID 4567  # Terminate process by PID
```

#### System Maintenance Commands
```cmd
chkdsk   # Check disk for errors
driverquery  # List installed drivers
sfc /scannow  # Scan and repair system files
```

---

## Windows PowerShell

### What Is PowerShell?
PowerShell is an object-oriented command-line shell and scripting language.

### PowerShell Basics
```powershell
Get-Command  # List available cmdlets
Get-Help Get-Process  # Get help on cmdlets
Get-Alias  # Show command shortcuts
```

### Navigating the File System
```powershell
Get-ChildItem  # List files and directories
Set-Location -Path "C:\Users"  # Change directory
New-Item -Path "NewFolder" -ItemType "Directory"  # Create folder
Remove-Item -Path "OldFolder" -Recurse  # Delete folder
Copy-Item file1.txt file2.txt  # Copy file
Move-Item file1.txt C:\NewFolder  # Move file
Get-Content file.txt  # Display file contents
```

### Filtering and Sorting Data
```powershell
Get-ChildItem | Sort-Object Length  # Sort files by size
Get-ChildItem | Where-Object Extension -eq ".txt"  # Filter by file type
```

### Network and System Information
```powershell
Get-ComputerInfo  # Retrieve system details
Get-NetIPConfiguration  # Show IP configuration
Get-Process  # List running processes
Get-Service  # Show services and their status
Get-NetTCPConnection  # Display active network connections
Get-FileHash file.txt  # Generate file hash
```

### PowerShell Scripting
```powershell
# Example Script
$Name = Read-Host "Enter your name"
Write-Host "Welcome, $Name!"
```

#### Remote Command Execution
```powershell
Invoke-Command -ComputerName RemotePC -ScriptBlock { Get-Process }
```

---

## Linux Shell

### Basic Linux Commands
```bash
pwd  # Show current directory
cd /path/to/directory  # Change directory
ls  # List files and directories
cat file.txt  # Display file contents
grep 'pattern' file.txt  # Search text in file
```

### Checking Shell Type
```bash
echo $SHELL  # Show current shell
cat /etc/shells  # List available shells
chsh -s /bin/zsh  # Change default shell to Zsh
```

### Shell Scripting
```bash
#!/bin/bash
echo "Enter your name:"
read name
echo "Welcome, $name"
```

### Making Scripts Executable
```bash
chmod +x script.sh  # Give execution permissions
./script.sh  # Run the script
```

### Loops in Shell Scripts
```bash
for i in {1..10}
do
echo $i
done
```

### Conditional Statements
```bash
echo "Enter your name:"
read name
if [ "$name" = "Stewart" ]; then
    echo "Welcome, Stewart!"
else
    echo "Sorry!"
fi
```

### Adding Comments
```bash
# This is a comment in shell script
```

---

## Conclusion
This guide provides an overview of Windows `cmd.exe`, PowerShell, and Linux shells, covering commands for system administration, networking, file management, and scripting. Mastering the command line is crucial for automation, troubleshooting, and system administration.

