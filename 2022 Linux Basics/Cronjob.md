# Cronjob

- Helps user schedule jobs in Linux. 
- Specify any date, time, or frequency to schedule a task.
- facilitated by `cron service` which runs in the background.

<br>

## **Crontab commands**

- `crontab -e`: edit cronjob task schedule file. 
- `crontab -r`: cronjob for root
- `crontab -l`: list cronjobs.

        11 11 * * 3 /usr/local/bin/forest-reporter.sh
        23 11 * * 2 /usr/local/bin/forest-inspector.sh
        23 23 * * 2 /usr/local/bin/forest-debugger.sh
        11 23 * * * /usr/local/bin/forest-tester.sh
        11 23 * 2 * /usr/local/bin/forest-troubleshooter.sh
        11 23 * * 2 /usr/local/bin/tree-identifier.sh


<br>

## Examples

|    | Minute   |  Hour  |  Day  |  Month  | Weekday   |
| -- | -- | -- | -- | -- | -- |
|  at 10th minute  |  10  |    |    |    |    |
|  16th hour<br>4 PM  |    |  16  |    |    |    |
|  19th day<br>of the month  |    |    |  19  |    |    |
| February   |    |    |    |  2  |    |
| Any day<br>of the week   |    |    |    |    |  *  |
| `*/` == Every<br>every 30 minutes   |  */30 <br>00,30<br>00/30 |    |    |    |    |
| `*` == All<br>All hours<br> (== every hour)   |   |  *  |    |    |    |

- `0 21 * * * uptime >> /tmp/system-report.txt`: run `uptime` and redirect the reulst to the specified file.
- `sudo tail /var/log/syslog`: check the system log to verify job log. 

        Jan 23 06:40:49 forest-walker-10 systemd[2744]: Listening on GnuPG cryptographic agent and passphrase cache (restricted).
        Jan 23 06:40:49 forest-walker-10 systemd[2744]: Listening on GnuPG cryptographic agent and passphrase cache (access for web browsers).
        Jan 23 06:40:49 forest-walker-10 systemd[2744]: Listening on GnuPG network certificate management daemon.
        Jan 23 06:40:49 forest-walker-10 systemd[2744]: Listening on GnuPG cryptographic agent and passphrase cache.
        Jan 23 06:40:49 forest-walker-10 systemd[2744]: Reached target Sockets.
        Jan 23 06:40:49 forest-walker-10 systemd[2744]: Reached target Paths.
        Jan 23 06:40:49 forest-walker-10 systemd[2744]: Reached target Basic System.
        Jan 23 06:40:49 forest-walker-10 systemd[2744]: Reached target Default.
        Jan 23 06:40:49 forest-walker-10 systemd[2744]: Startup finished in 82ms.
        Jan 23 06:41:01 forest-walker-10 cron[2671]: (bob) INSECURE MODE (mode 0600 expected) (crontabs/forest)


