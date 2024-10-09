# Task Scheduling

Task scheduling is a feature in Linux systems that allows users to schedule and automate tasks. It allows administrators and users to run tasks at a specific time or within specific frequencies without having to start them manually. It can be used in Linux systems such as Ubuntu, Redhat Linux, and Solaris to manage a variety of tasks. Examples include automatically updating software, running scripts, cleaning databases, and automating backups. This also allows users to schedule regular and repetitive tasks to ensure they are regularly. In addition, alerts can be set up to display when certain events occur or to contact administrators or users. There are many different use cases for automation of this type, but these cover most cases.

&nbsp;

# Systemd

Systemd is a service used in Linux systems such as Ubuntu, Redhat Linux, and Solaris to start processes and scripts at a specific time. With it, we can set up processes and scripts to run at a specific time or time interval and can also specify specific events and triggers that will trigger a specific task. To do this, we need to take some steps and precautions before our scripts or processes are automatically executed by the system.

- Create a timer
- Create a service
- Activate the timer

&nbsp;

## Create a Timer

To create a timer for systemd, we need to create a directory where the timer script will be stored.

```Shell
bobinsson@BoB$ sudo mkdir /etc/systemd/system/mytimer.timer.d
bobinsson@BoB$ sudo vim /etc/systemd/system/mytimer.timer
```

Next, we need to create a script that configures the timer. The script must contain the following options: "<span style="color: #2dc26b;">Unit</span>", "<span style="color: #2dc26b;">Timer</span>" and "<span style="color: #2dc26b;">Install</span>". The "<span style="color: #2dc26b;">Unit</span>" option specifies a description for the timer. The "<span style="color: #2dc26b;">Timer</span>" option specifies when to start the timer and when to activate it. Finally, the "<span style="color: #2dc26b;">Install</span>" option specifies where to install the timer.

### mytimer.timer

```txt
[Unit]
Description=My Timer

[Timer]
OnBootSec=3min
OnUnitActiveSec=1hour

[Install]
WantedBy=timers.target
```

Here it depends on how we want to use our script. For example, if we want to run our script only once after the system boot, we should use <span style="color: #2dc26b;">OnBootSec</span> setting in <span style="color: #2dc26b;">Timer</span>. However, if we want our script to run regularly, then we should use the <span style="color: #2dc26b;">OnUnitActiveSec</span> to have the system run the script at regular intervals. Next, we need to create our <span style="color: #2dc26b;">service</span>.

&nbsp;

## Create a Service

```Shell
bobinsson@BoB$ sudo vim /etc/systemd/system/mytimer.service
```

Here we set a description and specify the full path to the script we want to run. The "multi-user.target" is the unit system that is activated when starting a normal multi-user mode. It defines the services that should be started on a normal system startup.

### mytimer.service

```txt
[Unit]
Description=My Service

[Service]
ExecStart=/full/path/to/my/script.sh

[Install]
WantedBy=multi-user.target
```

After that, we have to let <span style="color: #2dc26b;">systemd</span> read the folder again to include the changes.

&nbsp;

Verify that the files created contain no errors

```Shell
bobinsson@BoB$ systemd-analyze verify /etc/systemd/system/mytimer.*
```

&nbsp;

## Reload Systemd

```Shell
bobinsson@BoB$ sudo systemctl daemon-reload
```

After that, we can use <span style="color: #2dc26b;">systemctl</span> to start the service manually and <span style="color: #2dc26b;">enable</span> the auto-start.

&nbsp;

## Start the Timer & Service

```Shell
bobinsson@BoB$ sudo systemctl enable mytimer.timer
bobinsson@BoB$ sudo systemctl start mytimer.timer
```

&nbsp;

&nbsp;

&nbsp;

&nbsp;

# Cron

Cron is another tool that can be used in Linux to schedule and automate processes. It allows users and administrators to execute tasks at a specific time or within specific intervals. For the above examples, we can also use Cron to automate the same tasks. We just need to create a script and then tell the cron daemon to call it at a specific time.

With Cron, we can automate the same tasks, but the process for setting up the Cron daemon is a little different than Systemd. To set up the cron daemon, we need to store the tasks in a file called <span style="color: #2dc26b;">crontab</span> and then tell the daemon when to run the tasks. Then we can schedule and automate the tasks by configuring the cron daemon accordingly. The structure of Cron consists of the following components:

| Time Frame | Description |
| --- | --- |
| Minutes (0-59) | This specifies in which minute the task should be executed |
| Hours (0-23) | This specifies in which hour the task should be executed |
| Days of month (1-31) | This specifies on which day of the month the task should be executed |
| Months (1-12) | This specifies in which month the task should be executed |
| Days of week (0-7) | This specifies on which day of the week the task should be executed |

For example, such crontab would look like this:

```txt
# System Update
* */6 * * /path/to/update_software.sh

# Execute scripts
0 0 1 * * /path/to/scripts/run_scripts.sh

# Cleanup DB
0 0 * * 0 /path/to/scripts/clean_database.sh

# Backups
0 0 * * 7 /path/to/scripts/backup.sh
```

The "<span style="color: #2dc26b;">System Update</span>" should be executed every sixth hour. This is indicated by the entry <span style="color: #2dc26b;">\*/6</span> in the hour column. The task is executed by the script <span style="color: #2dc26b;">update_software.sh</span>, whose path is given in the last column.

The task "<span style="color: #2dc26b;">Execute scripts</span>" is to be executed every first day of the month at midnight. This is indicated by the entries <span style="color: #2dc26b;">0</span> and <span style="color: #2dc26b;">0</span> in the minute and hour columns, and the <span style="color: #2dc26b;">1</span> in the days-of-the-month column. The task is executed by the <span style="color: #2dc26b;">run_scripts.sh</span> script, whose path is given in the last column.

The third task, "<span style="color: #2dc26b;">Cleanup DB</span>", is to be executed every Sunday at midnight. This is indicated by the entries <span style="color: #2dc26b;">0</span> and <span style="color: #2dc26b;">0</span> in the minute and hour columns and <span style="color: #2dc26b;">0</span> in the days-of-the-week column. The task is executed by the <span style="color: #2dc26b;">clean_database.sh</span> script, whose path is given in the last column.

The fourth task, "<span style="color: #2dc26b;">backups</span>", is to be executed every Sunday at midnight. This is indicated by the entries <span style="color: #2dc26b;">0</span> and <span style="color: #2dc26b;">0</span> in the minute and hour columns and <span style="color: #2dc26b;">7</span> in the days-of-the-week column. The task is executed by the <span style="color: #2dc26b;">backup.sh</span> script, whose path is given in the last column.

It is also possible to receive notifications when a task is executed successfully or unsuccessfully. In addition, we can create logs to monitor the execution of the tasks.

&nbsp;

&nbsp;

# Systemd vs Cron

Systemd and Cron are both tools that can be used in Linux systems to schedule and automate processes. The key difference between these two tools is how they are configured. With Systemd, you need to create a timer and service scripts that tells the operating system when to run the tasks. On the other hands, with Cron you need to create a <span style="color: #2dc26b;">crontab</span> file that tells the cron daemon when to run the tasks.

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

# Questions

- What is the Type of the service of the "dconf.service"?

dbus