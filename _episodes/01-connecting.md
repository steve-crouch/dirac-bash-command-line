---
title: "Connecting to the remote HPC system"
slug: dirac-bash-command-line-connecting-remote-hpc-system
teaching: 25
exercises: 10
questions:
- How do I open a terminal?
- How do I connect to a remote computer?
- What is an SSH key?
objectives:
- Connect to a remote HPC system.
- Understand the importance of keeping SSH keys, and any other credentials, secure.
- Describe the major benefit of 2-factor authentication.
keypoints:
- To connect to a remote HPC system using SSH and a password,
  run `ssh yourUsername@remote.computer.address`.
- To connect to a remote HPC system using SSH and an SSH key,
  run `ssh -i ~/.ssh/key_for_remote_computer yourUsername@remote.computer.address`.
- Protect your SSH keys by managing them carefully!
- 2-factor authentication is a way to help ensure a user's identity by requiring two forms of identity evidence.
---

## Opening a Terminal

Connecting to an HPC system is most often done through a tool known as "SSH"
(Secure SHell) and usually run through a terminal. To begin using an
HPC system, we need to begin by opening a terminal. Different operating systems
have different terminals, none of which are exactly the same in terms of their
features and abilities while working on the operating system. When connected to
the remote system the experience between terminals will be identical as each
will faithfully present the same experience of using that system.

Here is the process for opening a terminal in each operating system.

### Linux

There are many different versions (aka "flavours") of Linux and how to open a
terminal window can change between flavours. Fortunately most Linux users
already know how to open a terminal window since it is a common part of the
workflow for Linux users. If this is something that you do not know how to do,
then a quick search on the internet for "how to open a terminal window in" with
your particular Linux flavour appended to the end should quickly give you the
directions you need.

### MacOS

Macs have had a terminal built in since the first version since it is
built on a UNIX-like operating system, leveraging many parts from BSD (Berkeley
Software Distribution). The terminal can be quickly opened through the
the Spotlight Search tool. Hold down the command key and press the spacebar. In
the search bar that pops up type "terminal" and choose the terminal app from the
list of results (it will look like a tiny, black computer screen). You will
be presented with a terminal window. Alternatively, you can find Terminal under
"Utilities" in the applications Launchpad.

### Windows

While Windows does have a command-line interface known as the "Command Prompt"
that has its roots in MS-DOS (Microsoft Disk Operating System), it usually
does not have an SSH tool built into it and so one needs to be installed. There
are a variety of programs that can be used for this; a few common ones we
describe here, as follows:

#### Git Bash

Git Bash gives you a terminal like interface in Windows. You can use this to
connect to a remote computer via SSH. It can be downloaded for free from
[here](https://gitforwindows.org/).

#### Windows Subsystem for Linux

The Windows Subsystem for Linux also allows you to connect to a remote computer
via SSH. Instructions on installing it can be found
[here](https://docs.microsoft.com/en-us/windows/wsl/install-win10).

#### MobaXterm

MobaXterm is a terminal window emulator for Windows and the home edition can be
downloaded for free from
[mobatek.net](https://mobaxterm.mobatek.net/download-home-edition.html). If you
follow the link you will note that there are two editions of the home version
available: Portable and Installer. The portable edition puts all MobaXterm
content in a folder so that
it is easy to add plug-ins or remove the software. The installer edition adds
MobaXterm to your Windows installation and menu as any other program you might
install. If you are not sure that you will continue to use MobaXterm in the
future, the portable edition is likely the best choice for you.

Download the version that you would like to use and install it as you would any
other software on your Windows installation. Once the software is installed you
can run it by either opening the folder installed with the portable edition and
double-clicking on the executable file named `MobaXterm_Personal_11.1` (your
version number may vary) or, if the installer edition was used, finding the
executable through either the start menu or the Windows search option.

Once the MobaXterm window is open you should see a large button in the middle
of that window with the text "Start Local Terminal". Click this button and you
will have access to a terminal window.

#### PuTTY

It is strictly speaking not necessary to have a terminal running on your local
computer in order to access and use a remote system, only a window into the
remote system once connected. PuTTY is likely the oldest, most well-known, and
widely used software solution to take this approach.

PuTTY is available for free download from
[https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html).
Download the version that is correct for your operating system and install it
as you would other software on your Windows system. Once installed it will be
available through the start menu or similar.

Running PuTTY will not initially produce a terminal but instead a window full
of connection options. Putting the address of the remote system in the "Host
Name (or IP Address)" box and either pressing enter or clicking the "Open"
button should begin the connection process.

If this works you will see a terminal window open that prompts you for a
username through the "login as:" prompt and then for a password. If both of
these are passed correctly, then you will be given access to the system and will
see a message saying so within the terminal. If you need to escape the
authentication process you can hold the Control (<kbd>Ctrl</kbd>) key and press
the <kbd>c</kbd> key to exit and start again.

Note that you may want to paste in your password rather than typing it. Use
<kbd>Ctrl</kbd> plus a right-click of the mouse to paste content from the
clipboard to the PuTTY terminal.

## Creating an SSH key

SSH keys are an alternative method to using your password for authentication to
obtain access to remote computing systems. They can also be used for
authentication when transferring files or for accessing version control systems.

*As part of the registration process for DiRAC you should already have obtained and registered
SSH keys for accessing DiRAC resources, but the process will be also covered
here for completeness - as well as to emphasize safe SSH key management.*

This section will cover how to create a pair of SSH keys - a private key which you keep
on your own computer and a public key which is placed on the remote HPC system
that you will log in to.

### Linux, Mac and Windows Subsystem for Linux

SSH keys are stored, by default, in the `~/.ssh/` directory. Once you have
opened a terminal, check for existing SSH keys and filenames,
```
$ ls ~/.ssh/
```
{: .language-bash}

then generate a new public-private key pair,
```
$ ssh-keygen -t ed25519 -a 100 -f ~/.ssh/id_ed25519
```
{: .language-bash}

- `-o` (no default): use the OpenSSH key format,
  rather than the PEM format.
- `-a` (default is 16): number of rounds of passphrase derivation;
  increase to slow down brute force attacks.
- `-t` (default is [rsa](https://en.wikipedia.org/wiki/RSA_(cryptosystem))):
  specify the "type" or cryptographic algorithm.
  [ed25519](https://en.wikipedia.org/wiki/EdDSA) (or EdDSA)
 is faster and shorter than the default RSA algorithm for comparable strength.
- `-f` (default is /home/user/.ssh/id_algorithm): filename to store your keys.
  If you already have SSH keys, make sure you specify a different name:
  `ssh-keygen` will **overwrite** the default key if you don't specify!

If ed25519 is not available, use the older (but still strong and trusted)
[RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem)) cryptography:

```
$ ssh-keygen -o -a 100 -t rsa -b 4096 -f ~/.ssh/id_rsa
```
{: .language-bash}

The flag `-b` sets the number of bits in the key.
The default is 2048. EdDSA uses a fixed key length,
so this flag would have no effect.

When prompted, enter a strong password that you will remember - **do not be tempted
to enter a blank password!**
Cryptography is only as good as the weakest link, and this will be
used to connect to a powerful and precious computing resource.

Take a look in `~/.ssh` (using `ls ~/.ssh`). You should see the two
new files: your private key (`~/.ssh/key_ed25519`
or `~/.ssh/key_rsa`) and
the public key (`~/.ssh/key_ed25519.pub` or
`~/.ssh/key_rsa.pub`). If a key is
requested by the system administrators, the *public* key is the one
to provide.

### Windows

On Windows you can use one of the following:

- puttygen, see the Putty
  [documentation](https://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html)
- MobaKeyGen, see the MobaXterm
  [documentation](https://mobaxterm.mobatek.net/documentation.html)

> ##### Private keys are your private identity
>
> A private key that is visible to anyone but you should be considered compromised,
> and must be destroyed. This includes having improper permissions on the directory
> it (or a copy) is stored in, traversing any network in the clear, attachment on
> unencrypted email, and even displaying the key (which is ASCII text) in your
> terminal window.
>
> For DiRAC, how security is implemented often differs from site-to-site
> in terms of operating procedures, security policies, and mechanisms and tools 
> used for security. The key thing is to be aware of which programs are using
> your SSH keys as well as any other types of credential you may have. If using
> multiple DiRAC resources it's likely you will have multiple keys, so managing
> them in a secure way is very important.
> 
> Protect these keys as if they unlock your front door. In many ways, they do.
{: .caution}

> ## What is 2-factor authentication?
> 
> Another security mechanism used across some DiRAC sites is *2-factor authentication*.
> 2FA, as it is also known, is a method used to help ensure a user's identity when
> granting access to (typically) an online account or resource. With 2FA, a user needs
> to present at least two forms of evidence of identity in order to successfully gain 
> access to that resource.
> 
> Historically, passwords alone have been found to be inadequate. There have been many
> password-related security breaches in recent years due to many factors, including
> humans' poor memories, having too many accounts to maintain different password, and 
> a general 'security fatigue', all of which may lead to easily guessable or hackable
> passwords. 2FA attempts to solve this problem by having an additional "second factor"
> such as:
> 
> - Something you know: the most common form, such as a bank card PIN or answers to secret questions.
> - Something you have: such as a credit card, a smartphone with an authentication app,
> or a small device able to produce a security token.
> - Something you are: a more advanced category, this could include biometrics such as a
> fingerprint, or a voice scan.
> 
> Requiring multiple authentication factors is therefore based on the premise that someone
> who wishes to gain improper access is unlikely to be able to supply both required factors.
{: .callout}

> ## Further information
>
> For more information on SSH security and some of the
> flags set here, an excellent resource is
> [Secure Secure Shell](https://stribika.github.io/2015/01/04/secure-secure-shell.html).
{: .callout}


## Logging onto the system

Let's connect to a remote HPC system. In this
workshop, we will connect to DiRAC's COSMA (COSmology MAchine) &mdash; an HPC system
located at the University of Durham. Although it's unlikely that
every system will be exactly like this one across DiRAC, it's a very good
example of what you can expect from an HPC installation. To connect to our
example computer, we will use SSH.

SSH allows us to connect to UNIX computers remotely, and use them as if they
were our own. The general syntax of the connection command follows the format
`ssh -i ~/.ssh/key_for_remote_computer yourUsername@remote.computer.address`
when using SSH keys and `ssh yourUsername@some.computer.address` if only
password access is available.  Let's attempt to connect to the HPC system
now:

```
$ ssh -i ~/.ssh/id_ed25519 yourUsername@login7.cosma.dur.ac.uk
```
{: .language-bash}

or

```
$ ssh -i ~/.ssh/id_rsa yourUsername@login7.cosma.dur.ac.uk
```
{: .language-bash}

or if SSH keys have not been enabled

```
$ ssh yourUsername@login7.cosma.dur.ac.uk
```
{: .language-bash}


```
$ ssh yourUsername@login7.cosma.dur.ac.uk
ECDSA key fingerprint is SHA256:JRj286Pkqh6aeO5zx1QUkS8un5fpcapmezusceSGhok.
ECDSA key fingerprint is MD5:99:59:db:b1:3f:18:d0:2c:49:4e:c2:74:86:ac:f7:c6.
Are you sure you want to continue connecting (yes/no)?  # type "yes"!
Warning: Permanently added the ECDSA host key for IP address to the list of known hosts.
Last login: Wed Jun 15 09:29:21 2022 from globalprotect-nat-extbord.soton.ac.uk
Welcome to COSMA!

You are on a COSMA7 login node.  Please use this responsibly.  Do not start
any long-running jobs directly (submit to the batch queue instead), and do
not run processes which hog CPU.

For more information, please see https://www.dur.ac.uk/cosma

   COSMA TIP:
The c7jobload (and its 5, 6 and 8 variants) will show information (load,
memory usage) about currently running jobs.  c7jobload -l JOBID will
provide this information about a particular job ID.

[yourUsername@login7a [cosma7] ~]$ 
```
{: .output}

If you've connected successfully, you should see a prompt like the one below.
This prompt is informative, and lets you grasp certain information at a glance.
(If you don't understand what these things are, don't worry! We will cover
things in depth as we explore the system further.)

```
[yourUsername@login7a [cosma7] ~]$ 
```
{: .output}

There are several login nodes associated with the COSMA7 cluster, to which you will be allocated on in a round-robin fashion, so your prompt may your prompt may look slightly different.

In order to exit the shell - and subsequently close the remote connection - you can either type `exit` followed by enter, or press <kbd>Ctrl</kbd> + <kbd>D</kbd>.

## Telling the Difference between the Local Terminal and the Remote Terminal

You may have noticed that the prompt changed when you logged into the remote
system using the terminal (if you logged in using PuTTY this will not apply
because it does not offer a local terminal). This change is important because
it makes it clear on which system the commands you type will be run when you
pass them into the terminal. This change is also a small complication that we
will need to navigate throughout the workshop. Exactly what is reported before
the `$` in the terminal when it is connected to the local system and the remote
system will typically be different for every user. We still need to indicate
which system we are entering commands on though so we will adopt the following
convention:

- `[local]$` when the command is to be entered on a terminal connected to your
  local computer
- `[yourUsername@login7a [cosma7] ~]$` when the command is to be entered on a
  terminal connected to the remote system
- `$` when it really doesn't matter which system the terminal is connected to.

> ## Being certain which system your terminal is connected to
>
> If you ever need to be certain which system a terminal you are using is
> connected to then use the following command: `hostname`.
{: .callout}

> ## Keep two terminal windows open
>
> It is strongly recommended that you have two terminals open, one connected to
> the local system and one connected to the remote system, that you can switch
> back and forth between. If you only use one terminal window then you will
> need to reconnect to the remote system using one of the methods above when
> you see a change from `[local]$` to `{{ site.workshop_host_prompt }}` and
> disconnect when you see the reverse.
{: .callout}
