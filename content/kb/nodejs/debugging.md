start a process in inspect mode and connect to it
use breakpoints

```bash
node --inspect app.js
# can be useful if debugger statement is already in code
```

```bash
# inspect with active breakpoint - preferred,-app not fully initialized, no async task
node --inspect-brk app.js
```

then chrome://inspect from browser (Chrome DevTools)
goto remote target
click on inspect link

also,
putting "debugger;" statement into code creates breakpoint in code

see Debuggin Guide from nodejs.org