Basic operations with directories
---------------------------------

This section introduces the basic commands that get you moving between different
locations in the file system and viewing the items in a folder or many folders.
If you are relatively new to the GNU/Linux system, you might want to read the
brief description of the Linux file system first, before diving into the
commands.

### Brief description of the Linux file system hierarchy
###### Tags: `#new-concept`, `#directory`, `#folder`, `#file`, `#hierarchy`, `#path`

The files & folders system in GNU/Linux (so-called "Linux") is hierarchical,
like Windows. This means that a folder can contain files, as well as other
folders, and those folders can contain more files and folders. All accessible
files and folders in a GNU/Linux system is stored in a folder, and this folder
is called the root folder, denoted "/". The below graph illustrates a small part
of a file system on a typical GNU/Linux machine, according to the hierarchy.

![File system hierarchy illustration](../img/fsh.png)

In a hierarchical file system like this, each file and folder has its own
address (commonly called **paths**). There are 2 types of paths:

1. **Absolute path**: An absolute path goes from the root folder (`/`) and
traces down to the destination folder/file. For example, in the system in the
above illustration, the absolute path of the file `bashrc` is `/etc/bashrc`.
2. **Relative path**: The relative path of a file or folder is the path relative
to a folder. For example, in the system illustrated above, the relative path of
the `boot.log` file relative to the `var` directory is `log/boot.log`. Unlike
absolute paths, relative paths don't begin with `/`.

As you might have noticed, GNU/Linux systems (and Unix systems in general) use
the forward slash character ("/") to separate folders at different hierarchical
level (unlike Windows, which use the backward slash character "\\").

There are three symbols, usually appear in paths, that are reserved for special
purposes:
1. The dot symbol ("."): The dot symbol refers to the current working directory.
2. The double dots symbol (".."): The double dots symbol refers to the parent
directory (i.e. the directory that is one level above in terms of hierarchy).
For example, in the system illustrated above, the relative path of the `README`
file to the `/usr/include` folder has to contain the double dots symbol:
`../../var/log/README`.
3. The tilde symbol ("~"): The tilde symbol refers to the home directory of the
current logged-in user. If you have logged in as a normal user, your home
directory should be `/home/USERNAME`, replacing "USERNAME" with your username
(e.g. if your username is "john", your home directory should be `/home/john/`).
If you have logged in as root (equivalence of admin in Windows), the home
directory should be `/root`.

Just one final note before we proceed: "folder" and "directory" are the same
concepts; the difference is, the term "folder" is used more in graphical
interface, while the term "directory" is used more when talking about the
command line interface. Hence, in this document, you will see the term
"directory" appears more.

### `pwd`
###### Tags: `#new-command`, `#new-concept`, `#directory`

At any time in the command line, you are in a directory, and it's called the
"current working directory". To see the _absolute path_ of that directory, enter
`pwd` (<b>p</b>rint <b>w</b>orking <b>d</b>irectory). Usually, when you just
jumped into the command line, the working directory is the home directory
(either `/root` or `/home/USERNAME`, replacing "USERNAME" with the logged in
user's name).

### Moving between directories using `cd`
###### Tags: `#new-command`, `#directory`, `#navigating`

`cd` (<b>c</b>hange <b>d</b>irectory) moves you to another folder on the system.

|Example input|Description|
|:--:|---|
|`cd /home/john/Pictures`|Move to directory `/home/john/Pictures` (absolute path).|
|`cd ../Code/Project1`|Move to directory `../Code/Project1` (relative path). Remember that the `..` symbol is<br>used to refer to the parent directory, as discussed in the introduction above.|
|`cd ~`|Move to the home directory. The `~` symbol refers to the home directory.|
|`cd -`|Move to the previous directory that you were in. For example, if you were in `/home/john`,<br>and then you moved to `/home/john/Pictures/Family`, doing `cd -` will move you back<br>to `/home/john`.|
|`cd ..`|Move to the parent directory.|
|`cd .`|Move to the current directory. This is useless, but is still shown up as an example to get<br>you to remember what the dot symbol `.` is for.|

### Moving between directories, advanced: `pushd`, `popd`, and `dirs`
###### Tags: `#new-command`, `#new-concept`, `#directory`, `#navigating`


#### The notion of stack and directory stack
###### Tags: `#new-concept`, `#directory`

Stacks to computer science is like telescopes to astronomy. In computing, you
can think of a stack as a collection of items. There are two basic operations
with stacks, they are **push** and **pop**. The push operation adds an item to
the _end_ of the stack, and the pop operation removes the most recently added
item.

Let's say we have a stack of fruit over here. They are numbered in order from
the least recently added item (number 1) to the most recently added item (number
5).

|     1      |   2    |     3      |  4   |     5     |
|   :---:    | :---:  |   :---:    |:---: |   :---:   |
|:strawberry:|:banana:|:watermelon:|:pear:|:pineapple:|

If we perform a push operation to our fruit stack to add a, say, melon :melon:
to our collection, the stack will become something like this:

|     1      |   2    |     3      |  4   |     5     |   6   |
|   :---:    | :---:  |   :---:    |:---: |   :---:   | :---: |
|:strawberry:|:banana:|:watermelon:|:pear:|:pineapple:|:melon:|

If we perform a single pop operation to the new stack (the one with 6 items
above, not the original one with 5 items), the stack will become something like
this:

|     1      |   2    |     3      |  4   |     5     |
|   :---:    | :---:  |   :---:    |:---: |   :---:   |
|:strawberry:|:banana:|:watermelon:|:pear:|:pineapple:|

Note that after the pop, the most recently added item - the melon :melon: - is
now removed from the stack. Pop one more time:

|     1      |   2    |     3      |  4   |
|   :---:    | :---:  |   :---:    |:---: |
|:strawberry:|:banana:|:watermelon:|:pear:|

Now that you know what a stack is, you might have guessed what a directory
stack is. A directory stack is a stack containing absolute paths that we've
pushed into it. You will see it in action by learning the `pushd`, `popd`, and
`dirs` command below.

#### `pushd`
###### Tags: `#new-command`, `#directory`, `#navigating`

Just like `cd`, `pushd` (**push** <b>d</b>irectory) moves you to a new
directory. However, `pushd` does one more thing: adding the new directory to the
directory stack.

Side notes: `cd .` is useless, but `pushd .` can be useful.

#### `dirs`
###### Tags: `#new-command`, `#directory`

`dirs` (<b>dir</b>ectory <b>s</b>tack) displays the directory stack.

Let's say you started at your home directory. Then you do
`pushd Pictures/Miscellaneous/`, and then `pushd ~/Videos/`, and then stop at
` pushd ../Templates/`. The output of `dirs` should be like this:

```
~/Templates ~/Videos ~/Pictures/Miscellaneous ~
```

The default display is, however, hard to interpret. If you want `dirs` to print
each path on its own line instead, feed `dirs` the `-p` option. So that is
`dirs -p`. The output should be like this:

```
~/Templates
~/Videos
~/Pictures/Miscellaneous
~
```

You can see that the most recently added element (`~/Templates`) is in the first
line of the output, and the least recently added element
(`~/Pictures/Miscellaneous`) is in the second last line of the output. The last
line of the output is the directory that you were in _before_ you did your first
`pushd` command.

The `~` symbol, as mentioned, refers to the home directory. If you don't want
`dirs` to use this symbol, but use the actual path instead, throw the `-l`
option at it. `dirs -l -p` should be like this (still in the context of our
example, assuming the username is "john"):

```
/home/john/Templates
/home/john/Videos
/home/john/Pictures/Miscellaneous
/home/john
```

To remove all the elements in the directory stack, type `dirs -c`.

Useful options for `dirs`, summarized in a table:

| Option |    Hint    |                                    Description                                    |
| :---:  |    ---     |                                        ---                                        |
|  `-l`  |  **l**ong  |Instead of writing the `~` symbol, write the actual path that the symbol refers to.|
|  `-p`  |**p**er line|Print each element on its own line.                                                |
|  `-c`  | **c**lear  |Clear the stack: Remove all the items in the directory stack.                      |

#### `popd`
###### Tags: `#new-command`, `#directory`

`popd` (**pop** <b>d</b>irectory) pops the most recently `pushd`ed path, and
moves you to the directory that is now the last element in our directory stack
(after the pop operation).

Example: Let's say your directory stack currently looks like this (this is from
a previous example):

```
~/Templates
~/Videos
~/Pictures/Miscellaneous
~
```

...in which `~/Templates` is the most recently added path. If you do a `popd`,
you will be moved to `~/Videos`, and `~/Templates` will be gone from the stack:

```
~/Videos
~/Pictures/Miscellaneous
~
```

`popd` won't do anything (but warns you) if the directory stack is empty.

### Viewing items in a directory: `ls` and `tree`
###### Tags: `#new-command`, `#view`, `#directory`

#### `ls`
###### Tags: `#new-command`, `#view`, `#directory`

`ls` (<b>l</b>i<b>s</b>t) is used to list files. If you only enter `ls` (with
no option), it will list all files and folders (excluding hidden ones) in the
current working directory. You can specify a directory's path to `ls` as an
argument, and `ls` will list the files and folders in that directory (e.g.
`ls /etc` will list all the files and folders in `/etc`).

It has several helpful options:

| Option |  Hint   |                                       Description                                       |
| :---:  |   ---   |                                           ---                                           |
|  `-A`  | **a**ll |List all files and folders, including the hidden ones.                                   |
|  `-l`  |**l**ong |Display list in long format (see below for more information).                            |
|  `-s`  |**s**ize |Display size of each file, should be used with the -h option.                            |
|  `-h`  |**h**uman|Display size in a human readable form (e.g. "5.5M" for 5.5 megabytes instead of "5500"). |

Note that you must put the options before the directory you wish to look inside
(e.g. `ls -l -A /proc` will list all the files and folders (including hidden
ones) in the `/proc` directory with long listing format).

If you use the long listing format, you will see something like this:

```
drwxrwxr-x.  3 john john 4096 Jul 13 21:40 basecalc
drwxrwxr-x.  9 john john 4096 Jul 19 18:39 firefox-dev
drwxrwxr-x. 16 john john 4096 Jul 17 14:16 inkscape
-rw-rw-r--.  1 john john  578 Jul 19 11:04 Inkscape-Dependencies.txt
drwxrwxr-x.  2 john john 4096 Jun 21 21:17 iso
drwxrwxr-x.  4 john john 4096 Jul  8 13:38 TerminalImageViewer
drwx------.  3 john john 4096 Jul 12 22:57 tor-browser
```

...which is to say, there are 7 columns. The meaning of each column is as
follows:

1. File permission
2. Number of links
3. File/Folder's owner
4. File/Folder's owner group
5. File size (in bytes, but can be made easier to interpret with -h option)
6. When was the file/folder last modified? (this one actually spans to multiple
columns)
7. The file/folder's name

#### `tree`
###### Tags: `#new-command`, `#view`, `#directory`, `#hierarchy`

`ls dir` lists the files and folders inside the `dir` directory, but if `dir`
actually has sub-directories in it, `ls` won't list the items inside those
sub-directories. There's actually an option for `ls` to do that, but why would
you use it when you have `tree`?

The `tree` command lists file and folders, recursively, in a hierarchical tree
format. This means that `tree` displays the hierarchical relationship between
files and folders (e.g. which folder does this file reside in). An example of
an output of `tree` is as follow:

```
wfind
├── example
│   ├── file1
│   ├── file2
│   └── file3
├── lib
│   ├── wfind.cpp
│   └── wfind.hpp
├── main.cpp
├── Makefile
├── README.md
├── UNLICENSE
└── wfind

2 directories, 10 files
```

By looking at the output above, you can interpret that `file1`, `file2`, `file3`
are inside the `example` folder, `wfind.cpp` is in `lib` folder, and, of course,
`lib` and `example` folders are inside the `wfind` folder.

Just like `ls`, `tree` does not list hidden files by default. Instead, you have
to throw the `-a` option in. Here are some useful `tree` options:

| Option |    Hint     |                             Description                              |
| :---:  |     ---     |                                 ---                                  |
|  `-a`  |   **a**ll   |List all files and folders, including the hidden ones.                |
|  `-d`  |**d**irectory|List folders only, don't list files.                                  |
|  `-f`  |  **f**ull   |Print the absolute path, not just the name of the files/folders found.|

### Creating a new directory
###### Tags: `#new-command`, `#new`, `#directory`

To create a new directory, use the `mkdir` (<b>m</b>a<b>k</b>e <b>dir</b>ectory)
command:

```
mkdir newdir
```

...replacing "newdir" with the name of the new directory at your choice. This
will create a new directory in the current working directory. If you want to
create a new directory in somewhere else that is not the current working
directory, say, `/home/john/Programs`, you can do:

```
mkdir /home/john/Programs/newdir
```

Note that in the above command, if the parent directory `/home/john/Programs`
does not exist, an error will be thrown. If you want `mkdir` to create those
parent directories for you (in case they don't exist), simply feed it with the
`-p` option.

```
mkdir -p /home/john/Programs/newdir
```

### Removing a directory
###### Tags: `#new-command`, `#remove`, `#directory`

`rm -r dir`, replacing "dir" with the name of the directory you want to remove.
The `rm` command (<b>r</b>e<b>m</b>ove) can also be used to delete files, which
we will discuss later. The `-r` option must be specified so that `rm` can remove
the directory <b>r</b>ecursively (meaning all files and folders inside that
directory will also be deleted).