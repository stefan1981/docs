# Linux commands
```
alias                make a commando alias
apropos              search in manual
cal                  show calendar
cat                  display a file
cd                   change directory
chmod                change file permissions
chown                change owner
chpasswd             change password
chsh                 change login shell
clear                empty the screen
compgen              show all commands (-b buildin, -c all)
cp                   copy files and folders
cut                  cut columns
date                 show current date
df                   show free space (disk free)
dirs                 show a list of remembered directories
du                   show disk usage
echo                 echo a string
env                  show environment variables
exit                 close terminal
fg                   bring a job to the foreground
find                 find files and directories
file                 show filetype
free                 show machines memory
grep                 find a string
groups               list the groups you belong to
head                 display the head of a file
hostname             show computer name
htop                 system status (cpu, memory)
ifconfig             list network interfaces
ln                   create a symbolic link
look                 lookup a word in a dictionary
ls                   list content of folder
man                  show manual
more                 display file
mv                   move files and folders
nano                 simple editor
netstat              show network status
nl                   number a line
popd                 pop last entry from directory stack
ps                   show processes
pushd                save current directory in the dir-stack
pwd                  print working directory
scp                  copy files to/from a remote machine
tac                  display content of a file in reverse order
tail                 show end of file
uname                show os version
unzip                unzip files
wget                 download files
which                find location of a command

hard to learn
awk                  text processor
curl                 request webpages
crontab              manage scheduled jobs
docker               container management
sed                  mighty stream editor
ssh                  connect to remote machines
tmux                 screen splitter
vim                  mighty editor
```

# some common problem solutions
Getting the size of all files and folders inside the current directory can be done by typing:
```
 ls | xargs du -s -BM
```

Get filesize in descending order from all files in a recursive folder structure
```
find . -type f -exec du -a -h --max-depth=100 {} + | sort -hr
```

Another quite handy tool is ncdu. It provides you the disc usage in a graphical way on the shell. Install it like this:
```
sudo apt-get install ncdu
```

List folders bigger 1Mb and list them in descending order
```
du -sm * | awk '$1 > 1' | sort -n -r
```

List the ten largest folders inside a recursive folder structure
```
find . -type d -print0 | xargs -0 du | sort -n | tail -10 | cut -f2 | xargs -I{} du -sh {}
```

## perform action on multiple files
In reality you have quite often perform a certain command on several files. First i detect all files I'm looking for and store them in a filelist.

```
find . -name "myFilename" > filelist.txt
```

Second i run through this file, line by line and perform my command on all filename I've received one step before.

```
while read -r LINE; do echo "$LINE"; done < filelist.txt
```

You need to replace in several files. You can do it with sed. Be aware about masking special chars. In the example below the string "my\folder" is replaced with "your\folder".
Replacing with sed works like this: sed -i s#old#new#g
The delimiter (in this case the #) can be changed. The first sign after the s acts as delimiter.

```
while read -r LINE; do
sed -i s#\"my\/folder\",\"your\/folder\"#g "$LINE";
done < filenames.txt
```

## PDF frontpage to jpg
First find all the files you want to convert and store their filenames in a file

```
find my-pdf-folder/ -type f -name "*.pdf" > pdffiles.txt
```

Iterate through that files and make a jpeg from them

```
while read -r LINE; do pdftoppm -f 1 -singlefile -jpeg "$LINE" "$LINE"; done < pdffiles.txt
```

Finally move all jpgs to a separate location if necessary

```
find my-pdf-folder/ -type f -name "*.jpg" -exec mv {} jpgs/ \;
```

## Find files and strings on the shell
Find files and file content on the terminal can be achieved by the use of the find tool.

By far one of the best tools is "Silversearcher ag", install it with sudo apt-get install silversearcher-ag. If you want to search for example recursive all folders, only php files for the search string "my Value", simply type:
```
ag --php "my Value" .
```

Find file/s containing a specific string in its name:
```
find . -name "*mySearchString*" -type f
```

Now list all files that contain the word "searchTerm"
```
find . -type f -exec grep -l 'searchTerm' {} \;
```

List all the occurrence and their respective line-number of "searchTerm" in files. The parameter -n toggles the line-number, the parameter -H toggles the filename.
```
find . -type f -exec grep -Hn 'searchTerm' {} \;
```

Do you want to list all files bigger than size x? Here we go:
```
find . -type f -size +100M -exec ls -lh {} \;
```

A handy tool as well is ack-grep (you have to install it first). Be sure to wrap the search-term in single quotes.
```
 ack-grep --php '"the":"SearchTerm"'
```

## show word occurence in pdf on commandline
Do you want to know how often words occur in a pdf file? And sort them by the most occurring word:
```
pdftotext mypdf.pdf - | sed "s/[[:cntrl:][:digit:][:punct:]]//g" | tr '[:space:]' '[\n*]' | sort | uniq -c | sort -bnr
```
Let's break it down step by step:
```
pdftotext mypdf.pdf -
```

displays the pdf content on the command-line
```
sed "s/[[:cntrl:][:digit:][:punct:]]//g"
```

replaces all control characters (cntrl), all numbers (digits) and all punctuation characters (punct) with an empty string.
See here for character classes.
```
tr '[:space:]' '[\n*]'
```

replaces all spaces with a newline
```
sort | uniq -c | sort -bnr
```

The last part sorts the output, groups unique lines and prefix them with the amount and finally sort them again
with ignored leading blanks (-b), sort numeric (-n), in reverse order (-r)

## CURL
CURL is a mighty tool for transfering data to URLS. It supports a wide range of protocols.
Lets have a closer look on some handy snippets for daily work on the commandline:
Fetching content from a URL
```
curl http://www.myurl.com
```

When a page redirects to another location (the respondet http-status is somewhat 3xx)
curl can follow the redirection with the -L (- -location) flag.
```
curl -L http://www.myurl.com
```

Want to see what the request and response headers like?
It's as simple as using the -v (- -verbose) flag.
```
curl -v http://www.myurl.com
```

For POSTing data to an URL u can use the parameter -d (- -data) .
```
curl --data "param1=value1&param2=value2" http://www.myurl.com
```

You want to upload a file (image.jpg is located in the folder from where you run this command) to a php-script running at localhost/upload.php:
```
curl -F "data=@image.jpg" localhost/upload.php
```

a upload-script for taking the uploaded file and store it in a file - the filename is the current timestamp - looks like this:
```
<?php
if (isset($_FILES)) {
  move_uploaded_file($_FILES['data']['tmp_name'], date('YmdHis'));
  }
  ?>
```

This is for windows using the cmd. Parsing a folder recursively for all pdf files and send them as a POST request to a certain URI:
```
for /R %f in (*.pdf) do curl -F "data=@%f" www.mydomain.com/upload
```

## Monitoring network traffic
### tcpdump

You want to track all incomming http-Headers on your Webserver?
```
sudo tcpdump -A -s 10240 'tcp port 80' | egrep --line-buffered "^........(GET |HTTP\/|POST |HEAD )|^[A-Za-z0-9-]+: " | sed -r 's/^........(GET |HTTP\/|POST |HEAD )/\n\1/g'
```

Show the HTTP Requests only:

```
sudo tcpdump -A -s 10240 'tcp port 80' | egrep "^........(GET |HTTP\/|POST |HEAD )|^[:alnum:]+: " | sed -r 's/^........(GET |HTTP\/|POST |HEAD )/\n\1/g'
```

capture for a specific URL string (only inside the url's path)

```
sudo tcpdump -s0 -A -vv | grep "myPathString"
```


### tcptrack

A quite handy tool for monitoring tcp traffic is tcptrack. Get it with:

```
sudo apt-get install tcptrack
```

and start it with

```
sudo tcptrack -i eth0 port 80
```

the interface parameter -i eth0 must be according to your needs, you can check your interfaces with ifconfig.

There is also tcpdump, tcpflow and other nice tools

### Netstat
Sometimes one like to figure out what programs listen at a port. Here we can get an overview with:
```
sudo netstat -tupln
```

Where t=tcp, u=udp, p=show program name, l=show listening ports, n= numeric (not resolve machine names)
The Output could look something like this:

```
Proto Recv-Q Send-Q Local Address Foreign Address State PID/Program name
tcp 0 0 127.0.0.1:3306 0.0.0.0:* LISTEN 1338/mysqld 
tcp 0 0 127.0.1.1:53 0.0.0.0:* LISTEN 1709/dnsmasq 
tcp 0 0 0.0.0.0:22 0.0.0.0:* LISTEN 1303/sshd 
tcp 0 0 127.0.0.1:3350 0.0.0.0:* LISTEN 1900/xrdp-sesman
tcp 0 0 127.0.0.1:9050 0.0.0.0:* LISTEN 1356/tor 
tcp 0 0 0.0.0.0:3389 0.0.0.0:* LISTEN 1887/xrdp 
tcp6 0 0 :::80 :::* LISTEN 2064/apache2 
tcp6 0 0 :::22 :::* LISTEN 1303/sshd 
tcp6 0 0 :::443 :::* LISTEN 2064/apache2 
udp 0 0 0.0.0.0:35885 0.0.0.0:* 1709/dnsmasq 
udp 0 0 0.0.0.0:5353 0.0.0.0:* 7335/chrome 
udp 0 0 0.0.0.0:5353 0.0.0.0:* 1062/avahi-daemon: 
udp 0 0 0.0.0.0:55935 0.0.0.0:* 1062/avahi-daemon: 
udp 0 0 127.0.1.1:53 0.0.0.0:* 1709/dnsmasq 
udp 0 0 0.0.0.0:68 0.0.0.0:* 1698/dhclient 
udp 0 0 0.0.0.0:631 0.0.0.0:* 4625/cups-browsed
udp6 0 0 :::51764 :::* 1709/dnsmasq 
udp6 0 0 :::45088 :::* 1062/avahi-daemon: 
udp6 0 0 :::5353 :::* 7335/chrome 
udp6 0 0 :::5353 :::* 1062/avahi-daemon:
```

### lsof
With lsof you can determine easily which program listens on a port and under which user that program runs:
```
sudo lsof -i :80
```

### mtr
Need Ping  and traceroute combined in a single application? Use mtr (matt's traceroute)
```
mtr google.com
```

# Linux tutorial
These tutorials explain the Linux operating system (os) step by step. Starting with simple examples and getting more advanced each lesson.

The line where we type in the commands is called the prompt, the blinking something right to it is the cursor.

Lets show who is logged in:
```
whoami
```

And the hostname of the computer can be shown with:
```
hostname
```

As you can see, username and hostname are contained in the prompt.

If you want to know the name of the operating system, you can use:
```
uname
```

How long the computer is up (not rebooted since) shows the following command:
```
uptime
```

Clear all the stuff on the screen with:
```
clear
```

If you using the arrow keys (up and down), you can browse between the commands you already entered.

When you want to know in which folder you are, use:
```
pwd
```

It's short for: print working directory.

Let's go to your users home directory by typing:
```
cd
```

At this location, all the files related to your current user are stored. You home-folder is a special folder. The tilde sign "~" is a synonym for your home folder. Instead of typing "cd" you can type "cd ~" as well. When you're in your home folder you can see the tilde in the prompt. The tilde sign is a variable. You can output its content to the screen:
```
echo ~
```

Create a directory named garage withe this command:
```
mkdir garage
```

mkdir is short for: make directory

Most important command of all is "cd". It means change directory. With this command you can browse in the directory tree.
Let's switch to the directory "garage" you recently created by typing:
```
cd garage
```

The prompt, by the way, also shows you in which directory you are. (So you don't have to type pwd all the time)

If I want to go back in the directory-tree, like one level up, we use:
```
cd ..
```

Don't forget a space between cd and the two dots. The two dots are a symbol for the upper (superordinate) folder.

If you type the first letters of a command, try to press two times the tab-key, and the command will be completed.
This will save your time. It's one of the most useful ways to increase your working speed.

Commands you have head about in Part1 are:
whoami, hostname, uname, uptime, clear, pwd, cd, echo, ~ and others.

In the second part of the Linux Tutorial, we will learn about dealing with files and folders (directory).

First move to your home directory with (change directory):
```
cd
```

Now let us create a directory, named "garage", with the "make directory" command:
```
mkdir garage
```

You have created the garage directory, but you are still inside your home directory. Go into the garage directory with:
```
cd garage
```

Notice,  that the "change directory" is by far the most important command. You will see, that you will use this command very often.
In the next step, you go "one directory up", so that you will be in your home directory again. This can be achieved with:
```
cd ..
```

Don't forget the space between "cd" and the two dots. The two dots is a sign for the directory above. There is a top-level directory, called the root. There is no directory above the root directory. You can go there with:
```
cd /
```

Again, there is a space between "cd" and the "slash". If you are curious whats inside the root folder, type:
```
ls -l
```

The list command, with the parameter "l", shows you the directories content. Time to go back into our garage directory inside our home directory. Go there with:
```
cd ~/garage
```

The tilde (press Alt-key + tilde-sign) is a sign for our home folder. And we want to go inside the garage folder, which is inside our home folder. We create now a file called "mycar.txt" and write "bmw" inside this file. Here we go:
```
echo "bmw" > mycar.txt
```

The echo command simply displays the word in quotes, in this case "bmw". The > character redirects the result ("bmw") into the file, which came behind the ">" sign. Btw: a single ">" always create an empty file, even when the file already exists. When you want to attach something to a file simply use ">>". Let's look into the file:
```
cat mycar.txt
```

We want to copy the file mycar.txt:
```
cp mycar.txt mybike.txt
```

And now we want to attach something to our new file:
```
echo "is not a car" >> mybike.txt
```

Check the content of our new file with:
```
cat mybike.txt
```

And check the content of our garage folder with:
```
ls -l
```
