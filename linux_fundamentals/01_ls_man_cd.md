# Welcome
Welcome to the first tutorial for Linux fundamentals. I'm excited to take you on a journey to get to know one of the most widely used operating systems. Well, Linux isn't really an operating system - its just a kernel, but we'll get to that.

## Are you new here?
You've just installed Linux for the first time and are greeted with a prompt. What do you do? You start poking around the filesystem to see what files exist. This is what I did the first time I booted linux successfully. At that stage, I only had print-outs of a few linux commands. Luckily we've come a long way since then.

Besides pressing enter a few times, which is helpful by the way, nothing will change. You'll need to learn 2 commands to start making your way around the filesystem.

# LetS get started
The first is `ls`, or "LiSt directory contents". Its super simple, but always useful. Lets try it out.

```
localhost% ls
localhost%
```

Nothing there. In my case, the directory is empty. You might see a list of files

```
localhost% ls
file1 file2 mydir
localhost%
```

For a more detailed view, you can use `-l` to list the files with some details

```
localhost% ls -l
total 4
-rw-r--r--  1 gevious gevious    0 Sep 20 22:13 file1
-rw-r--r--  1 gevious gevious    0 Sep 20 22:13 file2
drwxr-xr-x  2 gevious gevious 4096 Sep 20 22:13 mydir
localhost%
```

Its super useful being able to choose between 2 views, especially when you start writing scripts to automate some of the menial typing.

Sometimes, files are hidden. Linux has a convention that hidden files start with a dot '.'. You'll notice that none of the directories or files you listed started with a dot. Lets see if there are any hidden files.

```
localhost% ls -la
total 6
-rw-r--r--  1 gevious gevious    0 Sep 20 22:13 file1
-rw-r--r--  1 gevious gevious    0 Sep 20 22:13 file2
drwxr-xr-x  2 gevious gevious 4096 Sep 20 22:15 .hiddendir
-rw-r--r--  1 gevious gevious    0 Sep 20 22:15 .hiddenfile
drwxr-xr-x  2 gevious gevious 4096 Sep 20 22:13 mydir
localhost%
```

## Man oh man
`ls` has a lot of parameters or arguments you can pass into it. Most humans won't spend the time to memorize all of them (though I've seen people memorize strange and fun things for no reason at all). For the rest of us there is the `man` command. No, Linux isn't gender biased. Rather, this `man` command shows the manual page for a command. Try it out

```
localhost% man ls
```

Woah, a whole page of writing. You're now in an editor, likely a version of vi, that shows how you can interact with the `ls` command, and contains some meta information as well. You may even notice that one of the authors, [Richard Stallman](https://en.wikipedia.org/wiki/Richard_Stallman), who started the GNU Project, which helped popularize Linux, and is a strong advocate for free software.

The manual pages are organized into multiple sections, some for users like us, but others for super users and also developers. Since `ls` is an executable command, it is found in section one. Try the command below and you'll get the same result.

```
localhost% man 1 ls
```

To understand more about how the manual pages are organized you can look up the manual page for the `man` command.

```
localhost% man man
```

You can search through a page by typing '/' and then a word or phrase to search for. Typing 'gg' ends the game. Jokes, it takes you to the top of the manual page, while 'G' takes you to the bottom.

## CDs are not for music
Now its quite boring to just list a single directory. To change to another directory use the `cd` (Change Directory) command. You can traverse into a subdirectory from the current (or parent) directory by simply typing its name. This would be a relative reference.

```
localhost% cd mydir
```

You can also provide an absolute reference, starting from the root of the tree. This root is demarkated with a forward slash ('/').


```
localhost% cd /
localhost% cd /home/gevious
localhost% 
```

If you try traverse to a directory that doesn't exit you'll see an error, and you'll need to try again.


```
localhost% cd /foobar
cd: no such file or directory: /foobar
localhost% 
```

Weirdly, if you try to `cd` into a file, you get a different error.

```
localhost% cd /foobar
cd: not a directory: file1
localhost% 
```

# GGs
That should be enough to get you started. GGs, to you for getting this far. From here you can navigate the whole directory tree on your filesystem, and discover new commands from the manual pages.


# Appendix A - Commands 
`ls`
`cd`
`man`

