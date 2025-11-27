# Linux - 43 modules

Linux is an operating system that is similar to Unix. It is free and open source, and users can expand it.
A Linux distribution combines the Linux kernel with other software applications to provide a complete operating system environment.
All Linux distributions come with a CLI. Some also offer a GUI.
The bashshell is the default shell in Linux.
You can use the man command to read the Linux manual pages.

## Lab 1 

This is just an introduction to Linux CLI using Putty to connect to an EC2 

### Connecting to Putty ussing the SSH
![EC2](../images/Linux/Linux_putty_connecting_to_ec2.PNG)

### Running Commands
![EC2](../images/Linux/running_simple_commands.PNG)

### Creating a script to make workflow easier
![EC2](../images/Linux/Simplifying_workload.PNG)

## Lab 2 - Managing Users and Groups

### Create Users
In this section, you create users based on the following table:

First Name	Last Name	User ID	Job Role	Starting Password
Alejandro	Rosalez	arosalez	Sales Manager	P@ssword1234!
Efua	Owusu	eowusu	Shipping	P@ssword1234!
Jane	Doe	jdoe	Shipping	P@ssword1234!
Li	Juan	ljuan	HR Manager	P@ssword1234!
Mary	Major	mmajor	Finance Manager	P@ssword1234!
Mateo	Jackson	mjackson	CEO	P@ssword1234!
Nikki	Wolf	nwolf	Sales Representative	P@ssword1234!
Paulo	Santos	psantos	Shipping	P@ssword1234!
Sofia	Martinez	smartinez	HR Specialist	P@ssword1234!
Saanvi	Sarkar	ssarkar	Finance Specialist	P@ssword1234!
Ensure that you are spelling the user IDs correctly so that these users can use default credentials to log in.

### Create Groups
In this section you create groups of users and add users to the groups.

Sales
HR
Finance
Personnel
CEO
Shipping
Managers

### Assign Users to the groups created 

To check the group memberships, enter sudo cat /etc/group into the terminal and press Enter.

Sales:x:1014:arosalez,nwolf,ec2-user
HR:x:1015:ljuan,smartinez,ec2-user
Finance:x:1016:mmajor,ssarkar,ec2-user
Shipping:x:1017:eowusu,jdoe,psantos,ec2-user
Managers:x:1018:arosalez,ljuan,mmajor,ec2-user
CEO:x:1019:mjackson,ec2-user

## Lab 3
### Creating a File
In this task, you create a specific folder structure. A picture of the files and folders is provided, and your task is to recreate the structure in the new machine.

Using the terminal, you recreate the following structure on the Linux machine.

/home/ec2-user/CompanyA/
/home/ec2-user/CompanyA/Finance/
/home/ec2-user/CompanyA/Finance/ProfitAndLossStatements.csv
/home/ec2-user/CompanyA/Finance/Salary.csv
/home/ec2-user/CompanyA/HR/
/home/ec2-user/CompanyA/HR/Assessments.csvv
/home/ec2-user/CompanyA/HR/TrialPeriod.csv
/home/ec2-user/CompanyA/Management/
/home/ec2-user/CompanyA/Management/Managers.csv
/home/ec2-user/CompanyA/Management/Schedule.csv

![EC2](../images/Linux/Filingstructure.PNG)

For this next task, you:

Copy the Finance folder and its content to the HR folder, and remove the previous Finance folder 
Move the Management folder inside the HR folder
Create an Employees folder inside the HR folder, and move the Assessments.csv and TrialPeriod.csv file inside the Employees folder
To ensure that you are in the appropriate CompanyA folder, enter pwd into the terminal and press Enter.

### Deleting and reorganizing

[ec2-user@ CompanyA]$ pwd
/home/ec2-user/CompanyA
To copy the Finance folder and its content, enter cp -r Finance HR and press Enter.

To verify that the folder and the content was copied, enter ls HR/Finance and press Enter.

[ec2-user@ CompanyA]$ ls HR/Finance
ProfitAndLossStatements.csv Salary.csv
To remove the Finance folder from the CompanyA folder structure, enter rmdir Finance and press Enter. 

[ec2-user@ companyA]$ rmdir Finance
rmdir: failed to remove ‘Finance/’: Directory not empty
Note:
rmdir works only on an empty directory.
To remove the folder, you have two options:

Remove the files inside the folder and then remove the Finance folder.
Use the rm command with the -r option to recursively delete the folder and its content.
To remove the files inside the Finance folder, enter rm Finance/ProfitAndLossStatements.csv Finance/Salary.csv and press Enter.

To verify that the folder is empty, enter ls Finance and press Enter.

[ec2-user@ CompanyA]$ ls Finance
[ec2-user@ CompanyA]$
To remove the folder, enter rmdir Finance and press Enter.

To verify that the folder was removed, enter ls and press Enter.

[ec2-user@ companyA]$ ls
HR Management
[ec2-user@ companyA]$
To move the Management folder inside the HR folder, enter mv Management HR and press Enter.

To verify that the folder and files were moved, enter ls . HR/Management and press Enter.

[ec2-user@ CompanyA]$ ls . HR/Management
.:
HR

HR/Management:
Managers.csv  Schedule.csv
[ec2-user@ CompanyA]$
To navigate inside the HR folder, enter cd HR and press Enter.

To create the Employees folder, enter mkdir Employees and press Enter.

To move the files to this folder, enter mv Assessments.csv TrialPeriod.csv Employees and press Enter.

To verify that the files were moved, enter ls . Employees and press Enter.

[ec2-user@ HR]$ ls . Employees
.:
Employees  Finance  Management

Employees/:
Assessments.csv  TrialPeriod.csv
[ec2-user@ HR]$

# Managing Processes

## Create List of Processes
In this exercise, you will create a log file from the ps command. This log file should be added to the SharedFolders section:

Create a log file named processes.csv from ps -aux and omit any processes that contain root user or contain "["or"]" in the COMMAND section.

Note:
There is a space following the command followed by a period to represent the current location.

To validate that you are in the /home/ec2-user/companyA folder, enter pwd and press Enter. 

If you are not in this folder, enter cd companyA and press Enter.

View all processes running on the machine and filter out the word root by typing sudo ps -aux | grep -v root | sudo tee SharedFolders/processes.csv and pressing ENTER.

Validate your work by typing cat SharedFolders/processes.csv and pressing ENTER.
![EC2](../images/Linux/Processes1.PNG)

## List the processes using the top command
In this exercise, you will use the top command:

Run the top command to display processes and threads that are active in the system.
Observe the outputs of the top command.
In the main terminal run the command top and press ENTER:

### top
The top command is used to display the system performance and lists the processes and threads active in the system. The output of the top command should look similar to the picture below:
![EC2](../images/Linux/Processes2.PNG)

## Create a Cron Job
In this exercise, you will create a cron job that will create an audit file with ##### to cover all csv files:

Note:
You may have to use sudo to complete this exercise if you are not root.

Remember that cron is a command that runs a task on a regular basis at a specified time. This command maintains the list of tasks to run in a crontab file, which you create in this task. You create a job that creates the audit file with ##### in order to cover all .csv files. When you enter the crontab -e command, you are taken to an editor where you then enter a list of steps of what the cron daemon will run. The crontab file includes six fields: minutes, hour, day of month (DOM), month (MON), day of Week (DOW), and command (CMD). These fields can also be denoted with asterisks. Once this command runs, you can verify your work.

To validate that you are in the /home/ec2-user/companyA folder, enter pwd and press Enter.

To create a cron job that creates the audit file with ##### to cover all .csv files, enter sudo crontab -e and press Enter to enter the default text editor.

Press i to enter insert mode, and press Enter.

For the first line, enter SHELL=/bin/bash and press the Space bar.

For the second line, enter PATH=/usr/bin:/bin:/usr/local/bin and press Enter.

For the third line, enter MAILTO=root and press Enter.

For the last line, enter 0 * * * * ls -la $(find .) | sed -e 's/..csv/#####.csv/g' > /home/ec2-user/companyA/SharedFolders/filteredAudit.csv

Your terminal should look like the following image:

![The terminal window shows the establishment of a cron job. The cron job is used to create an audit file.](images/cron.jpg)

*Figure: In the terminal, it shows how the cron job with the SHELL, PATH, MAILTO, and a script that was referenced earlier in the lab.*
To save and close the file, press ESC. Then enter :wq and press Enter.
To validate your work, enter sudo crontab -l and press Enter. Inspect the crontab file to ensure that it matches the text exactly, as the following output shows:
![The terminal window show output from the command sudo crontab -l ](images/installed-cron.jpg)

*Figure: A validated cron job is shown by entering the command sudo crontab -l. The output of the command will be from the file that was entered from earlier in the lab.*
