# Update most of the software on your Ubuntu OS

Run the command:

`sudo apt upgrade`

# Edit a text using vim

Use the commands: h = left, l = right, j = down and k = up to navigate in the file
I typed r+the character that I want to replace. In this case I replaced the Vs for Cs.
I type s+character to inser the symbol : near the digit word.

![text_edited](https://github.com/ahammermuller/Exam_2420/blob/main/Images/text_edited.jpg)

# journalctl

How to use journalctl

![use_journalctl](https://github.com/ahammermuller/Exam_2420/blob/main/Images/use_journalctl.jpg)

- print logs for the current boot(-b)

![b_option](https://github.com/ahammermuller/Exam_2420/blob/main/Images/b.jpg)

- logs should have a priority of warning or more important(-p, --priority=)

![p_option](https://github.com/ahammermuller/Exam_2420/blob/main/Images/p.jpg)

- output in a nice pretty json. (-o json-pretty)

![o_option](https://github.com/ahammermuller/Exam_2420/blob/main/Images/o.jpg)

![json_pretty](https://github.com/ahammermuller/Exam_2420/blob/main/Images/json_pretty.jpg)

`journalctl -p warning -b -o json-pretty`

![journalctl](https://github.com/ahammermuller/ACIT-2420-LinuxSysAdmin/blob/main/Exam_2420/Images/journalctl.jpg)

# Script that finds all of the regular users on a system

First I created a new user with home directory

![useradd_user2](https://github.com/ahammermuller/Exam_2420/blob/main/Images/useradd_user2.jpg)

```
  1 #!/bin/bash
  2
  3 echo "Regular users on the system are"
  4 for userid in `awk -F: '{print $3}' /etc/passwd`
  5 do
  6 if (("$userid" >= 1000)); then
  7 echo `cat /etc/passwd | grep ":$userid:" | awk -F: '{print $1,$3}'`
  8 fi
  9 done
 10
 11 echo "Users currently logged in are:"
 12 echo `w`
```

![script_finduser](https://github.com/ahammermuller/Exam_2420/blob/main/Images/script_finduser1.jpg)

![result_findusers](https://github.com/ahammermuller/Exam_2420/blob/main/Images/result_findusers.jpg)

# Service file to run my script

![finduser_service](https://github.com/ahammermuller/Exam_2420/blob/main/Images/finduser_service.jpg)

Store your service file at /etc/systemd/system/

Type:
systemctl enable findusers.service
systemctl start findusers.service

# timer

![user_timer](https://github.com/ahammermuller/Exam_2420/blob/main/Images/user_timer.jpg)


