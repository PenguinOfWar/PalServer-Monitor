# PalServer-Monitor

A tiny bash script that checks if PalServer is up and reports on CPU and memory usage. If PalServer is down, the script starts it again.

## Logs

Live logs are stored in `/home/steam/log/palserver.csv`. After a crash, the current log is moved to `/home/steam/log/crashes/palserver-$TIMESTAMP.csv`.

```bash
root@machine:~# cat /home/steam/log/crashes/palserver-1706431621522.csv 
2024-01-28 09:46:01,running,55.6,15.4,5211236,1897096
2024-01-28 09:47:01,restarting,0,0,0,0
```
