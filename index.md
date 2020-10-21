# Episode 2

## Exploring More `ls` Flags

You can also use two options at the same time. What does the command `ls` do when used
with the `-l` option? What about if you use both the `-l` and the `-h` option?

Some of its output is about properties that we do not cover in this lesson (such as file permissions and ownership), but the rest should be useful nevertheless.
<details>
<summary>Solution
</summary>
`-l` - long listing format, showing not only the file/directory names but also additional information such as the file size and the time of its last modification. 
`-h` + `-l` option -makes file size ‘Human readable’, i.e. displaying something like `5.3K` instead of `5369`.
</details>

## Listing in Reverse Chronological Order
By default ls lists the contents of a directory in alphabetical order by name. The command `ls -t` lists items by time of last change instead of alphabetically. The command `ls -r` lists the contents of a directory in reverse order. Which file is displayed last when you combine the `-t` and `-r` flags? Hint: You may need to use the `-l` flag to see the last changed dates.

<details>
<summary>Solution
`-rt` - most recently changed file. 
This can be very useful for finding your most recent edits or checking to see if a new output file was written.
</summary>
</details>

## Absolute vs Relative Paths
Starting from `/Users/amanda/data`, which of the following commands could Amanda use to navigate to her home directory, which is `/Users/amanda`?

1. `cd .`
2. `cd /`
3. `cd /home/amanda`
4. `cd ../..`
5. `cd ~`
6. `cd home`
7. `cd ~/data/..`
8. `cd`
9. `cd ..`


<details>
<summary>Solution
1. No: `.` stands for the current directory.
2. No: `/` stands for the root directory.
3. No: Amanda's home directory is `/Users/amanda`.
4. No: this goes up two levels, i.e. ends in `/Users`.
5. Yes: `~` stands for the user's home directory, in this case `/Users/amanda`.
6. No: this would navigate into a directory `home` in the current directory if it exists.
7. Yes: unnecessarily complicated, but correct.
8. Yes: shortcut to go back to the user's home directory.
9. Yes: goes up one level.
</summary>
</details>

## Relative Path Resolution
Using the filesystem diagram below, if `pwd` displays `/Users/thing`,
 what will `ls -F ../backup` display?
1.  `../backup: No such file or directory`
2.  `2012-12-01 2013-01-08 2013-01-27`
3.  `2012-12-01/ 2013-01-08/ 2013-01-27/`
4.  `original/ pnas_final/ pnas_sub/`

![File System for Challenge Questions](fig/filesystem-challenge.svg)


<details>
<summary>Solution
1. No: there *is* a directory `backup` in `/Users`.  
2. No: this is the content of `Users/thing/backup`, but with `..` we asked for one level further up.  
3. No: see previous explanation.  
4. Yes: `../backup/` refers to `/Users/backup/`.  
</summary>
</details>

## `ls` Reading Comprehension

Using the filesystem diagram below,
if `pwd` displays `/Users/backup`,
and `-r` tells `ls` to display things in reverse order,
what command(s) will result in the following output:

~~~
pnas_sub/ pnas_final/ original/
~~~

![File System for Challenge Questions](fig/filesystem-challenge.svg)

1.  `ls pwd`
2.  `ls -r -F`
3.  `ls -r -F /Users/backup`
<details>
<summary>Solution
1. No: `pwd` is not the name of a directory.
2. Yes: `ls` without directory argument lists files and directories in the current directory.
3. Yes: uses the absolute path explicitly.
</summary>
</details>

