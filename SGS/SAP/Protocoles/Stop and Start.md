
## STOPSAP

```bash
sapcontrol -nr 47 -function StopWait 2700 1
sapcontrol -nr 47 -function StopService
StopWait 2700
cleanipc 47 remove

sapcontrol -nr 88 -function StopWait 2700 1
sapcontrol -nr 88 -function StopService
sapcontrol -nr 88 -function GetProcessList
cleanipc 88 remove
```


## STARTSAP


```bash
sapcontrol -nr 88 -function StartService P47
sapcontrol -nr 88 -function StartWait 2700 1
sapcontrol -nr 88 -function GetProcessList

sapcontrol -nr 47 -function StartService P47
sapcontrol -nr 47 -function StartWait 2700 1
sapcontrol -nr 48 -function GetProcessList
```
