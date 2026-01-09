---
title: "Using a Shell"
weight: 5
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
# bookIcon: ''
---

# Shell

The **shell** is a robust communication interface between you and your operating system. This section is concerned with the basics of navigating and manipulating the file-tree accessible via the Bourne Again Shell - **BA$H**.

## Navigation in the Shell

The prompt is the textual input into the shell. This is where you'll execute commands and view their output, usually designated by the **$**. The text to the left of the prompt contains details about your session such as user, hostname, and current working directory or path; i.e. `user@hostname ~/current/working/directory $ _`. 

The commands you should familiarize yourself with first are `ls` and  `cd`.

- **ls**: list files in directory
- **cd**: change directory 

{{< asciinema
    cast="/casts/cdls.cast"
    loop=true
    autoplay=true
    cols=90
    rows=3
    speed=2 >}}

To view a more comprehensive description of each of these commands, execute `man ls` or `man cd` to view the "man-page" for either. This goes for any of the following commands introduced as well.

## Manipulating the File-Tree

The file-tree can be directly manipulated in the shell. A few commands you should familiarize yourself with are `touch`, `mkdir`, `rm`, `cp`, and `mv`. 

{{< tabs >}}
{{% tab "Create a File" %}}
Create a file with a given path: `touch /path/to/file.txt`.
{{% /tab %}}
{{% tab "Create a Directory" %}}
Create a directory/folder with a given path: `mkdir /path/to/folder/`.
{{% /tab %}}
{{% tab "Remove a File" %}}
Remove a file with a given path: `rm /path/to/file`.
{{% /tab %}}
{{% tab "Copy a File" %}}
Copy a file from one path to another: `cp /path/to/file /path/to/copy`.
{{% /tab %}}
{{% tab "Move a File" %}}
Move a file from one path to another: `mv /path/to/file /path/to/new-file`. 

This can also be used to rename an existing file!
{{% /tab %}}
{{< /tabs >}}

## Editing Files in the Terminal
