Run SafetyKatz through loader in Debug mode. To show how everything works. 

Use the version of donut.exe and loader.exe that are built in debug mode. MAKE SURE TO USE THE 64-BIT VERSION OF VISUAL STUDIOS DEVELOPER TOOLS. Built files are included in the local dir.

Generate an instance file and shellcode with donut:
```
.\donut.exe -f SafetyKatz.exe
```

Make a note of all of the debug output. Walk through it.

Open up an admin prompt (if not already) and run the instance file through the loader.
```
.\loader.exe .\instance
```