# Assemblies

To build the DemoCreateProcess DLL, go to the `DemoCreateProcess` folder and run:

```
msbuild /p:Configuration=Release
```

In this folder, run:

```
..\..\donut.exe -f .\DemoCreateProcess.dll -c TestClass -m RunProcess -p "notepad.exe calc.exe"
```

To execute, run ProcessManager to find the PID of what you want to inject into:
```
..\ProcessManager.exe --name notepad
```

To inject, run:
```
 ..\DonutTest.exe 19644
```

Show .NET Assemblies in Process Hacker.

# JScript

The JScript we will run:
```javascript
var sh  
sh = new ActiveXObject("Wscript.Shell")  
sh.Run("calc.exe")  
WScript.Quit()
```

Generate shellcode for the JScript.
```
..\..\donut.exe -f .\calc.js
```

To execute, run ProcessManager to find the PID of what you want to inject into:
```
..\ProcessManager.exe --name notepad
```

To inject, run:
```
 ..\DonutTest.exe 19644
```

Show `jscript.dll` in Process Hacker.



# VBScript
The VBScript we will run:
```vbscript
Dim sh  
Set sh = CreateObject("Wscript.Shell")  
Call sh.Run("calc.exe")  
Set sh = Nothing  
WScript.Quit()
    
```

Generate shellcode for the VBScript:
```
..\..\donut.exe -f .\calc.vbs
```

To execute, run ProcessManager to find the PID of what you want to inject into:
```
..\ProcessManager.exe --name notepad
```

To inject, run:
```
 ..\DonutTest.exe 19644
```

Show `vbscript.dll` in Process Hacker.

# Unmanaged Executables (PEs)

The unmanaged DLL that we will run includes the following C code. It exports DLLMain, which creates a MessageBox on PROCESS_ATTACH.
```c
__declspec(dllexport)
BOOL WINAPI DllMain(HMODULE hModule,
                      DWORD ul_reason_for_call,
                      LPVOID lpReserved) {
  switch (ul_reason_for_call) {
    case DLL_PROCESS_ATTACH:
      MessageBox(NULL, L"Hello, World!", L"Hello, World!", 0);
      break;
    case DLL_THREAD_ATTACH:
    case DLL_THREAD_DETACH:
    case DLL_PROCESS_DETACH:
      break;
  }
  return TRUE;
}
```

Generate shellcode for the dll:
```
..\..\donut.exe -f .\hello.dll
```

To execute, run ProcessManager to find the PID of what you want to inject into:
```
..\ProcessManager.exe --name notepad
```

To inject, run:
```
 ..\DonutTest.exe 19644
```

There will be no additional loaded DLLs loaded into notepad.