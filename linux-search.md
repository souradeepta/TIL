# Linux / UNIX Recursively Search All Files For A String

## grep command: Recursively Search All Files For A String

The syntax is:
`cd /path/to/dirgrep -r "word" .`
`grep -r "string" .`
To ignore case distinctions:
`grep -ri "word" .`
To display print only the filenames with GNU grep, enter:
`grep -r -l "foo" .`
You can also specify directory name:
`grep -r -l "foo" /path/to/dir/*.c`

## find command: Recursively Search All Files For A String

find command is recommend because of speed and ability to deal with filenames that contain spaces.

```
cd /path/to/dir
find . -type f -exec grep -l "word" {} +
find . -type f -exec grep -l "seting" {} +
find . -type f -exec grep -l "foo" {} +
find /search/dir/ -type f -name "*.c" -print0 | xargs -I {} -0 grep "foo" "{}"
 
## Search /etc/ directory for 'nameserver' word in all *.conf files ##
find /etc/ -type f -name "*.conf" -print0 | xargs -I {} -0 grep "nameserver" "{}"
```

Older UNIX version should use [xargs to speed](https://www.cyberciti.biz/faq/linux-unix-bsd-xargs-construct-argument-lists-utility/) up things:
`find /path/to/dir -type f | xargs grep -l "foo"`
It is good idea to pass -print0 option to find command that it can deal with filenames that contain spaces or other metacharacters:
`find /path/to/dir -type f -print0 | xargs -0 grep -l "foo"`
OR use the following OSX/BSD/find or GNU/find example:

```
find /path/to/dir/ -type f -name "file-pattern" -print0 | xargs -I {}  -0 grep -l "search-term" "{}"
 
## OR ##
find /mycool/project/ -type f -name "*.py" -print0 | xargs -I {}  -0 grep -H --color "methodNameHere" "{}"
 
## OR search all files in /etc/ dir for 'nameserver' word ##
find /etc/ -iname "*" -type f -print0  |  xargs -0 grep -H "nameserver"
```

Sample outputs from the last command:

[![img](data:image/svg+xml;base64,PHN2ZyBoZWlnaHQ9JzE0Nycgd2lkdGg9JzU5OScgeG1sbnM9J2h0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnJyB2ZXJzaW9uPScxLjEnLz4=)![Fig.01: Unix and Linux: How to Grep Recursively?](https://www.cyberciti.biz/media/new/faq/2007/07/unix-grep-recursively.jpg)](https://www.cyberciti.biz/faq/howto-recursively-search-all-files-for-words/unix-grep-recursively/)





https://www.cyberciti.biz/faq/howto-recursively-search-all-files-for-words/