# Easy to learn
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

# random commands
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
