## Cron

One day my website's SSL certificates failed to renew. It was then that I realised that cron had not been running for a long time. To avoid this happening in the future, I needed a tool which would check that cron is working after each change to the crontab.

First create the directory ```sudo mkdir /var/log/croncheck```

Then add the following lines to ```/etc/crontab```:

```
* * * * * root echo $(date +"\%Y-\%m-\%d \%T"): cron executed command at /etc/crontab >> /var/log/croncheck/croncheck.log
0 0 * * * root mv /var/log/croncheck/croncheck.log /var/log/croncheck/croncheck$(date +\%u).log
```

The first line writes a line of text to a file every minute. The second line rotates these files daily so they don't get too big, and overwrites them after a week.

To use it, make your changes to ```etc/crontab```, then read the file ```/var/log/croncheck/croncheck.log``` to see when cron last ran.