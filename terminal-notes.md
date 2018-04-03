# Terminal Training Video
https://terminal.training/
https://training.leftlogic.com/course/terminal/

### Terminal files:
https://remysharp.com/2013/07/25/my-terminal-setup

### WIFI Crontab
https://gist.github.com/remy/6079223#file-online-check-sh

How to schedule crons using `chrontab`
https://www.techradar.com/how-to/computing/apple/terminal-101-creating-cron-jobs-1305651


### TRLR
http://tldr.sh/

### Basic Commands

`ls -ltr` (l=list, t=time order, r=reverse)

`clear` and `control+l` clears the screen.

`cd -` previous directory.

`cd ~` home directory.

`which echo` tells where in the directory the file lives.

`ponysay "Hello Kenny"` Gemfile app - just for fun.

`which` locate a program file or alias.

### Homebrew

Best homebrew packages.
- `brew install gifify`
- `terminal-notifier`

### npm

`npm install -g json` json utility to read/pipe json

Echo example:

``` javascript
echo '{"age":10}' | json -e this.age++
{
  "age": 11
}
```

### Piping

``` bash
find . | grep .md | wc #wordcount
7       7     117

find . | grep .md | wc -l
7 #letters

find . | grep .md | wc -w
7 #words

find . | grep .md | wc -c
117 #characters

find. | less #paging through

/ .md #search inside less.

cat js-notes.md #content of files

cat js-notes.md | say

cat js-notes.md | wc #wordcount
```

### grep

``` bash
# search for "get" and display line number
cat js-notes.md | grep get -n

# number of instances of 'get'
cat js-notes.md | grep get -c

# same as above.
grep get -c js-notes.md

# show line number, 2 lines before and 2 lines after.
cat js-notes.md | grep get -n A 2 -B 2

# Matches even non-heading
cat js-notes.md | grep '#' -n

# Regex included.
cat js-notes.md | grep '^#' -n

# Paste board copy
cat js-notes.md | pbcopy

```

Twitter CLI
https://github.com/sferik/t

``` bash
# print the end of the file
tail freezing.log

# follow/continue the stream
tail -f freezing.log

# use regex 'egrep' for one or more space with @
tail freezing.log | egrep '^\s+@'

# Last 1000 with follow
tail -1000 -f freezing.log | egrep '^\s+@'

# get @ from file then see the top 10
egrep '^\s+@' freezing.log | head -10

head -n -2 [file] # = all except last 2
head -n +2 [file] # = first 2 lines

# Skip first 11 lines, then get last 10 lines.
egrep '^\s+@' freezing.log | head -11 | tail -10
```

`less` used for going into paused/interactive mode with search options.

Sort | Uniq

``` bash
# Uniq count, find text, Sort -numerical reverse, page through
cat demo | egrep '^\s+RT @' | sort | uniq -c | sort -nr | less
```

Permissions: mode & owner

`whoami`

`ls -ltr` (l=list, t=time order, r=reverse)

``` bash
drwx------
drwx------+
drwx------@

# easier than bitwise operator e.g., 777
sudo chmod g+rw secret.txt #global
sudo chmod u-x secret.txt #user
sudo chmod o+rwx secret.txt #other 

sudo chown # change owner
```

Health Check

`ps` processes I'm running
`ps auxww` show all running processes - Don't usually look at it like this.

``` bash
ps auxww | grep Chrome -i | grep 'kenny' | wc

ps auxww | grep Chrome -i | grep 'kenny' | awk '{ print $2 }' | xargs kill

pidof chrome
pifof sketch | xargs sudo kill
sudo kill -9 <processid> #force quit

df -h #human readable, 'h' can be even used on 'ls -h'

du #disk usage

# Output to report, sort, then anaylze the directories with most disk usage
du -h > report.txt
less !$
du > report.txt
sort -nr report.txt | less

top #native
htop # better view

uptime #shows uptime
```

### Customize Terminal

https://rem.io/terminal
https://rem.io/dotfile
https://rem.io/powerline

`gist` create gist from paste buffer

`gs` git status

### Webdev / Tools

Remy uses `curl` more than `wget`.

rem.io/perf

rem.io/wget-spider. Spider and get a website:
`wget -r -l4 -spider -D remysharp.com https://remysharp.com`

ngrok - expose localhost to the internet for testing.

`ccat` - syntax highting for cat. Alias cat to ccat

`json` - parse out data into json format.

`awk '{ print $10 }'` split output into columns.
https://training.leftlogic.com/course/terminal/play/cli-ch-plus-awk

### Watch - cool for monitoring for changes.

`watch -g -t --differences=permanent "curl -s https://somewebsite.com -I | grep X-Rate" && afplay /System/Library/Sounds/Hero.aiff
