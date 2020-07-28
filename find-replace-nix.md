# Linux: how to find and replace text in multiple files

Harness the power of grep and sed.

Often times I need to search and replace a string of text across multiple files in my Linux box. After a bit of research I've come up with a nice solution. Assuming that you want to search for the string `search` through multiple files and replace it with `replace`, this is the one-liner:

```
grep -RiIl 'search' | xargs sed -i 's/search/replace/g'
```

Let me now dissect it and take a quick look at the different tools in use.

## `grep`

`grep` is a utility for searching for strings through multiple text files. Here I'm invoking it with the following parameters:

- `R` — perform a recursive search, also across symbolic links;
- `i` — case-insensitive search
- `I` — skip binary files. We are working with text, afer all;
- `l` — print results as a simple list of file names. This is needed for the next command.

The output of `grep` is then piped to ...

## `xargs`

This is a little command-line utility that takes what receives in input and passes it *as argument* to another program. So in this example the output of `grep` is passed to the next command `sed` as its argument.

## `sed`

`sed` is a glorious Unix utility that transforms text. In the current snippet I'm using it to replace text with the following parameters:

- `i` — replace in file. Remove it for a dry run mode;
- `s/search/replace/g` — this is the substitution command. The `s` stands for substitute (i.e. replace), the `g` instructs the command to replace all occurrences.

## Fine tuning 1: how to exclude directories while searching

You can add the `--exclude-dir=<dir>` parameter to `grep` if you want to skip a specific directory while searching for files. For example, say you want to skip the `tests/` directory:

```
grep -RiIl --exclude-dir=tests 'search' | xargs sed 's/search/replace/g'
```

Exclude multiple directories by wrapping them into curly braces, like so:

```
grep -RiIl --exclude-dir={dir1,dir2,dir3} 'search' | xargs sed 's/search/replace/g'
```

## Fine tuning 2: regular expressions

Both `grep` and `sed` support regular expressions, so you can search with `grep` given a specific pattern and then replace the text with `sed` given another one. Take a look at the [grep manual](http://man7.org/linux/man-pages/man1/grep.1.html) and the [sed manual](https://linux.die.net/man/1/sed) for more information.

## Sources

StackOverflow - [How to replace a string in multiple files in linux command line](https://stackoverflow.com/questions/11392478/how-to-replace-a-string-in-multiple-files-in-linux-command-line/20721292)

[Linux](https://www.internalpointers.com/tag/linux) • [Unix](https://www.internalpointers.com/tag/unix) • [grep](https://www.internalpointers.com/tag/grep) • [sed](https://www.internalpointers.com/tag/sed) • [text](https://www.internalpointers.com/tag/text)

