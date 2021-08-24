---
layout: post
title: Command Notes
category: blue
---

_A collection of useful commands_
```bash
#List all files in long listing in human readable format sorted by time
ls -halt
```

```bash
#Find files in a directory that are larger than 250MB and for each file found ls them
find / -type f -size +250M -exec ls -halt {} \;
```

```bash
#Removes the files found in the above command
find / -type f -size +250M -exec rm -f {} \;
```

```bash
#Removes the files older than 90 days in /var/log/
find /var/log/* -mtime +90 -type f -exec rm -f {} \;
```

```bash
#Finds where java is then follows all the symlinks down to where its actually installed
readlink -f `which java`
```

```bash
#Create the user username, assigns thier home directory to /home/username, and set their shell to /bin/bash
useradd -m -d /home/username -s /bin/bash username
```

```bash
#Wipes out the contents of a file and then removes it; Faster than just removing a very large file
cat /dev/null > /path/to/file; rm -f /path/to/file
```

```bash
#Change ownership on a directory recursively
chown -R username:usergroup /path/to/directory
```

```bash
#Preserve the last 100 lines of a file wipe out the rest
echo $(tail -n 100 /path/to/file) > /path/to/file
```

```bash
#Run a file hosted on a webserver localy
curl -sSL http://webserver.web/path/to/raw/file | bash -s argument1 argument2 argument3
```

```bash
#Kickoff a process in the background under its own pid
nohup longRunningCommandHere > longRunningCommand.log 2>&1 &
```

```bash
#Set up a user so its password never expires
chage -E 1 -I 1 -m 0 -M 99999 -W 7 username; chage -l username
```

```bash
#Find all of the copies of Apache Struts running on a system and return their version numbers
#!/bin/bash
JarFiles=$(find / -type f -name struts.jar)
for Jar in $JarFiles; do
  JarVer=$(unzip -p "$Jar" META-INF/MANIFEST.MF | grep Specification-Version | awk -F: '{{print $2}}')
  echo "$Jar|$JarVer" >> "{parameter "outputFile"}"
done
```



```
du / | sort -n | less
```
