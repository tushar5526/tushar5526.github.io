---
title: "getting back ctrl over ctrl+alt+arrow in gnome"
date: 2018-07-01T10:47:28+02:00
---

## the task

Deactivating Gnome's keyboard shortcuts for ctrl+alt+left / ctrl+alt+right to make those combinations available for another application.

In my case I tried to use these shortcuts in Jetbrain's PyCharm for code navigation.

## the obvious (and wrong) way

- Go to
  - *Settings*
  - *Devices*
  - *Keyboard*
  - *Keyboard Shortcuts*
- Change or reset the values for the shortcuts

Although both shortcuts are obviously blocked by Gnome, they are not listed.

## one way

```bash
$ gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-right []
$ gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-left []
```

## another way

```bash
$ sudo dnf install dconf-editor
```

- open dconf-editor
- navigate to org / gnome / desktop / wm / keybindings
- change the keybindings

## the explanation

I think those missing shortcuts on the settings page is a [bug](https://bugzilla.redhat.com/show_bug.cgi?id=1597280) (auto-closed as Fedora 28 is EOL), or at very least a usability problem. Speaking of usability problems, on the settings page you can only see one shortcut per action, even when there are two or more defined.

Luckily, as shown above, there are ways to counter those problems.

In order to save preferences both for the system and for applications, Gnome uses a configuration system called **gconf**. I think of it as simple key-value database.

There are several ways to access this database, amongst there are:

- **gsettings**
- **dconf-editor** (needs installation)

### gsettings

**gsettings** is a high level command line interface to the above mentioned **dconf** configuration system.

The simplified syntax is:

```bash
$ gsettings COMMAND SCHEMA KEY [VALUE]
```

Let's have another look at one of the commands from above:

```bash
$ gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-right []
```

- **set** is the COMMAND; another command would be **get** to retrieve a value
- **org.gnome.desktop.wm.keybindings** is the SCHEMA, think of a group or a box in order to make it easier to find many key/value pairs
- **switch-to-workspace-right** is the KEY, in this case the action which follows a specific keystroke
- **[]** is the value; in this case it is set to an empty value

You can explore the many possibilities of **gsettings** by either having a look at the manpage
or by entering one of the following self explaining commands:

```bash
$ gsettings list-keys org.gnome.desktop.wm.keybindings
$ gsettings list-schemas
```

### dconf-editor

**dconf-editor** is a graphical user interface to the **dconf** database.

It is not installed by default, so in order to use it, you have to enter...

```bash
$ sudo dnf install dconf-editor
```

There is not much to add here - you can set and get all the key/value pairs just as with **gsettings**, but this time there is no command line involved.

## pitfalls

All settings are saved on a per user basis - so do not use **gsettings** or **dconf-editor** as root, when you plan to change your user settings.

## used versions

```bash
$ gnome-shell --version
GNOME Shell 3.28.2

$ cat /etc/fedora-release 
Fedora release 28 (Twenty Eight)
```

## references
- [ubuntuusers.de - dconf](https://wiki.ubuntuusers.de/GNOME_Konfiguration/dconf/) (German)
- [chapter 9 of Redhat's Desktop Migration and Administration Guide](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/desktop_migration_and_administration_guide/user-and-system-settings)
- [What is dconf, what is its function, and how do I use it?](https://askubuntu.com/questions/22313/what-is-dconf-what-is-its-function-and-how-do-i-use-it)
