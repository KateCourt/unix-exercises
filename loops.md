# Episode 5 Loops

The general form of a loop:

~~~
for thing in list_of_things
do
    operation_using $thing    # Indentation within the loop is not required, but aids legibility
done
~~~

## 5.1 Variables in loops

This exercise refers to the data-shell/molecules directory. ls gives the following output:

~~~
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
~~~

What is the output of the following code?

~~~
$ for datafile in *.pdb
> do
>    ls *.pdb
> done
~~~

Now, what is the output of the following code?

~~~
$ for datafile in *.pdb
> do
>	ls $datafile
> done
~~~

Why do these two loops give different outputs?

<details>
<summary>Solution</summary>
The first code block gives the same output on each iteration through the loop. Bash expands the wildcard *.pdb within the loop body (as well as before the loop starts) to match all files ending in .pdb and then lists them using ls. The expanded loop would look like this:

~~~
$ for datafile in cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> do
>	ls cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> done
~~~
~~~
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
~~~

The second code block lists a different file on each loop iteration. The value of the datafile variable is evaluated using $datafile, and then listed using ls.

~~~
cubane.pdb
ethane.pdb
methane.pdb
octane.pdb
pentane.pdb
propane.pdb
~~~
</details>

## 5.2 Limiting Sets of Files

What would be the output of running the following loop in the data-shell/molecules directory?

~~~
$ for filename in c*
> do
>    ls $filename
> done
~~~

1. No files are listed.
2. All files are listed.
3. Only `cubane.pdb`, octane.pdb and pentane.pdb are listed.
4. Only `cubane.pdb` is listed.

<details>
<summary>Solution</summary>
4 is the correct answer. * matches zero or more characters, so any file name starting with the letter c, followed by zero or more other characters will be matched.
</details>

How would the output differ from using this command instead?

~~~
$ for filename in *c*
> do
>    ls $filename
> done
~~~

1. The same files would be listed.
2. All the files are listed this time.
3. No files are listed this time.
4. The files cubane.pdb and octane.pdb will be listed.
5. Only the file octane.pdb will be listed.

<details>
<summary>Solution</summary>
4 is the correct answer. * matches zero or more characters, so a file name with zero or more characters before a letter c and zero or more characters after the letter c will be matched.
</details>

## 5.3 Saving to a File in a Loop - Part One

In the `data-shell/molecules` directory, what is the effect of this loop?

~~~
for alkanes in *.pdb
do
    echo $alkanes
    cat $alkanes > alkanes.pdb
done
~~~

1. Prints `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb`, `pentane.pdb` and `propane.pdb`, and the text from `propane.pdb` will be saved to a file called `alkanes.pdb`.
2. Prints `cubane.pdb`, `ethane.pdb`, and `methane.pdb`, and the text from all three files would be concatenated and saved to a file called `alkanes.pdb`.
3. Prints `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb`, and `pentane.pdb`, and the text from `propane.pdb` will be saved to a file called `alkanes.pdb`.
4. None of the above.

<details>
<summary>Solution</summary>
1 is the correct answer. The text from each file in turn gets written to the `alkanes.pdb` file. However, the file gets overwritten on each loop interation, so the final content of `alkanes.pdb` is the text from the `propane.pdb` file.
</details>

## 5.4 Saving to a File in a Loop - Part Two

Also in the `data-shell/molecules` directory, what would be the output of the following loop?

~~~
for datafile in *.pdb
do
    cat $datafile >> all.pdb
done
~~~

1. All of the text from `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb`, and `pentane.pdb` would be concatenated and saved to a file called `all.pdb`.
2. The text from `ethane.pdb` will be saved to a file called `all.pdb`.
3. All of the text from `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb`, `pentane.pdb` and `propane.pdb` would be concatenated and saved to a file called `all.pdb`.
4. All of the text from `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb`, `pentane.pdb` and `propane.pdb` would be printed to the screen and saved to a file called `all.pdb`.

<details>
<summary>Solution</summary>
3 is the correct answer. `>>` appends to a file, rather than overwriting it with the redirected output from a command. Given the output from the `cat` command has been redirected, nothing is printed to the screen.
</details>
