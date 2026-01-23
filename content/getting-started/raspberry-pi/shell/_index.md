---
title: "Terminal / Shell"
weight: 4
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
# bookIcon: ''
bookCollapseSection: true
---

# Terminal / Shell

The **shell** is a robust communication interface between you and your operating system. This section is concerned with the basics of navigating and manipulating the file-tree accessible via the Bourne Again Shell - **BA$H**.

## Navigation in the Shell

The prompt is the textual input into the shell. This is where you'll execute commands and view their output, usually designated by the **$**. The text to the left of the prompt contains details about your session such as user, hostname, and current working directory or path; i.e. `user@hostname ~/current/working/directory $ _`. 

In the shell, the user navigates through a **tree-structure** of directories, with the "root" directory as the lowest (bottom) member. You can navigate up and down this tree by executing commands in the shell. Below is an example of this tree structure.

```
/ <-- root 
├── home/
│   ├── staff/
│   │   ├── my-cool-directory/
│   │   └── my-cool-file.txt
│   └── user42/
└── ...
```

To move through the tree-structure, the commands you should familiarize yourself with first are `ls` and  `cd`.

- **ls**: list files in directory
- **cd**: change directory 

{{< asciinema
    cast="/casts/cdls.cast"
    loop=true
    autoplay=true
    cols=90
    rows=3
    speed=2 >}}

> [!Tip]
> The paths `./` and `../` exist in all non-root directories and refer to the **current** and **previous** directories, respectively. You can navigate to them like any other directory.

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
{{% tab "Remove a File / Directory" %}}
Remove a file with a given path: `rm /path/to/file`. To remove a directory, add the `-r` option to the command: `rm -r /path/to/folder`.
{{% /tab %}}
{{% tab "Copy a File / Directory" %}}
Copy a file from one path to another: `cp /path/to/file /path/to/file-copy`. To copy a directory, add the `-r` option to the command: `cp /path/to/folder/ /path/to/folder-copy/`
{{% /tab %}}
{{% tab "Move a File / Directory" %}}
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

### Nano

**[GNU Nano](https://www.nano-editor.org)** is a terminal-based text editor for Unix based systems. Nano comes preinstalled on most Linux operating systems, but can be installed manually by executing the following in your shell.

```
sudo apt install -y nano
```

Nano, much like (neo)Vi(m), has it's own learning curve. However, Nano tends to be more beginner friendly while lacking much of the customizability of the vim-like editors.

Nano can be opened by executing `nano` in your shell. Start typing to write into the buffer, and press control-O (`^O`) to save. To exit the editor, press control-X (`^X`).

{{< asciinema
    cast="/casts/nano.cast"
    loop=true
    autoplay=true
    cols=90
    rows=21
    speed=2 >}}


## Multiplexing

**Terminal multiplexing** is an extremely powerful tool to split your terminal into multiple *pseudo*-terminal sessions. **[Tmux](https://github.com/tmux/tmux/wiki)** is the recommended terminal multiplexer for this class. **Tmux** offers persistence across local and remote sessions, allowing you to resume working after a log-out.

First make sure **tmux** is installed by executing `tmux -V` in your shell. If not, install **tmux** by executing the following:

```
sudo apt install -y tmux
```

Starting a new **tmux** session is done by executing `tmux new` in your terminal, or `tmux new -s my-new-session` to specify a name.

### Commands 

You can **split** your terminal window into sub-sessions by pressing the default command leader (tmux prefix) control-B (`^B`), followed by either double-quotes to split the window **horizontally** (`"`), or the percent sign (`%`) to split the window **vertically**.

> [!Tip]
> The default command leader `^B` is used in tandem with a number of other key-combinations. You can change this by editing your **tmux** configuration file at `~/.config/tmux/tmux.conf`. For example, the following command will change the command leader to `^<Space>`.
> ```
> cat > ~/.config/tmux/tmux.conf << EOF
> unbind-key C-b
> set-option -g prefix C-Space
> bind-key C-Space send-prefix
> EOF
> ```


{{< tabs >}}
{{% tab "Panes" %}}
Tmux prefix + `"`. This splits your terminal **window** horizontally into two **panes**. Likewise, you can split your **window** vertically, by executing tmux prefix + `%`.

You can then **kill** a **pane** by executing tmux prefix + `x`.

You can **change** focus by using the tmux prefix in combination with the arrow keys. For example, executing tmux prefix + `↑` swaps focus to the **pane** above the current in focus.

To make your life easier, you can enable **mouse support** swapping focus by executing tmux prefix + `:` to open the tmux **command line**, and then entering `set -g mouse on`. You can also add this to your tmux **config** to enable it across all sessions.
{{% /tab %}}
{{% tab "Windows" %}}
Tmux prefix + `c`. As well as having multiple **panes**, you can also have multiple **windows** within a single tmux session. Tmux prefix + `<window number>`. For example, to change focus to **window** `2`, execute tmux prefix + `2`.
{{% /tab %}}
{{% tab "Attaching and Detaching" %}}
Executing tmux prefix + `d` will detach you from the current tmux session. To reattach to the last session execute `tmux a` in your terminal.
{{% /tab %}}
{{< /tabs >}}

## Becoming a Terminal Power-User

When combined with a robust terminal-based text editor, **tmux** becomes an invaluable asset to any engineer working in the terminal. Oftentimes, the customizability offered by **tmux** and any of the **(neo)Vi(m)** lends to a more tailor-made experience than popular IDEs such as VSCode or Sublime Text. For this class the lightweight, persistent sessions across ssh log-ins/out are going to pay dividends in the long run.

{{< asciinema
    cast="/casts/tmux.cast"
    loop=true
    autoplay=true
    speed=2 >}}

sdfs
