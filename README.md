# ![logo](etc/telega-logo64.png) telega.el [![Telegram chat](https://img.shields.io/badge/chat-%40emacs__telega-%2335ADE1)](https://t.me/emacs_telega) [![TDLib](https://img.shields.io/badge/tdlib-v1.5.0-%2335ADE1)](https://github.com/tdlib/td)

`telega.el` is full featured unofficial client for
[Telegram](https://telegram.org "Telegram") platform for [GNU
Emacs](https://www.gnu.org/software/emacs/ "GNU Emacs").

`telega.el` is actively developed, for this reason, some features are
not implemented, or they are present just as skeleton for future
implementation. However, the core parts are mature enough so that it
is possible to use `telega.el` for basic chat.

# Features

In some random order:

- [x] Listing chats, reordering chats according to internal Telegram
      order
- [x] Getting info about users, groups and supergroups
- [x] Joining chats by invitation link `M-x telega-chat-join-by-link RET`
- [x] Fetching chat history, sending messages, replies, edits, deleting
- [x] Resend failed messages
- [x] Creating new groups, upgrading basicgroup to supergroup
- [x] Forwarding messages
- [x] D-Bus notifications on incoming messages in chats with enabled
      notifications
- [x] VoIP calls, including D-Bus call notification
- [x] Downloading files from the cloud
- [x] Uploading files/media (also pasting images from clipboard) to the cloud
- [x] Display chat actions, such as "@user is typing..."
- [x] Display/Update chat's draft message
- [x] Company backends for emoji (`:<emoji>` syntax), usernames,
      hashtags, bot commands completions
- [x] Secret chats
- [x] Online global searching chats/contacts/messages
- [X] Avatars, Photos, Stickers
- [x] Animated GIF, via ffplay
- [x] Special messages such as location/live location, voice/video
      messages, contacts, etc (PARTLY)
- [x] Poll messages, Poll creation
- [x] Handling local links, such as "tg:" or "https://t.me/xxx" (PARTLY)
- [x] ReplyMarkup (inline keyboard buttons) in the messages (PARTLY)
- [x] InstantView for web pages
- [x] Blocking/Unblocking users, listing blocked users
- [x] Inline bots requests, via "@bot query<TAB>"
- [ ] Searching for messages in chat
- [ ] Shared media
- [ ] Traffic control, see [telega#62](https://github.com/zevlg/telega.el/issues/62)
- [ ] [TranslationsPlatform](https://translations.telegram.org) support
- [ ] Delayed messages, see [TDLib#695](https://github.com/tdlib/td/issues/695)
- [ ] Cloud themes, see [TDLib#691](https://github.com/tdlib/td/issues/691)

## Unique features

- [x] Expressive `ibuffer`-like chats filtering
- [x] Custom order for chats (some chats on top, some chats on bottom, etc)
- [x] Labeled chats, you can assign custom label to chat
- [x] Client side messages filtering
- [x] Get diff for the message edits, see [screenshot](https://zevlg.github.io/telega/screen-13.png)

# Screenshots

Root buffer screen:

![screen14](https://zevlg.github.io/telega/screen-14.png)

Chat in [@emacs_en](https://t.me/emacs_en "emacs_en") group:

![screen11](https://zevlg.github.io/telega/screen-11.png)

Attaching funny cat sticker:

![screen12](https://zevlg.github.io/telega/screen-12.png)

# Installation

`telega.el` depends on `visual-fill-column` package.  This dependency
automatically installs if you install telega from MELPA.  Otherwise
you need to install this package by hand.

`telega.el` is built on top of the official library provided by
Telegram [TDLib 1.5.0](https://core.telegram.org/tdlib "tdlib"). Most
of the distributions do not package this libary, so it has to be build
from source.

## Dependences

   * `GNU Emacs` (at least 26.1 is required)
   * `GNU make`
   * `GNU gperf`
   * `CMake`

`make` is found in most of the modern machines. The other packages can
be download with the system package manager (such as `apt` for
Debian-based distributions, `dnf` for Fedora or `pacman` for
Arch-based).

### MacOS users

1. If you are using [Emacs For Mac OS X](https://emacsformacosx.com/),
   or you installed Emacs by running `brew cask install emacs`, your
   Emacs lacks svg support, so you cannot use telega. Please switch to
   [emacs-plus](https://github.com/d12frosted/homebrew-emacs-plus)

2. If you are using
   [Emacs-mac](https://bitbucket.org/mituharu/emacs-mac/), or you
   installed Emacs by running `brew install emacs-mac` or `brew cask
   install emacs-mac`, your Emacs has bug dealing with complex svg,
   which leads to Emacs hangup.  Compiling Emacs with rsvg support by running
   `brew install emacs-mac --with-rsvg` will fix the hangup problem.

   Telega cannot display stickers correctly with emacs-mac, even when emacs-mac
   is compiled with rsvg support.  If you want stickers, please consider
   switching to emacs-plus.

3. [emacs-plus](https://github.com/d12frosted/homebrew-emacs-plus) is
   the best choice to run telega.

### Linux users

`telega.el` requires at least GNU Emacs 26.1 with `imagemagick` and
`svg` support.

### Building tdlib

[TDLib](https://core.telegram.org/tdlib "tdlib") is the library for
building Telegram clients. It requires a large amount of memory to be
built.  Make sure you are using 1.5.0 TDLib version.

On MacOS you can install pre-build `tdlib` package using homebrew from
[brew.sh](https://brew.sh).  Just run:
```console
$ brew install tdlib
```

On Linux you need build `tdlib` from sources.

To get the source:
```console
$ git clone https://github.com/tdlib/td.git
```

Move into the folder with `cd td`

Prepare a folder for building the library:
```console
$ mkdir build && cd build && cmake ../
```

Build the sources:
```console
$ make -jN
```

with `N` number of cores that should be used for the compilation (the optimal
value is the number of physical cores on the machine).

Finally, to install the library system-wide:
```console
$ sudo make install
```

It will install headers to `/usr/local/include` and library itself
into `/usr/local/lib`.  These paths are hardcoded in `telega.el`.

### Building libtgvoip

VoIP support in `telega.el` is optional, if you don't need VoIP, just
ignore this section.

[libtgvoip](https://github.com/zevlg/libtgvoip "libtgvoip") is the
VoIP library for telegram clients.  This is the fork from
[original](https://github.com/grishka/libtgvoip) library with patches
needed by `telega.el`.

To get the source:
```console
$ git clone https://github.com/zevlg/libtgvoip.git
```

Move into the folder with `cd libtgvoip`

Prepare a folder for building the library:
```console
$ autoreconf --force --install && ./configure && make
```

Install the library system-wide:
```console
$ sudo make install
```

It will also install headers to `/usr/local/include` and library into
`/usr/local/lib`.

## Installing telega.el from MELPA

`telega.el` already in [MELPA](https://melpa.org), so you can install
it from there as usual package.  This method is preferable, because it
will handle all dependencies.  After installing `telega.el` from MELPA
you can skip to [Fire up `telega.el`](#fire-up-telegael) section.

Or you could use git repository or this melpa-style recipe:

```lisp
(:fetcher github
 :repo "zevlg/telega.el"
 :branch "master"
 :files (:defaults "etc" "server" "Makefile"))
```

## Building telega-server

Now that the `TDLib` library is set-up, it is time to install
`telega.el`. The first step consists in building `telega-server`,
which is a C interface to the `TDLib`, or just let `telega` ask you at
the first start and do the job.

To get the source:

```console
$ git clone https://github.com/zevlg/telega.el
```

Moving into the folder with `cd telega.el`, it is possible to build
the `telega-server` executable and move into the `$HOME/.telega` with:
```console
$ make && make install && make test
```

If you want VoIP support in `telega.el` and `libtgvoip` is installed,
then use this instead:
```console
$ make WITH_VOIP=t && make WITH_VOIP=t install && make WITH_VOIP=t test
```

This command does not require superuser privileges.

Start with `M-x telega RET` and follow instructions

## Installing telega.el

Now it is time to install `telega.el` on GNU Emacs.

This can be done with `use-package`:

```elisp
(use-package telega
  :load-path  "~/telega.el"
  :commands (telega)
  :defer t)
```

Or with:
```elisp
(add-to-list 'load-path "~/telega.el")
(require 'telega)
```

The code should be put in the configuration file for Emacs, which
usually is `init.el`, or `emacs.el`.

## Fire up `telega.el`

`telega.el` can now be started with `M-x telega RET`. The first time
it ask for the phone number to login to the Telegram network.

# Minor modes

## telega-mode-line-mode

Minor mode to display telega status in modeline.  To enable it use

```elisp
(telega-mode-line-mode 1)
```

## telega-notifications-mode

`telega.el` ships with support for D-Bus notifications, but they are
disabled by default.  To enable notifications add next code to your
`init.el`:

```elisp
(telega-notifications-mode 1)
```

# Enabling emoji completions in chat buffer

Emoji completions with `:<EMOJI-NAME>:` syntax, uses nice
[company-mode](http://company-mode.github.io).  It provides
`telega-company-emoji` company backend.  So you need to add it to
`company-backends`, maybe along with other backends in
`telega-chat-mode-hook`, for example:

```elisp
(add-hook 'telega-chat-mode-hook
          (lambda ()
            (set (make-local-variable 'company-backends)
                 (append '(telega-company-emoji
                           telega-company-username
                           telega-company-hashtag)
                         (when (telega-chat-bot-p telega-chatbuf--chat)
                           '(telega-company-botcmd))))
            (company-mode 1)))
```

# Configuring client side messages filtering

In official telegram clients all messages in group chats are displayed
even if message has been sent by blocked user.  `telega.el` has client
side message filtering feature implemented.  Ignoring messages can be
done via installing special functions into
`telega-chat-pre-message-hook` which could mark message as ignored,
for example, to ignore messages from particular user with id=12345 you
could add next code:

```elisp
(defun my-telega-ignore-12345-user (msg &rest notused)
  (when (= (plist-get msg :sender_user_id) 12345)
    (telega-msg-ignore msg)))

(add-hook 'telega-chat-pre-message-hook 'my-telega-ignore-12345-user)
```

Or to ignore messages from blocked users, just add:

```elisp
(add-hook 'telega-chat-pre-message-hook 'telega-msg-ignore-blocked-sender)
```

To view recent messages that has been ignored use
`M-x telega-ignored-messages RET` command.

# How to contribute

Join our [Telegram group](https://t.me/emacs_telega "Telegram group")
to discuss the development of `telega.el`.

Submitting [issues](https://github.com/zevlg/telega.el/issues) is
exceptionally helpful.

# License

`telega.el` is licensed under GNU GPL version 3.

# FAQ

**Q**: I got the error after `M-x telega RET`

```console

Status: telega-server: exited abnormally with code 127
/home/user/.telega/telega-server: error while loading shared libraries:
libtdjson.so: cannot open shared object file: No such file or directory
```

**A**: Add `/usr/local/lib` into library loading path using next:

```console
# echo "/usr/local/lib" > /etc/ld.so.conf.d/usr_local_lib.conf
# ldconfig
```

**Q**: I've got the error installing telega

```console
Cannot open load file: No such file or directory, visual-fill-column
```

**A**: `telega.el` depends on `visual-fill-column` package, please
install it first.  This package is available from
[MELPA](https://melpa.org)

**Q**: I've got error, while running telega

```elisp
(error "Invalid image type ‘svg’")
```

and/or

```elisp
(error "Invalid image type ‘imagemagick’")
```

**A**: `telega.el` requires Emacs with SVG and ImageMagick support.
SVG support in Emacs is done using `librsvg` library.  As to
imagemagick, you will need `libmagickcore-dev` and `libmagickwand-dev`
packages installed.  But unfortunately Emacs recently disabled
imagemagick support by default (see
https://lists.gnu.org/r/emacs-devel/2018-12/msg00036.html).  So you
need to compile Emacs by hand, specifying `--with-imagemagick` flag to
`./configure` script.

Telega won't depend on `imagemagick` in future, since required image
features has been added to newer Emacs, see
https://lists.gnu.org/r/emacs-devel/2019-06/msg00242.html

**Q**: I'm from Russia, does `telega.el` has proxy support?

**A**: Yes, use `telega-proxies` custom variable, for example:

```elisp
(setq telega-proxies
      (list
       '(:server "1.2.3.4" :port 8080 :enable :false
                 :type (:@type "proxyTypeSocks5"
                               :username "rkn" :password "jopa"))
       '(:server "2.3.4.5" :port 8088 :enable t
                 :type (:@type "proxyTypeSocks5"
                               :username "rkn" :password "jopa"))
       ))
```
See `C-h v telega-proxies RET` for full range of proxy types.

**Q**: Stickers are not shown.

**A**: Make sure you have `imagemagick` support and please install `webp` package

**Q**: No glyphs for some unicode characters.

**A**: Please install `fonts-symbola` package

**Q**: There is some formatting issues when some unicode characters are used.  Is there way to fix it up?

**A**: Yes, partly.  If character has full width of multiple ordinary chars you can tweak `char-width-table`.  Add code like this to your init.el:

```elisp
(setq telega-symbol-unread "🄌")

(defun my-telega-load ()
  ;; 🄌 occupies two full chars, but (string-width "🄌") returns 1
  ;; so we install custom widths to `char-width-table'
  (telega-symbol-set-width telega-symbol-unread 2)

  ;; ... other code
  )

(add-hook 'telega-load-hook 'my-telega-load)
```

There is also `telega-symbol-widths` custom variable, you might want
to modify it.

**Q**: Is there erc-like chats tracking functionality?

**A**: Yes, set `telega-use-tracking` to non-nil.

Take into account that telega tracks only opened chats with enabled
notifications.

**Q**: Is it possible to use `telega.el` in tty-only Emacs (aka
emacs-nox)?

**A**: Yes, set `telega-use-images` to `nil`, before start.
