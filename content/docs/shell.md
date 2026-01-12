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

# Terminal / Shell

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

## Text Editors in the Shell

There are a number of terminal-based text editors, each with their own grade of learning curve. Proficiency with at least one of these is recommended. MASLAB recommends familiarizing yourself with either the [(neo)Vi(m)](https://www.vim.org) or [Nano](https://www.nano-editor.org) text editors.

### (neo)Vi(m)

**Vi** is an extremely powerful terminal-based text editor for the Unix operating system. **Vim** and **Neovim** are variants of the **Vi** editor packaged with a library of additional features.

If not already installed, they can be added by the following in your shell.

```
sudo apt install -y neovim vim vi 
```

> [!WARNING]
> You should memorize/write-down the following information if unfamiliar with the **(neo)Vi(m)** editors.

#### Modes

**(neo)Vi(m)** is interacted through a number of *modes*. The two you will familiarize yourself with most will be the **i**nsert and **v**isual modes. These modes are **entered** by pressing `i` or `v` for the **i**nsert and **v**isual modes, respectively. You can **exit** a mode at any time by pressing the `esc` key.

In the insert mode, text can be written to or removed from the active buffer (the current window). The cursor can be moved through the use of the arrow keys.

In the visual mode, text can be highlighted, copied, pasted, deleted, and more. The cursor is moved in the same manner as the insert mode, or through the use of the `hjkl` keys. To copy text, first highlight it and press the `y` key, then paste it by pressing `p`.

#### Commands

The **(neo)Vi(m)** command line is opened by pressed `:` on the keyboard. Here, you can enter a palette of commands, a few of which are listed below.

{{< tabs >}}
{{% tab "Writing Files" %}}
To write the buffer to a new file, use `:w file-name.txt`.
{{% /tab %}}
{{% tab "Leaving the Editor" %}}
To leave the editor, use `:q`. You can forcibly leave the editor by executing `:q!`.
{{% /tab %}}
{{% tab "Searching" %}}
A pattern can be searched for by executing `:/pattern`, where pattern is the desired term to be sought after.
{{% /tab %}}
{{% tab "Line Numbers" %}}
Line numbers can be shown by executing `:set number`. Among others, you can enable spellcheck by executing `set spell` .
{{% /tab %}}
{{% tab "Combining Commands" %}}
You can combine commands into a single statement by leading them on-top of one another. For example, to both save and quit `:wq`.
{{% /tab %}}
{{< /tabs >}}

#### Opening the Editor

To use **(neo)Vi(m)**, execute the name of the editor to be used (`vi / vim / nvim`).

{{< asciinema
    cast="/casts/vi.cast"
    loop=true
    autoplay=true
    cols=90
    rows=21
    speed=2 >}}

To familiarize yourself, play around with the editor or head to [OpenVim](https://openvim.com), a great tool to practice your text-editing in **(neo)Vi(m)**.


