---
title: Writing and reading files
slug: dirac-bash-command-line-writing-reading-files
teaching: 30
exercises: 15
questions:
- How do I create/edit text files?
- How do I move/copy/delete files?
objectives:
- Learn to use the `nano` text editor.
- Understand how to move, create, and delete files.
- Learn to copy files to/from a remote DiRAC resource.
- Use and create archival collections of files for efficient file transfer.
keypoints:
- There are many different text editors available on DiRAC.
- Use `nano` to create or edit text files from a terminal.
- Use `cat file1 [file2 ...]` to print the contents of one or more files to
  the terminal.
- Use `mv old dir` to move a file or directory `old` to another directory `dir`.
- Use `mv old new` to rename a file or directory `old` to a `new` name.
- Use `cp old new` to copy a file under a new name or location.
- Use `cp old dir` copies a file `old` into a directory `dir`.
- Use `rm old` to delete (remove) a file.
- File extensions are entirely arbitrary on UNIX systems.
- Use `scp` to transfer files from and to a remote DiRAC resource.
- Use `tar` to de-archive and archive sets of numerous and/or large files.
---

Now that we know how to move around and look at things, let's learn how to
read, write, and handle files! We'll start by moving back to our home directory
and creating a scratch directory:

```
$ cd ~
$ mkdir hpc-test
$ cd hpc-test
```
{: .language-bash}


## Creating and Editing Text Files

When working on an HPC system, we will frequently need to create or edit text
files. Text is one of the simplest computer file formats, defined as a simple
sequence of text lines.

What if we want to make a file? There are a few ways of doing this, the easiest
of which is simply using a text editor. DiRAC systems are based on Linux, which
fortunately comes with a number of editors pre-installed. Some of the more common
ones are:

- `vi`: a very basic text editor developed during the 1970's/80's. It differs from most editors - and is commonly found to be confusing because of it - in that it has two modes of operation: command and insert. In command mode, you are able to pass instructions to the editor, such as dealing with files (save, load, or insert a file), and editing (cut, copy, and paste text). However, you can't insert new characters. For that the editor needs to be in insert mode, which allows you to type into a text document. You can enter insert mode by typing <kbd>i</kbd>. To return to the command mode, you can use <kbd>Escape</kbd>.
- `vim`: built on `vi`, `vim` goes much further, adding features like undo/redo, autocompletion, search and replace, and syntax highlighting (which uses different coloured text to distinguish different programming language text). It mainly uses the same command/insert modes as `vi` which can take some getting used to, but is developed as a power-users editing tool that is highly configurable.
- `emacs`: also highly configurable and extensible, `emacs` has a less steep learning curve than `vim` but offers features common to many modern code editors. It readily integrates with debuggers, which is great if you need to find problems in your code as it runs.
- `nano`: a lightweight editor that also uses the more common way of allowing the editing of text by default, but allows you to access extra editor functionality such as search/replace or saving files by using <kbd>Ctrl</kbd> with other keys.

These are all text-based editors, in that they do not use a graphical user interface like Windows. They simply appear in the terminal, which has a key advantage, particularly for HPC systems like DiRAC: they can be used everywhere there is a terminal, such as via an SSH connection.

One of the common pitfalls of using Linux is that the `vi` editor is commonly set as the default editor. If you find yourself in `vi`, you can exit using <kbd>Escape</kbd> to get into command mode, and then <kbd>:</kbd> to enter a new command followed by <kbd>q</kbd> + <kbd>!</kbd>, which means quit `vi` without saving the file.

For this lesson, we are going to use `nano`, since it's more intuitive than many other terminal text editors.

To create or edit a file, type `nano <filename>`, on the terminal, where
`<filename>` is the name of the file. If the file does not already exist, it
will be created. Let's make a new file now, type whatever you want in it, and
save it.

```
$ nano draft.txt
```
{: .language-bash}

![Nano in action]({{ site.url }}{{ site.baseurl }}/fig/nano-screenshot.png)

Nano defines a number of *shortcut keys* (prefixed by the <kbd>Control</kbd> or
<kbd>Ctrl</kbd> key) to perform actions such as saving the file or exiting the
editor. Here are the shortcut keys for a few common actions:

* <kbd>Ctrl</kbd>+<kbd>O</kbd> &mdash; save the file (using a current filename or a
  new filename).

* <kbd>Ctrl</kbd>+<kbd>X</kbd> &mdash; exit the editor. If you have not saved
  your file upon exiting, `nano` will ask you if you want to save.

* <kbd>Ctrl</kbd>+<kbd>K</kbd> &mdash; cut ("kill") a text line. This command
  deletes a line and saves it on a clipboard. If repeated multiple times
  without any interruption (key typing or cursor movement), it will cut a chunk
  of text lines.

* <kbd>Ctrl</kbd>+<kbd>U</kbd> &mdash; paste the cut text line (or lines). This
  command can be repeated to paste the same text elsewhere.


> ## Using `vim` as a text editor
>
> From time to time, you may encounter the `vim` text editor. Although `vim`
> isn't the easiest or most user-friendly of text editors, you'll be able to
> find it on any system and it has many more features than `nano`.
>
> `vim` has several modes, a "command" mode (for doing big operations, like
> saving and quitting) and an "insert" mode. You can switch to insert mode with
> the `i` key, and command mode with `Esc`.
>
> In insert mode, you can type more or less normally. In command mode there are
> a few commands you should be aware of:
>
> * `:q!` &mdash; quit, without saving
> * `:wq` &mdash; save and quit
> * `dd` &mdash; cut/delete a line
> * `y` &mdash; paste a line
{: .callout}

Do a quick check to confirm our file was created.

```
$ ls
```
{: .language-bash}

```
draft.txt
```
{: .output}


## Reading Files

Let's read the file we just created now. There are a few different ways of
doing this, one of which is reading the entire file with `cat`.

```
$ cat draft.txt
```
{: .language-bash}

```
It's not "publish or perish" any more,
it's "share and thrive".
```
{: .output}

By default, `cat` prints out the content of the given file. Although `cat` may
not seem like an intuitive command with which to read files, it stands for
"concatenate". Giving it multiple file names will print out the contents of the
input files in the order specified in the `cat`'s invocation. For example,

```
$ cat draft.txt draft.txt
```
{: .language-bash}

```
It's not "publish or perish" any more,
it's "share and thrive".
It's not "publish or perish" any more,
it's "share and thrive".
```
{: .output}

> ## Reading Multiple Text Files
>
> Create two more files using `nano`, giving them different names such as
> `chap1.txt` and `chap2.txt`. Then use a single `cat` command to read and
> print the contents of `draft.txt`, `chap1.txt`, and `chap2.txt`.
{: .challenge}


## Creating Directory

We've successfully created a file. What about a directory? We've actually done
this before, using `mkdir`.

```
$ mkdir files
$ ls
```
{: .language-bash}
```
chap1.txt  chap2.txt  draft.txt  files
```
{: .output}


## Moving, Renaming, Copying Files

**Moving** &mdash; We will move `draft.txt` to the `files` directory with `mv`
("move") command. The same syntax works for both files and directories: `mv
<file/directory> <new-location>`

```
$ mv draft.txt files
$ cd files
$ ls
```
{: .language-bash}
```
draft.txt
```
{: .output}

**Renaming** &mdash; `draft.txt` isn't a very descriptive name. How do we go
about changing it? It turns out that `mv` is also used to rename files and
directories. Although this may not seem intuitive at first, think of it as
*moving* a file to be stored under a different name. The syntax is quite
similar to moving files: `mv oldName newName`.

```
$ mv draft.txt newname.testfile
$ ls
```
{: .language-bash}
```
newname.testfile
```
{: .output}

> ## File extensions are arbitrary
>
> In the last example, we changed both a file's name and extension at the same
> time. On UNIX systems, file extensions (like `.txt`) are arbitrary. A file is
> a `.txt` file only because we say it is. Changing the name or extension of
> the file will *never* change a file's contents, so you are free to rename
> things as you wish. With that in mind, however, file extensions are a useful
> tool for keeping track of what type of data it contains. A `.txt` file
> typically contains text, for instance.
{: .callout}

**Copying** &mdash; What if we want to copy a file, instead of simply renaming
or moving it? Use `cp` command (an abbreviated name for "copy"). This command
has two different uses that work in the same way as `mv`:

- Copy to same directory (copied file is renamed): `cp file newFilename`
- Copy to other directory (copied file retains original name): `cp file
  directory`

Let's try this out.

```
$ cp newname.testfile copy.testfile
$ ls
$ cp newname.testfile ..
$ cd ..
$ ls
```
{: .language-bash}

```
copy.testfile  newname.testfile
chap1.txt  chap2.txt  files  newname.testfile
```
{: .output}

## Removing files

We've begun to clutter up our workspace with all of the directories and files
we've been making. Let's learn how to get rid of them. One important note
before we start... **when you delete a file on UNIX systems, they are gone
*forever***. There is no "recycle bin" or "trash". Once a file is deleted, it
is gone, never to return. So be *very* careful when deleting files.

Files are deleted with `rm file [moreFiles]`. To delete the `newname.testfile`
in our current directory:

```
$ ls
$ rm newname.testfile
$ ls
```
{: .language-bash}

```
chap1.txt  chap2.txt  files  newname.testfile
chap1.txt  chap2.txt  files
```
{: .output}

That was simple enough. Directories are deleted in a similar manner using `rm
-r` (the `-r` option stands for 'recursive').

```
$ ls
$ rm -r files
$ ls
```
{: .language-bash}

> ## Beware the Destructive Power of `rm`!
>
> Depending on the system, using `rm -r` will not prompt you if you're sure you want to
> proceed with the deletion or not, and `rm -rf` will just do it. This is probably the
> scariest command on UNIX - it will delete a directory and all of its contents without prompting. **ALWAYS** double check your typing before using it... if you leave out the arguments, it will attempt 
> to delete everything on your file system that you have permission to delete. So when deleting 
> directories be very, very careful.
{: .caution}

```
chap1.txt  chap2.txt  files
chap1.txt  chap2.txt
```
{: .output}

> ## What happens when you use `rm -rf` accidentally
>
> Steam is a major online sales platform for PC video games with over 125
> million users. Despite this, it hasn't always had the most stable or
> error-free code.
>
> In January 2015, user kevyin on GitHub [reported that Steam's Linux client
> had deleted every file on his
> computer](https://github.com/ValveSoftware/steam-for-linux/issues/3671). It
> turned out that one of the Steam programmers had added the following line:
> `rm -rf "$STEAMROOT/"*`. Due to the way that Steam was set up, the variable
> `$STEAMROOT` was never initialized, meaning the statement evaluated to `rm
> -rf /*`. This coding error in the Linux client meant that Steam deleted every
> single file on a computer when run in certain scenarios (including connected
> external hard drives). Moral of the story: **be very careful** when using `rm
> -rf`!
{: .callout}

## Looking at files

Sometimes it's not practical to read an entire file with `cat`- the file might
be way too large, take a long time to open, or maybe we want to only look at a
certain part of the file. As an example, we are going to look at a large and
complex file type used in bioinformatics - a `.gtf` file. The GTF2 format is
commonly used to describe the location of genetic features in a genome.

Let's grab and unpack a set of demo files for use later. To do this, we'll use
[`wget`](https://www.gnu.org/software/wget/) (`wget link` downloads a file from
a link).

```
$ wget {{site.url}}{{site.baseurl}}/files/bash-lesson.tar.gz
```
{: .language-bash}

> ## Problems with `wget`?
>
> `wget` is a stand-alone application for downloading things over HTTP/HTTPS
> and FTP/FTPS connections, and it does the job admirably &mdash; when it is
> installed.
>
> Some operating systems instead come with [cURL]( https://curl.haxx.se/),
> which is the command-line interface to `libcurl`, a powerful library for
> programming interactions with remote resources over a wide variety of network
> protocols. If you have `curl` but not `wget`, then try this command instead:
>
> ```
> $ curl -O {{site.url}}{{site.baseurl}}/files/bash-lesson.tar.gz
> ```
> {: .language-bash}
>
> For very large downloads, you might consider using
> [Aria2](https://aria2.github.io/), which has support for downloading the same
> file from multiple mirrors. You have to install it separately, but if you
> have it, try this to get it faster than your neighbors:
>
> ```
> $ aria2c {{ site.url }}{{ site.baseurl }}/files/bash-lesson.tar.gz
> ```
> {: .language-bash}
>
> > ## Install cURL
> >
> > * macOS: `curl` is pre-installed on macOS. If you must have the latest
> >   version you can `brew install` it, but only do so if the stock version
> >   has failed you.
> > * Windows: `curl` comes preinstalled for the Windows 10 command line. For
> >   earlier Windows systems, you can download the executable
> >   [directly](https://curl.haxx.se/windows/); run it in place.
> >
> >   `curl` comes preinstalled in [Git for
> >   Windows](https://gitforwindows.org/) and [Windows Subsystem for
> >   Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10). On
> >   [Cygwin](https://www.cygwin.com/), run the setup program again and
> >   select the `curl` package to install it.
> > * Linux: `curl` is packaged for every major distribution. You can install
> >   it through the usual means.
> >   - Debian, Ubuntu, Mint: `sudo apt install curl`
> >   - CentOS, Red Hat: `sudo yum install curl` or `zypper install curl`
> >   - Fedora: `sudo dnf install curl`
>{: .solution}
>
> > ## Install Aria2
> >
> > * macOS: `aria2c` is available through a homebrew. `brew install aria2`.
> * Windows: download the latest
> > [release](https://github.com/aria2/aria2/releases) and run `aria2c` in
> > place. If you're using the [Windows Subsystem for Linux](
> > https://docs.microsoft.com/en-us/windows/wsl/install-win10),
> > * Linux: every major distribution has an `aria2` package. Install it by the
> >  usual means.
> >  - Debian, Ubuntu, Mint: `sudo apt install aria2`
> >  - CentOS, Red Hat: `sudo yum install aria2` or `zypper install aria2`
> >  - Fedora: `sudo dnf install aria2`
>{: .solution}
{: .callout}

You'll commonly encounter `.tar.gz` archives while working in UNIX. To extract
the files from a `.tar.gz` file, we run the command `tar -xvf filename.tar.gz`:

```
$ tar -xvf bash-lesson.tar.gz
```
{: .language-bash}

```
dmel-all-r6.19.gtf
dmel_unique_protein_isoforms_fb_2016_01.tsv
gene_association.fb
SRR307023_1.fastq
SRR307023_2.fastq
SRR307024_1.fastq
SRR307024_2.fastq
SRR307025_1.fastq
SRR307025_2.fastq
SRR307026_1.fastq
SRR307026_2.fastq
SRR307027_1.fastq
SRR307027_2.fastq
SRR307028_1.fastq
SRR307028_2.fastq
SRR307029_1.fastq
SRR307029_2.fastq
SRR307030_1.fastq
SRR307030_2.fastq
```
{: .output}

> ## Unzipping files
>
> We just unzipped a .tar.gz file for this example. What if we run into other
> file formats that we need to unzip? Just use the handy reference below:
>
> * `gunzip` extracts the contents of .gz files
> * `unzip` extracts the contents of .zip files
> * `tar -xvf` extracts the contents of .tar.gz and .tar.bz2 files
{: .callout}

That is a lot of files! One of these files, `dmel-all-r6.19.gtf` is extremely
large, and contains every annotated feature in the *Drosophila melanogaster*
genome. It's a huge file- what happens if we run `cat` on it? (Press `Ctrl + C`
to stop it).

So, `cat` is a really bad option when reading big files... it scrolls through
the entire file far too quickly! What are the alternatives? Try all of these
out and see which ones you like best!

- `head file`: Print the top 10 lines in a file to the console. You can control
  the number of lines you see with the `-n numberOfLines` flag.
- `tail file`: Same as `head`, but prints the last 10 lines in a file to the
  console.
- `less file`: Opens a file and display as much as possible on-screen. You can
  scroll with `Enter` or the arrow keys on your keyboard. Press `q` to close
  the viewer.

Out of `cat`, `head`, `tail`, and `less`, which method of reading files is your
favourite? Why?


## Copying files between your local system and DiRAC

Computing with a remote computer offers very limited use if we cannot get files
to or from the remote resource.

To copy a single file to or from the cluster, we can use `scp` ("secure copy"). `scp` is like `cp`, but makes use of a secure SSH connection to perform the copy. The syntax can be a little complex for new users, but we'll break it down.

To *upload to* another computer we can use something like:

```
$ scp path/to/local/file.txt yourUsername@dirac-resource.ac.uk:/path/on/dirac-resource
```
{: .language-bash}

To *download from* another computer:

```
$ scp yourUsername@dirac-resource.ac.uk:/path/on/dirac-resource/file.txt path/to/local/
```
{: .language-bash}

Note that everything after the `:` is relative to our home directory on the
remote computer. We can leave it at that if we don't care where the file goes, e.g.

```
$ scp local-file.txt yourUsername@login7.cosma.dur.ac.uk:
```
{: .language-bash}

> ## Upload a File
>
> Download the `bash-lesson.tar.gz` file to your local machine, and then copy it to the DiRAC system.
> If you're on Windows, feel free to use your SSH client tool to do this, or if you're on Linux or Mac (or have a variant of Bash on your Windows machine), use `scp` instead.
>
> > ## Solution
> >
> > ``` bash
> > $ scp bash-lesson.tar.gz yourUsername@login7.cosma.dur.ac.uk:
> > ```
> > {: .language-bash}
>{: .solution}
{: .challenge}

To copy a whole directory, we add the `-r` flag, for "**r**ecursive": copy the
item specified, and every item below it, and every item below those... until it
reaches the bottom of the directory tree rooted at the folder name you
provided. For example:

```
$ scp -r some-local-folder yourUsername@login7.cosma.dur.ac.uk:
```
{: .language-bash}

> ## Caution
>
> For a large directory -- either in size or number of files --
> copying with `-r` can take a long time to complete.
{: .callout}


## Archiving Files

One of the biggest challenges we often face when transferring data between
remote HPC systems is that of large numbers of files. There is an overhead to
transferring each individual file and when we are transferring large numbers of
files these overheads combine to slow down our transfers to a large degree.

This is where using `.tar.gz` files is really useful.
By *archiving* multiple files into smaller
numbers of larger files before we transfer the data to improve our transfer
efficiency. Plus, these types of file are also *compressed* to reduce
the amount of data we have to transfer and so speed up the transfer.

The most common archiving command you will use on a (Linux) HPC cluster is
`tar`. `tar` can be used to combine files into a single archive file and,
optionally, compress it.

Let's start with the file we downloaded from the lesson site,
`bash-lesson.tar.gz`. The "gz" part stands for *gzip*, which is a
compression library. Reading this file name, it appears somebody took a folder
named "lesson-data", wrapped up all its contents in a single file with
`tar`, then compressed that archive with `gzip` to save space. Let's check
using `tar` with the `-t` flag, which prints the "**t**able of contents"
without unpacking the file, specified by `-f <filename>`, on the remote
computer. Note that you can concatenate the two flags, instead of writing
`-t -f` separately.

```
$ ssh yourUsername@login7.cosma.dur.ac.uk
$ tar -tf bash-lesson.tar.gz
dmel-all-r6.19.gtf
dmel_unique_protein_isoforms_fb_2016_01.tsv
gene_association.fb
SRR307023_1.fastq
SRR307023_2.fastq
SRR307024_1.fastq
SRR307024_2.fastq
SRR307025_1.fastq
SRR307025_2.fastq
SRR307026_1.fastq
SRR307026_2.fastq
SRR307027_1.fastq
SRR307027_2.fastq
SRR307028_1.fastq
SRR307028_2.fastq
SRR307029_1.fastq
SRR307029_2.fastq
SRR307030_1.fastq
SRR307030_2.fastq
```
{: .language-bash}

This shows a folder containing another folder, which contains a bunch of files.
Let's see about that compression, using `du` for "**d**isk
**u**sage".

```
[yourUsername@login7a [cosma7] ~]$ du -sh bash-lesson.tar.gz
12M     bash-lesson.tar.gz
```
{: .language-bash}

Now let's unpack the archive. We'll run `tar` with a few common flags:

* `-x` to e**x**tract the archive
* `-v` for **v**erbose output
* `-z` for g**z**ip compression
* `-f` for the file to be unpacked

When it's done, check the directory size with `du` and compare.

> ## Extract the Archive
>
> Create a new directory called `bash-lesson` using `mkdir`.
> Next, `cd` into that directory.
> Using the four flags above, unpack the lesson data using `tar`.
> Then, check the size of the whole unpacked directory using `du`.
>
> Hint: `tar` lets you concatenate flags.
>
> > ## Commands
> >
> > ``` bash
> > $ mkdir bash-lesson
> > $ cd bash-lesson
> > $ tar -xvzf ../bash-lesson.tar.gz
> > ```
> > {: .language-bash}
> >
> > ```
> > x dmel-all-r6.19.gtf
> > x dmel_unique_protein_isoforms_fb_2016_01.tsv
> > x gene_association.fb
> > x SRR307023_1.fastq
> > x SRR307023_2.fastq
> > x SRR307024_1.fastq
> > x SRR307024_2.fastq
> > x SRR307025_1.fastq
> > x SRR307025_2.fastq
> > x SRR307026_1.fastq
> > x SRR307026_2.fastq
> > x SRR307027_1.fastq
> > x SRR307027_2.fastq
> > x SRR307028_1.fastq
> > x SRR307028_2.fastq
> > x SRR307029_1.fastq
> > x SRR307029_2.fastq
> > x SRR307030_1.fastq
> > x SRR307030_2.fastq
> > ```
> >
> > Note that we did not type out `-x -v -z -f`, thanks to the flag
> > concatenation, though the command works identically either way.
> >
> > Whilst still in the `bash-lesson` directory:
> >
> > ``` bash
> > $ du -sh .
> > 123M    .
> > ```
> > {: .language-bash}
> >
> > ## Was the Data Compressed?
> >
> > Text files compress nicely: the "tarball" is one-quarter the total size of
> > the raw data!
>{: .solution}
{: .challenge}

If you want to reverse the process -- compressing raw data instead of
extracting it -- set a `c` flag instead of `x`, set the archive filename,
then provide a directory to compress. Make sure you're in the directory
that contains the `bash-lesson` directory first, then:

```
$ tar -cvzf compressed_data.tar.gz bash-lesson
```
{: .language-bash}
