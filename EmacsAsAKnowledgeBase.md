---
layout: default
---

# Emacs as a Knowledge Base

One of the great attractions of Emacs is that you can do everything with it. It’s the one app that you don’t have to switch out of. You can keep your context for an entire computing session, all the while checking mail, taking notes, writing code, running code, planning projects, and even [surfing the web](https://www.gnu.org/software/emacs/manual/html_mono/eww.html).

So when a shiny new app comes out and starts to get everyone’s attention, it’s understandable that you might get concerned because “Maybe Emacs won’t be able to replicate this!” I bet some people thought that when [Roam Research](https://roamresearch.com/) hit the scene in early 2020 and set the Personal Knowledge Management space on fire 🔥

Nobody was doing two-way linking and for some reason that seemed like an Industrial Revolution-scale innovation. Things have cooled down since then, but one thing left standing in the rubble is the reassurance that, in fact, Emacs Can Do That™️.

In this post we will get familiar with an Emacs extension built for note taking and knowledge management. The key features of this package, [Org Roam](https://www.orgroam.com/), are that notes are two way-linked, ie. when viewing one note, you can see a list of all of the notes that refer to it.

This package builds off of the powerful Org Mode, so you get support for everything in org-mode **plus** a graph-based knowledge management system. Let’s dive in.

# Install

We will take a look at installation using the [straight.el](https://github.com/radian-software/straight.el) package manager. For MELPA-based installation take a look at the package’s [Online Manual](https://www.orgroam.com/manual.html#Installing-from-MELPA).

Install org-roam with a simple `use-package` line:

```lisp
(use-package org-roam
  :straight t
  :after (org))
```

You’ll want to tell it to wait until you’ve loaded `org-mode`, hence the `:after` clause.

# Configure

To get `org-roam` up and running productively, you only need a minimal configuration. Update your `use-package` block with the following.

```lisp
(use-package org-roam
  :straight t
  :after (org)
  :config
  (setq org-roam-directory (file-truename "~/org/org-roam"))
  (org-roam-db-autosync-mode))
```

The `(setq org-roam-directory ...)` command tells the package where to look for existing note files, and where to put new note files.

I recommend creating a new directory for this, that will only be used by `org-roam`. For example, I have an existing `~/org` directory which I use for regular Org files. So underneath this directory I put `~/org/org-roam`, and use that as my `org-roam-directory` variable.

If you are not already a user of `org-mode`, you can just create a new `org-roam` directory in your home directory.

```lisp
$ mkdir ~/org-roam
```

The `(org-roam-db-autosync-mode)` statement tells org-roam to automatically update its database with new notes and new links. This means that when you create a new note in org-roam, it will automatically become searchable, and connections will be created for any existing notes that a new note references.

# Usage

Note taking with org roam is most efficient when you have proper keybindings set up. You want to be able to record a note as soon as you think of something. Similarly, you want to be able to refer to a topic that you’ve already noted down without having to click around or type in a verbose command.

Fortunately, the API for org roam makes both of these actions super easy. They are both executed from the same command! If you run the command `org-roam-node-find` and type in the name of a note that already exists, selecting that note will bring you to the existing note! And if you type in a name that does not already refer to a note, selecting that name will create a new note in the org roam database, and bring you to a new buffer for that empty note!

So with one keybinding, we can have the most useful org roam command just a couple keypresses away at all times.

I recommend binding the `org-roam-node-find` command to something intuitive. Since this command will probably be associated in your brain with the terms “Roam”, “Org”, “Node”, “Find”, I recommend some short combination of the letters r, o, n, and f.

My binding for this command is `SPC n f`. I think of `n` as prefixing different “Note-taking” commands, and `f` as meaning “Find”. So typing my leader key — `SPC` — followed by `n f` means “execute the ‘Find Note’” command.

In other Emacs configuration frameworks, like Doom Emacs, this command is by default bound under `SPC n r f`, and the extra `r` refers to “Roam”.

## The Org-Roam Buffer

The two-way linking system in org roam means that when you are looking at one note, you can see a list of all the notes that refer to it.

Bring up the Org Roam buffer with the command `org-roam-buffer-toggle`. I have this bound to `, t t` whenever I am inside org-roam-mode.

Let’s see what that looks like in an example.

Here on the left I am looking at a note called “archaea,” which is about the domain of single-celled organisms by the same name. I want to see where I have written about archaea, and so I type `, t t` to bring up the Org Roam buffer, which appears on the right.

![Screen Shot 2023-01-25 at 9.40.06 PM.png](Emacs%20as%20a%20Knowledge%20Base%20fe82c78633ab4a7699e941f1c0d87fbb/Screen_Shot_2023-01-25_at_9.40.06_PM.png)

This shows me that I have referred to “archaea” one other time, in a note about the scientific paper titled “A Programmable Dual-RNA-Guided DNA Endonuclease in Adaptive Bacterial Immunity.” Apparently I was doing some research on CRISPR at some point!

---

With these setup instructions and these basic commands, you can go a long way with knowledge management in Emacs. This setup supports an easy way to create new notes from anywhere in Emacs, as well as view everything that refers to a note you are working on or viewing.

There is a lot more to the Org Roam package, and if this sparks your interest I encourage you to explore what’s available in the package’s Online Manual.

Furthermore, there is a really cool companion package called [Org Roam UI](https://github.com/org-roam/org-roam-ui) that will show you an interactive version of your knowledge graph in the browser!

Here’s what my current graph looks like 🙂

![Screen Shot 2023-01-25 at 9.48.40 PM.png](Emacs%20as%20a%20Knowledge%20Base%20fe82c78633ab4a7699e941f1c0d87fbb/Screen_Shot_2023-01-25_at_9.48.40_PM.png)
