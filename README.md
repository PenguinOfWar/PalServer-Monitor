# PalServer-Monitor

A tiny bash script that checks if Palworld PalServer is up and reports on CPU and memory usage. If PalServer is down, the script starts it again.

Crash logs are then dropped to an S3 bucket for a web interface to view. This is optional, if you don't need it just remove lines 12-13.

## Why

I'm running a Palworld server for myself and some friends and wanted to do some basic monitoring and intelligence around the crashes we've been encountering. This is then shared to a web interface so the others can see when the last crash was. I also didn't want to have to manually restart the server each time it died (for obvious reasons).

What I've observed so far is that the PalServer software will just eat memory until it dies. This isn't super surprising as the software is in alpha/early access state, however we are having great success running it on a NetCup VPS with 8 cores and 12GB of memory (with [15GB swap enabled](https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-22-04)).

I'm sharing this in case it's useful for any other enthusiasts who want to run their own server and keep track of what is going on.

## Logs

Live logs are stored in `/home/steam/log/palserver.csv`. After a crash, the current log is moved to `/home/steam/log/crashes/palserver-$TIMESTAMP.csv`.

```bash
root@machine:~# cat /home/steam/log/crashes/palserver-1706431621522.csv 
2024-01-28 09:46:01,running,55.6,15.4,5211236,1897096
2024-01-28 09:47:01,restarting,0,0,0,0
```
