# PalServer-Monitor

A handful of tiny bash scripts that checks if Palworld PalServer is up, restarts PalServer if necessary, reports on CPU and memory usage, and backs up save and world data.

Crash logs are then dropped to an S3 bucket for a web interface ([Palserver-Monitor-Log-Viewer](https://github.com/PenguinOfWar/palserver-monitor-log-viewer)) to view and a Vercel build is triggered. This is optional, if you don't need it just remove lines 12-14.

## Why

I'm running a Palworld server for myself and some friends and wanted to do some basic monitoring and intelligence around the crashes we've been encountering. This is then shared to a web interface so the others can see when the last crash was. I also didn't want to have to manually restart the server each time it died (for obvious reasons).

What I've observed so far is that the PalServer software will just eat memory until it dies. This isn't super surprising as the software is in alpha/early access state, however we are having great success running it on a NetCup VPS with 8 cores and 12GB of memory (with [15GB swap enabled](https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-22-04)).

I'm sharing this in case it's useful for any other enthusiasts who want to run their own server and keep track of what is going on.

## Scripts

`Monitor`: Live logs are stored in `/home/steam/log/palserver.csv`. After a crash, the current log is copied to `/home/steam/log/crashes/palserver-$TIMESTAMP.csv`, uploaded to the designated S3 bucket, and then cleared.

```bash
root@machine:~# cat /home/steam/log/crashes/palserver-1706431621522.csv 
2024-01-28 09:46:01,running,55.6,15.4,5211236,1897096
2024-01-28 09:47:01,restarting,0,0,0,0
```

`Performance`: Logs within `/home/steam/log/palserver.csv` are trimmed on an hourly basis to the last 1440 records (roughly the last 24 hours at one line per minute) then uploaded to the designated S3 bucket so you can view the last 24 hours of data in a time series chart.

`Backup:` Backs up the Palworld save and world files from `/home/steam/Palworld/Pal/Saved/SaveGames` to the designated S3 bucket.
