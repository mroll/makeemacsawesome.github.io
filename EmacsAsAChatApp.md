---
layout: default
---

# Emacs as a Chat App

One thing you won’t find in modern IDEs is an instant messenger application.[1] It might seem weird to be chatting with your friends and colleagues in the same place as you write your code. But Emacs is a 50 year old piece of technology, and doesn’t care for whatever inhibitions are currently in vogue 🙂

In this post we’ll cover two ways you can do instant messaging with Emacs.

# Slack

The first Emacs chat client we will cover is for [Slack](https://slack.com/), a popular instant messaging platform used ubiquitously in tech companies.

To chat with Slack in Emacs, we will be using [this](https://github.com/yuya373/emacs-slack) third-party package.

## Install

To install the package, add the following snippet to your init file.

```lisp
(use-package slack
  :straight t
  :init
  (setq slack-buffer-emojify t)
  (setq slack-prefer-current-team t))
```
_(You may have to adjust this if you are not using the straight.el package manager)_

Evaluate the snippet, reload the file, or reload Emacs for it to take effect.

## Authenticate

In order to authenticate to our Slack workspace, we need to grab both a cookie and a token from our browser.

The package readme has instructions [here](https://github.com/yuya373/emacs-slack#how-to-get-token-and-cookie), but I found them a bit hard to follow so I’ll include a more detailed walkthrough in this post.

1. Log into your slack workspace in your browser.
2. Once logged in, right click anywhere and click **Inspect** to open the developer tools.
3. Open the Console tab and run this code: `window.prompt("your api token is: ", TS.boot_data.api_token)`. Grab the token that gets displayed and paste it somewhere in a scratch pad.
4. Switch to the Application tab and expand the Cookies menu in the left side bar. Find the cookie named `d` and copy its value. Copy the raw value, do not click **Show URL decoded**.
5. Add the following code to your `use-package` snippet in your init file.


    ```lisp
    (use-package slack
      :straight t
      :commands (slack-start)
      :init
      (setq slack-buffer-emojify t)
      (setq slack-prefer-current-team t)
      :config

      (slack-register-team
       :name "your-org-name"
       :default t
       :token "<token>"
       :cookie "<cookie>"
       :subscribed-channels '(general)
       :full-and-display-names t))
    ```


1. Reload or evaluate for the slack extension to make the connection to your workspace.

## Usage

To start up a Slack-in-Emacs session, run the command `M-x slack-start`. This will establish a connection.

To open up a channel, run the command `M-x slack-channel-select`, and choose one of the channels. A buffer will open with a message prompt where you can type and send messages, just like if you were using the official Slack client!

![Screen Shot 2023-01-22 at 2.41.27 PM.png](Emacs%20as%20a%20Chat%20App%20bbffc54e18954000b06676b3890a1fde/Screen_Shot_2023-01-22_at_2.41.27_PM.png)

Check out the project readme for more info, like how to encrypt and store your authentication tokens, more commands that are available, and some convenient keybindings.

# IRC

If Slack is too corporate for your liking, you might prefer [IRC](https://en.wikipedia.org/wiki/Internet_Relay_Chat), a 30+ year old, open source, decentralized protocol for communicating online. Emacs comes pre-loaded with a powerful IRC client called [ERC](https://www.gnu.org/software/emacs/manual/html_mono/erc.html#Getting-Started).

Since it comes built-in, there is no installation required.

## Usage

Fire up an ERC session right away by running `M-x erc-tls`. You can also run the more basic `M-x erc` command, but that will default to an unencrypted connection.

Emacs will prompt you to enter a server, a port, a username, and a password. There will be sensible defaults for the server and port. As of this moment, [irc.libera.chat](http://irc.libera.chat) is the default server, which is home to the official `#erc` channel as well as other Free Software Foundation communities.

Other popular servers include Freenode, EFnet, Snoonet, and [more](https://www.mirc.com/servers.html).

Upon connecting to a server, ERC will open up a channel buffer which gives some basic info from the server, as well as shows some details about your connection and username lookup.

![Screen Shot 2023-01-22 at 2.56.10 PM.png](Emacs%20as%20a%20Chat%20App%20bbffc54e18954000b06676b3890a1fde/Screen_Shot_2023-01-22_at_2.56.10_PM.png)

To chat with people, you will first have to join a channel. Some popular channels on Freenode are `#linux`, `#archlinux`, and `#python`.

To join a channel, type `/join #channel`. For example, to join the `#python` channel, you would type `/join #python` and hit Enter. A new ERC buffer will be created for the newly joined channel, where you can type message at the prompt, and see other users’ messages in the buffer.

To leave a channel, simply type `/leave` in the prompt for the channel and hit Enter.

## Configuration

If you are using IRC frequently, it won’t be efficient to have to manually join your favorite networks and channels every time you log in. Fortunately Emacs supports some configuration that will automatically connect and join channels on login if you have things properly set up.

To quickly join a given network, you can define a command like the following in your init file.

```lisp
(global-set-key "\C-cef" (lambda () (interactive)
                           (erc :server "irc.libera.chat" :port "6667"
                                :nick "MYNICK")))
```

This will bind a function to the `C-c e f` key chord that joins the [irc.libera.chat](http://irc.libera.chat) server when executed. Just substitute your username for `MYNICK` in the snippet.

For automatically joining channels on a given server, you can configure the variable `erc-autojoin-channels-alist`. This is a dictionary with a mapping from server name to a list of channels to automatically join when a connection is made to that server.

For example, if you want ERC to join the channels `#emacs` and `#erc` whenever you connect to the [irc.libera.chat](http://irc.libera.chat) network, you would put the following in your init file.

```lisp
(setq erc-autojoin-channels-alist
      '(("Libera.Chat" "#emacs" "#erc")))
```

# Next Steps

These are not the only two instant messaging applications for Emacs. For example, you can also [use Emacs](https://github.com/alphapapa/ement.el) to chat over the open source [Matrix](https://matrix.org/) protocol.

Plus, the configuration examples I gave in this introductory post only scratch the surface. It’s possible to do so much more, like [store your server credentials in encrypted files](https://www.gnu.org/software/emacs/manual/html_mono/auth.html) for better security.

---

[1] With the exception of [Zed](https://zed.dev/), a very cool newcomer on the scene.
