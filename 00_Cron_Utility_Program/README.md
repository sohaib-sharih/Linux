### 10.11 Cron

1. **cron** is a time-based scheduling utility program.
2. It can schedule routine jobs on specific days or specified dates. You can also schedule routine repetitive jobs.
3. **cron** is driven by a configuration file called **/etc/crontab** (cron table), which contains the various shell commands that need to be run at the properly scheduled times.
4. Each line of a **crontab** file represents a job, and is composed of a so-called **CRON** expression, followed by a ***shell*** command to execute.
5. `crontab -e` would open up the cron editor.
6. You *edit* existing jobs or *create* new ones.
7. ***Pattern/ Format:*** Each line of the **crontab** file will contain 6 fields

| Field | Description      | Values                     |
| ----- | ---------------- | -------------------------- |
| MIN   | Minutes          | 0 to 59                    |
| HOUR  | hours            | 0 to 23                    |
| DOM   | Day of the Month | 1-31                       |
| MON   | Month            | 1-12                       |
| DOW   | Day of Week      | 0-6 (0 = Sunday)           |
| CMD   | Command          | Any command to be executed |
EXAMPLES

1. The entry `* * * * * /usr/local/bin/execute/this/script.sh` will schedule a job to execute **script.sh** every minute of every hour of every day of the month, and every month and every day in the week.
2. The entry **30 08 10 06 * /home/sysadmin/full-backup** will schedule a full-backup at 8.30 a.m., 10-June, irrespective of the day of the week.
3. Example
```
string         meaning
              ------         -------
              @reboot        Run once, at startup.
              @yearly        Run once a year, "0 0 1 1 *".
              @annually      (same as @yearly)
              @monthly       Run once a month, "0 0 1 * *".
              @weekly        Run once a week, "0 0 * * 0".
              @daily         Run once a day, "0 0 * * *".
              @midnight      (same as @daily)
              @hourly        Run once an hour, "0 * * * *".
              
```


### 10.12 anacron

1. **Cron** has been used in UNIX based computers for decades.
2. Modern users have moved to a newer ***utility program*** called the ***anacron.***
3. ***Cron*** always assumed that the machine was always running. *If the machine was off, that would mean cron jobs wouldn't run.*
4. ***Anacron*** will run scheduled jobs in a staggered manner and controlled manner when the system is up and running.

![[anacron.PNG]]


### 10.13 Sleep

1. Sleep command is used to put a *command* that is keeping another system busy which is needed for another task.
2. You can use the following format to run the sleep command:
```
sleep NUMBER[SUFFIX]...

```

| Suffixes | Descriptions |
| -------- | ------------ |
| s        | Seconds      |
| m        | Minutes      |
| h        | Hour         |
| d        | Days         |


3. **Sleep** & **At** Utility programs are different from each other, *sleep allows you to make a process or job to temporary pause, while **at** reschedules the task for another specified day and time.*
4. **Example Situation for usage:** *Sometimes, a command or job must be delayed or suspended. Suppose, for example, an application has read and processed the contents of a data file and then needs to save a report on a backup system. If the backup system is currently busy or not available, the application can be made to sleep (wait) until it can complete its work. Such a delay might be to mount the backup device and prepare it for writing.  An even simpler and frequent case is one where a system process needs to run periodically to take care of any work that has been queued up for it to deal with and then has to lurk in the background until it is needed again.*

## Practice Task

#### Scheduling a periodic task using the *cron utility program:*

1. There are 2 ways of creating a Crontab:
	a. Use the existing crontab through `crontab -e` command.
	b. Create a **new crontab** using the crontab command => `crontab exampletab`

2. **The format of a crontab:**

```
The format of a crontab file is:

MIN HOUR DOM MON DOW COMMAND

```

3. **Format of a crontab file:**
	a. There is no file extension for a crontab file, you just create it using ***nano example***
	b. The *example* is the name of the crontab file.
	c. Then you need to ***load*** the crontab file you created.
	d. The crontab file must include the *schedule + command* of the operation to be run.
	e. **Command:** The command indicates the file location ***path***, that needs to be run.
4. Steps to creating a Periodic tab with ***Cron***
	a. Create a crontab file using the *nano command.*
	b. Edit the file and add the Task and Schedule it.
	c. Create a file of .sh format, and edit it with the Operation that needs to take place.
	d. Make the .sh file an executable file by using the **chmod +x** command *followed by the name of the file.*
	e. **Load a crontab file:** Use the ***crontab crontabFileName*** to add the new file to the crontab(load the file to the crontab).
	f. Check the crontab using the ***crontab -l*** command.
	g. If you want to delete the cron jobs, use the ***crontab -r*** command.
5. Important Key Points to remember:
	a. **Crontab:** You can use the existing crontab file or reload a new one. Whenever you load a new crontab file, you basically replace the existing one. When you using the ***crontab -r*** command, it deletes the new one added/loaded and then loads the ***default file*** which looks like the one below:

```
crontab -e

OUTPUT:

# Edit this file to introduce tasks to be run by cron.
#
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
#
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').
#
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
#  ----> Remove the hash and replace it with your own cronjob here or simply add it in a new line.
``` 

### TASK # 1

Set up a cron job to do some simple task every day at 10 a.m.

1. Create a job script
```
nano /tmp/cronjob.sh

Edit the following content in the file:

#!/bin/bash
echo "Hello I am running $0 at $(date)" >> /tmp/myjob_output.txt

```
2. Make the .sh file executable:
```
chmod +x /tmp/cronjob.sh
```

3. Edit your crontab by the following command => `crontab -e`
4. Add the following line at the bottom, or as the last line after all the ashes:

```
0 10 * * * /tmp/myjob.sh
```
5. **VERIFY:**
```
crontab -l

OUTPUT:

# Edit this file to introduce tasks to be run by cron.
#
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
#
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').
#
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
#
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
#
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
#
# For more information see the manual pages of crontab(5) and cron(8)
#
# m h  dom mon dow   command
0 10 * * * /tmp/cronjob.sh
```

6. **Check the output after 10 AM:** cat /tmp/myjob_output.txt
7. **Delete the cronjob:** Use the following command => `crontab -r`
8. If the machine is not up at 10 AM on a given day, **anacron** will run the job at a suitable time.

### TASK # 2

1. Create a crontab

```
Nano newTab

Add the Following text:

5 * * * * * /tmp/newCron.sh
```

2. Load the new crontab

```
crontab newTab
```

3. Edit the .sh file

```
1. If you are using Nano Command Utility Program then saving it would require you to use CTRL X, then pressing Y key to confirm and then Enter.
2. If you are using VI to edit, then you have to run the command VI /tmp/newCron.sh -> press i for edit, then make the changes -> Press ESC -> Type wq: + Enter.
3. Following is the text to be added in it.

echo This is my new Cronjob > newCron_Output.txt

OR

#!/bin/bash
echo This is my new Cronjob > newCron_Output.txt

NOTE: Save this file. It can work in both cases.
```

4. Make the newCron.sh file executable by the following command:
```
chmod +x newCron.sh
```

5. Check the crontab using this command -> `crontab -l`
6. Wait for 5 minutes, then check the output file if it was created. If you followed the steps correctly, it should have created an output file by the name of ***newCron_Output.txt*** file with the echo string.
7. To delete the loaded crontab, simply run this command -> `crontab -r`