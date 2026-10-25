# Fa26 CS 415 Lecture Notes Repository

Code examples written in class, demos and other important resources will be found here.


## How to use this repository

Clone the repository to your computer *(do this once)*:

```
$ git clone https://github.com/Ensign-College/fa26-cs415-lecturenotes
```

I would like you to *experiment* with the demo programs I include in the lectures.  This is my invitation for you to change and/or break my code.  You can put all files in the repository back to their original state with this command:

```
$ git restore :/
```

Update your local repo with the latest notes *(do this after each lecture)*:

```
$ cd fa26-cs415-lecturenotes
$ git restore :/
$ git pull origin master
```


## Table of Contents


### Module 1 - Database Container and API
01. [Monday, October 26](./Module1/Lec01-Mon_Oct_26/README.md)
	* [Today's Spiritual Thought](./Module1/Lec01-Mon_Oct_26/README.md#todays-spiritual-thought)
	* [Course Overview](./Module1/Lec01-Mon_Oct_26/README.md#course-overview)
	* [Get To Know Your Professor](./Module1/Lec01-Mon_Oct_26/README.md#get-to-know-your-professor)
	* [Get To Know Each Other](./Module1/Lec01-Mon_Oct_26/README.md#get-to-know-each-other)
	* [Submitting Today's Exit Ticket](./Module1/Lec01-Mon_Oct_26/README.md#submitting-todays-exit-ticket)
	* [GitHub](./Module1/Lec01-Mon_Oct_26/README.md#github)
	* [Databases and Schemas](./Module1/Lec01-Mon_Oct_26/README.md#databases-and-schemas)
	* [Break](./Module1/Lec01-Mon_Oct_26/README.md#break)
	* [Work Time: Website Database Schema Diagram](./Module1/Lec01-Mon_Oct_26/README.md#work-time-website-database-schema-diagram)
	* [Wrap Up & Parting Thoughts](./Module1/Lec01-Mon_Oct_26/README.md#wrap-up--parting-thoughts)



## Creating a comprehensive study guide from individual lecture notes files

I have created a little tool to help you to easily find a topic when you don't
remember on which day it was covered or to create a study guide for an exam.

[concatenate.sh](./concatenate.sh) is a shell script that concatenates (joins)
all lecture note files found in this repository into one comprehensive file.
Only lecture notes named `README.md` are included; extra files such as code,
images and media are left out.

This program creates a read-only file called `all_notes.md`.

```
$ cd fa26-cs415-lecturenotes
$ git pull origin master
$ ./concatenate.sh
```

`all_notes.md`  is marked read-only to remind you to not make any important
changes as they will be destroyed the next time you run `concatenate.sh`.


### Troubleshooting

If `./concatenate.sh` yields error messages like these:

```
concatenate.sh: line 2: $'\r': command not found
concatenate.sh: line 9: syntax error near unexpected token `$'do\r''
```

it is because that script has MS-DOS line endings.  To fix it, try running this command **exactly** as written here:

```bash
$ sed -i -e 's/\r//' concatenate.sh
```

then re-run the concatenate script.
