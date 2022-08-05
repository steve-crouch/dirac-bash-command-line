---
title: "Using Bash Scripts in Pipes"
slug: dirac-bash-command-line-using-bash-scripts-pipes
teaching: 25 
exercises: 10
questions:
- "Can I use scripts I write in pipes as well?"
objectives:
- "Use a Bash script we've written within a pipe."
- "Create a Bash script that reads input from other commands within a pipe."
keypoints:
- "You can include your own Bash scripts in pipes."
- "A common and useful pattern in Bash shell is to run a program or script that generates potentially a lot of output, then use pipes to filter out what you're really after."
---

As we've seen in a previous lesson, one of the great things about shells like Bash is that you can chain commands together using pipes, but what about our own scripts?


## Using our own Scripts in Pipes

We've already seen how we can compose commands together using pipes, like so:

~~~
$ ls | head -5 | tail -2
~~~
{: .language-bash}

So what about our own scripts? The good news is that we can use scripts we write in pipes as well, chaining together sequences of commands using them too. And this emphasises a key strength of shells like Bash: the ability to compose simple programs together to accomplish complex tasks.

This simple idea is why systems like Unix - and its successors like Linux - have been so successful. Instead of creating enormous programs that try to do many different things, Unix programmers focus on creating lots of simple tools that each do one job well, and that work well with each other.

So let's go back to a previous script we've used. Replace `loop.sh` with the following:

~~~
#!/bin/bash

FILES=$(ls)
for VAR in $FILES
do
        echo $VAR
done
~~~
{: .language-bash}

And then run it within a pipe like any other command:

~~~
$ ./loop.sh | head -4
~~~
{: .language-bash}

~~~
bash-lesson.tar.gz
demo.sh
dmel-all-r6.19.gtf
dmel_unique_protein_isoforms_fb_2016_01.tsv
~~~
{: .output}

After printing out four lines of output, the pipe terminates.

## Accepting Input into Our Script

Oue previous example shows how we can include the output from our commands within a pipe. But what about how we might process input with our script? We can do this using `read` in Bash. Write a new script called `filter.sh`:

~~~
#!/bin/bash

while read line
do
   echo $line | grep "$1"
done
~~~
{: .language-bash}

What this script will do is continually read input using `read` until there is none left, at which point the script ends. For each line of input, we use `echo` and `grep` within a pipe to only filter out and only print any output that contains a particular word, specified as an argument to the script.

Then set its execute permissions, and execute it within a pipe like so:

~~~
$ chmod +x filter.sh
$ ./loop.sh | ./filter.sh dmel | head -n 1
~~~
{: .language-bash}

~~~
dmel-all-r6.19.gtf
~~~
{: .output}

Note that this pattern is quite a common one with the Bash shell: we're running a program that gives us potentially a lot of output, and we're filtering out in some way just what we're interested in.

> ## Simple is Good
> 
> Wherever possible, we should always try to simplify the code we write, removing any extraneous use of scripts or code that isn't needed. This enhances readability and makes our code easier to understand.
> 
> We've written a script that filters out output that ends in `0`. Instead of using `./filter.sh` in our pipe, what could we replace it with that would accomplish the same thing?
> 
> > ## Solution
> > 
> > ~~~ bash
> > $ ./loop.sh | grep "dmel" | head -n 1
> > ~~~
> > {: .language-bash}
> > 
> > By using the `grep` directly in the pipe, we've removed the need for a separate script, simplifying the pipe.
>{: .solution}
{: .challenge}
