# Episode 6 Shell Scripts

## 6.1 List Unique Species

Leah has several hundred data files, each of which is formatted like this:

~~~
2013-11-05,deer,5
2013-11-05,rabbit,22
2013-11-05,raccoon,7
2013-11-06,rabbit,19
2013-11-06,deer,2
2013-11-06,fox,1
2013-11-07,rabbit,18
2013-11-07,bear,1
~~~

An example of this type of file is given in `data-shell/data/animal-counts/animals.txt`.

We can use the command `cut -d , -f 2 animals.txt | sort | uniq` to produce the unique species in `animals.txt`. In order to avoid having to type out this series of commands every time, a scientist may choose to write a shell script instead.

Write a shell script called `species.sh` that takes any number of filenames as command-line arguments, and uses a variation of the above command to print a list of the unique species appearing in each of those files separately.

<details>
<summary>Solution</summary>
	
~~~
# Script to find unique species in csv files where species is the second data field
# This script accepts any number of file names as command line arguments

# Loop over all files
for file in $@
do
	echo "Unique species in $file:"
	# Extract species names
	cut -d , -f 2 $file | sort | uniq
done
~~~
</details>

## 6.2 List Unique Species

~~~
$ history | tail -n 5 > recent.sh
~~~~
If you run the above command the last command in the file is the history command itself, i.e., the shell has added history to the command log before actually running it. In fact, the shell always adds commands to the log before running them. Why do you think it does this?

<details>
<summary>Solution</summary>
If a command causes something to crash or hang, it might be useful to know what that command was, in order to investigate the problem. Were the command only be recorded after running it, we would not have a record of the last command run in the event of a crash
</details>

## 6.3 Variables in Shell Scripts

In the molecules directory, imagine you have a shell script called `script.sh` containing the following commands:

~~~
head -n $2 $1
tail -n $3 $1
~~~

While you are in the `molecules` directory, you type the following command:

~~~
bash script.sh '*.pdb' 1 1
~~~

Which of the following outputs would you expect to see?

1. All of the lines between the first and the last lines of each file ending in `.pdb` in the molecules directory
2. The first and the last line of each file ending in `.pdb` in the `molecules` directory
3. The first and the last line of each file in the `molecules` directory
4. An error because of the quotes around `*.pdb`

<details>
<summary>Solution</summary>
The correct answer is 2.

The special variables $1, $2 and $3 represent the command line arguments given to the script, such that the commands run are:

~~~
$ head -n 1 cubane.pdb ethane.pdb octane.pdb pentane.pdb propane.pdb
$ tail -n 1 cubane.pdb ethane.pdb octane.pdb pentane.pdb propane.pdb
~~~

The shell does not expand `'*.pdb'` because it is enclosed by quote marks. As such, the first argument to the script is `'*.pdb'` which gets expanded within the script by `head` and `tail`.
</details>

## 6.4 Find the Longest File With a Given Extension

Write a shell script called `longest.sh` that takes the name of a directory and a filename extension as its arguments, and prints out the name of the file with the most lines in that directory with that extension. When the script is run as below, it should print the name of the .pdb file in /tmp/data that has the most lines.

~~~
$ bash longest.sh /tmp/data pdb
~~~

<details>
<summary>Solution</summary>

~~~
# Shell script which takes two arguments:
#    1. a directory name
#    2. a file extension
# and prints the name of the file in that directory
# with the most lines which matches the file extension.

wc -l $1/*.$2 | sort -n | tail -n 2 | head -n 1
~~~
The first part of the pipeline, `wc -l $1/*.$2 | sort -n`, counts the lines in each file and sorts them numerically (largest last). When there’s more than one file, `wc` also outputs a final summary line, giving the total number of lines across all files. We use `tail -n 2 | head -n 1` to throw away this last line.

With `wc -l $1/*.$2 | sort -n | tail -n 1` we’ll see the final summary line: we can build our pipeline up in pieces to be sure we understand the output.
</details>

## 6.5 Script Reading Comprehension

For this question, consider the data-shell/molecules directory once again. This contains a number of .pdb files in addition to any other files you may have created. Explain what each of the following three scripts would do when run as bash script1.sh *.pdb, bash script2.sh *.pdb, and bash script3.sh *.pdb respectively.

~~~
# Script 1
echo *.*
~~~
~~~
# Script 2
for filename in $1 $2 $3
do
    cat $filename
done
~~~
~~~
# Script 3
echo $@.pdb
~~~
<details>
<summary>Solution</summary>
In each case, the shell expands the wildcard in `*.pdb` before passing the resulting list of file names as arguments to the script.

Script 1 would print out a list of all files containing a dot in their name. The arguments passed to the script are not actually used anywhere in the script.

Script 2 would print the contents of the first 3 files with a `.pdb` file extension. `$1`, `$2`, and `$3` refer to the first, second, and third argument respectively.

Script 3 would print all the arguments to the script (i.e. all the `.pdb` files), followed by `.pdb`. `$@` refers to all the arguments given to a shell script.

~~~
cubane.pdb ethane.pdb methane.pdb octane.pdb pentane.pdb propane.pdb.pdb
~~~
</details>

## 6.6 Debugging Scripts
Suppose you have saved the following script in a file called `do-errors.sh` in Nelle’s `north-pacific-gyre/2012-07-03` directory:

~~~
# Calculate stats for data files.
for datafile in "$@"
do
    echo $datfile
    bash goostats $datafile stats-$datafile
done
~~~

When you run it:
~~~
$ bash do-errors.sh NENE*[AB].txt
~~~
the output is blank. To figure out why, re-run the script using the `-x` option:

~~~
bash -x do-errors.sh NENE*[AB].txt
~~~
What is the output showing you? Which line is responsible for the error?


<details>
<summary>Solution</summary>
The `-x` option causes `bash` to run in debug mode. This prints out each command as it is run, which will help you to locate errors. In this example, we can see that `echo` isn’t printing anything. We have made a typo in the loop variable name, and the variable `datfile` doesn’t exist, hence returning an empty string.
</details>
