## 1.0 Python automation to detect data usage

1. The python code will generate the data usage on a daily basis and save it in an excel sheet.
2. You need to install the following packages to run the script on Windows as well as Ubuntu, depending on which OS you want to run the script.

	a. Windows: *pip install psutil openpyxl*
	b. Ubuntu: `sudo apt python3-pip -y` | `pip install openpyxl psutil`
	c. **Check the installed version of pip:** `pip3 --version`
3. You have to install Python, Pip, and Pip Modules required to run the python file. Because even though the python file resides inside Windows OS filesystem, but it uses the python installed in Ubuntu to execute and generate the xlsx file in the /tmp folder.
4. **Prerequisites:** You need to have Ubuntu installed in your system. Python, and the modules used to run the python script, even if you have them installed on windows, you will have to reinstall them on Ubuntu again because you will be running the python script through Ubuntu.

### 1.1 Steps for scheduling a Cron Job

1. Schedule a task on *Crontab -e* or create a new crontab file. If you are using the default crontab file, then just run the following command -> ***crontab -e*** and add the following schedule, which entails that the shell script is to run every 5 minutes.

```
5 * * * * /tmp/data.sh -> This command would run the data.sh shell script

5 * * * * /tmp/data.sh >> /tmp/data.log 2>&1 -> This command on the crontab will run the script and output any errors found on a data.log file which will be saved in the /tmp/data.log folder

NOTE:
You can test the errors if you are not sure if your shell script is going to run or not. Otherwise consider using the first command.
```
2. Create a ***Shell script*** + ***make it executable***
```
touch /tmp/data.sh
vi /tmp/data.sh
press i to edit

Add the following commands:
#!/bin/bash 
/usr/bin/python3 "/mnt/e/MISCELLENEOUS/MY PROJECT/LEarning/Blockchain/Node js/My Assignments/Practice/Linux/data_utility/data.py"

NOTE:
1. You don't need to use the following in the first line: #!/bin/bash
--------------------------------------------------
chmod +x /tmp/data.sh
ls -l /tmp/data.sh

OUTPUT:
-rwxr-xr-x 1 sohaibsharih sohaibsharih 142 May 31 18:01 data.sh
```

2. Check the crontab -> `crontab -l` or `crontab -e`
3. If you want to load your own cron file in the crontab, then do the following:
	a. Create a file without an extension -> `nano test`
	b. **Load it** *on the crontab* by the following command -> `crontab test`
	c. And you are done, follow the remaining steps as they are.
	d. To delete the crontab loaded -> `crontab -r` *This will reload the default crontab file.*
4. **When do we use #!/bin/bash?**
	a. When you're scheduling a task through cron, it will select the appropriate *shell* and *invoke it by itself, for every command in the Shell Script.*
	b. You need to add this line, when you are running the Shell Script ***directly***, so that the Shell knows which appropriate shell to use to run the ***command.***
5. **Test the script manually**: `./data.sh` or data.sh if you are in the working directory.

```
so########$$h@DESKTOP-Q^^%RIR:/tmp$ /tmp/data.sh
2025-05-31: 71.51 MB logged.

```

## 1.2 Two approaches to run your commands:

1. Method 1: *Running the commands directly from the crontab file*
2. Method 2: *Running the script through the shell script*

Yes—you can put any valid shell command (or Python invocation) right into a crontab line. For example, instead of:

cron
```
5 * * * * /tmp/data.sh

```

You should write
cron
```
5 * * * * /usr/bin/python3 /mnt/e/…/data.py >> /tmp/data.log 2>&1

```

Which method is “correct”?

1. **Direct command in crontab** is fine if it’s just one simple call (e.g. invoking Python with full paths).
2. **Using a separate shell script** is cleaner if you need to:
	a. run multiple steps in sequence
	b. set environment variables
	c. add comments or error‐handling logic
	d. keep your cron entries tidy
### 1.3 Additional Points

1. **Why `../mnt/...` fails in a cron job?**
	a. When you run a script in the terminal, you're in your current directory (e.g., `/tmp`), so `../mnt/...` works.
	b. But **cron runs from the root directory (`/`)**, **not from `/tmp`**, so `../mnt/...` becomes `/mnt/...` incorrectly → and fails.
	c. So relative paths like `../mnt/...` **do not resolve correctly in cron**.
2. **Enclosing the absolute path in Quotation marks:**
3. If your Python script reads or writes files (like Excel), use **absolute paths**. ***Cron jobs*** run from root (`/`) by default, not your home directory.
4. **Test the Shell Script Manually:** Use this command -> `/tmp/cronjob.sh`
5. **Error of File Path/Absolute Path:** *Cron interprets the path up until the **space** and then it breaks. In order to **fix this,** Wrap the full path to your Python file in **quotes** inside your `/tmp/data.sh` script.*

```
#!/bin/bash
/usr/bin/python3 "/mnt/e/MISCELLENEOUS/MY FILES/data_usage.py"
```
6. Common mistakes related to Absolute Paths:

```
#!/bin/bash
/usr/bin/python3 "../mnt/e/MISCELLENEOUS/MY PROJECT/LEarning/Blockchain/Node js/My Assignments/Practice/Linux/data_utility/data.py"

```

The issue here is you're using a **relative path**, but cron runs from `/`, so it won't resolve `"../mnt/...` correctly.

7. Why `../mnt/...` fails in a cron job?

a. When you run a script in the terminal, you're in your current directory (e.g., `/tmp`), so `../mnt/...` works.
b. But **cron runs from the root directory (`/`)**, **not from `/tmp`**, so `../mnt/...` becomes `/mnt/...` incorrectly → and fails.
c. So relative paths like `../mnt/...` **do not resolve correctly in cron**.
d. Use the **full absolute path**, always:

```
#!/bin/bash
/usr/bin/python3 "/mnt/e/MISCELLENEOUS/MY PROJECT/LEarning/Blockchain/Node js/My Assignments/Practice/Linux/data_utility/data.py"

```

8. **Steps to open a document through Windows**
	a. **Explorer:** `explorer.exe` is the **Windows File Explorer**. *When you run it with a file path, it **opens that file or folder in Windows** using the default associated app (like Excel for `.xlsx`).*
	b. **Find the path to Explorer.exe**:
```
ls /mnt/c/windows

OUTPUT:

 DtcInstall.log                     ServiceProfiles         explorer.exe --> THIS
 ELAMBKUP                           ServiceState            hh.exe
 Fonts                              Setup                   mib.bin
 GameBarPresenceWriter              ShellComponents         notepad.exe
 Globalization                      ShellExperiences        regedit.exe
 Help                               SoftwareDistribution    rescache
 HelpPane.exe                       Speech                  rtl8723b_mp_chip_bt40_fw_asic_rom_patch_new.dll
```
c. **Absolute path of the Explorer.exe File:** `/mnt/c/windows/explorer.exe`
d. **Run the following Command to open the Excel Document:** `/mnt/c/Windows/explorer.exe "$(wslpath -w /home/yourusername/filename.xlsx)"`
e. **wslpath -w:** This command converts the *absolute **Linux** path* to a **Windows Path.**
```
wslpath -w /mnt/e/miscellenous
OUTPUT:
E:\miscellenous
```
f. [[Environment Variables in Ubuntu]]


### 1.4 Screenshots of an example

![[crontab1.PNG]]

![[datalog2.PNG]]
![[tmpDatalog3.PNG]]
![[catDatalog4.PNG]]
![[excel5.PNG]]
