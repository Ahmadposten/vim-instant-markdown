vim-instant-markdown
====================
Want to instantly preview finnicky markdown files, but don't want to leave your favorite editor, or have to do it in some crappy browser textarea? **vim-instant-markdown** is your friend! When you open a markdown file in vim, a browser window will open which shows the compiled markdown in real-time, and closes once you close the file in vim.



This is a fork from [suan/vim-instant-markdown](https://github.com/suan/vim-instant-markdown)
due to lack of activity in the project. The original version did not support multiple sessions at the same time and allowed to only open a single markdown document at a time. This fork supports multiple sessions for each open buffer containing a markdown.


As a bonus, [github-flavored-markdown][gfm] is supported, and styles used while previewing are the same as those github uses!

[![Screenshot][ss]][ssbig]


Installation
------------
You first need to have node.js with npm installed. Then:

- `[sudo] npm -g install instant-markdown-d`
- If you're on Linux, ensure the following packages are installed:
  - `xdg-utils`
  - `curl`
  - `nodejs-legacy` (for Debian-based systems)
- If you're on Windows, you will need into install [cURL][curl] and put it on your `%PATH%`.
- Copy the `after/ftplugin/markdown/instant-markdown.vim` file from this repo into your `~/.vim/after/ftplugin/markdown/` (creating directories as necessary), or follow your vim package manager's instructions.
- Ensure you have the line `filetype plugin on` in your `.vimrc`
- Open a markdown file in vim and enjoy!



## How does it work
A nodejs server should be installed, this server implements web sockets using socket.io. When you start a vim open a vim buffer holding a `*.md ` file it starts the daemon on port 8090 which in turn opens a browser tab for you and connects to it using sockets. Every change to th buffer reflects instantly in the browser session and every open buffer is associated with a socket of a browser sessions so you can edit multiple mardown files at once and still be able to preview all of them in the browser.

On closing the buffer, the server terminates the browser session associated with this exact buffer.


Configuration
-------------
### g:instant_markdown_slow

By default, vim-instant-markdown will update the display in realtime.  If that taxes your system too much, you can specify

```
let g:instant_markdown_slow = 1
```

before loading the plugin (for example place that in your `~/.vimrc`). This will cause vim-instant-markdown to only refresh on the following events:

- No keys have been pressed for a while
- A while after you leave insert mode
- You save the file being edited

### g:instant_markdown_autostart
By default, vim-instant-markdown will automatically launch the preview window when you open a markdown file. If you want to manually control this behavior, you can specify

```
let g:instant_markdown_autostart = 0
```

in your .vimrc. You can then manually trigger preview via the command ```:InstantMarkdownPreview```. This command is only available inside markdown buffers and when the autostart option is turned off.

### g:instant_markdown_open_to_the_world
By default, the server only listens on localhost. To make the server available to others in your network, edit your .vimrc and add

```
let g:instant_markdown_open_to_the_world = 1
```

Only use this setting on trusted networks!

### g:instant_markdown_allow_unsafe_content
By default, scripts are blocked. To allow scripts to run, edit your .vimrc and add

```
let g:instant_markdown_allow_unsafe_content = 1
```

### g:instant_markdown_allow_external_content
By default, external resources such as images, stylesheets, frames and plugins are allowed.
To block such content, edit your .vimrc and add

```
let g:instant_markdown_allow_external_content = 0
```

Supported Platforms
-------------------
OSX, Unix/Linuxes*, and Windows**.

<sub>*: One annoyance in Linux is that there's no way to reliably open a browser page in the background, so you'll likely have to manually refocus your vim session everytime you open a Markdown file. If you have ideas on how to address this I'd love to know!</sub>

<sub>**: In Windows, there's no easy way to execute commands asynchronously without popping up a cmd.exe window. Thus, if you run this plugin without `g:instant_markdown_slow`, you might experience performance issues.</sub>

FAQ
---
> It's not working!

- Make sure `instant-markdown-d` was installed as a global module (e.g. using `npm -g install`)
- If you're on OSX and are using zsh, try to add `set shell=bash\ -i` in your `.vimrc` to set interactive bash as the default vim shell. (See [this issue](http://github.com/suan/vim-instant-markdown/issues/41))

> Why don't my `<bla>.md` files trigger this plugin?

By default, vim versions before 7.4.480 only recognize files ending with `.markdown`, `.mdown`, and `README.md` as markdown files. If you want `<anything>.md` to be recognized, I recommend installing one of many markdown plugins available, such as [this one][tpope-markdown].

etc.
---
If you're curious, the code for the mini-server component for this plugin can be found at http://github.com/suan/instant-markdown-d. A plugin can easily be written for any editor to interface with the server to get the same functionality found here.


[ss]: http://dl.dropbox.com/u/28956267/instant-markdown-demo_thumb.gif  "Click for bigger preview"
[ssbig]: http://dl.dropbox.com/u/28956267/instant-markdown-demo.gif
[gfm]: http://github.github.com/github-flavored-markdown/
[curl]: http://curl.haxx.se/download.html
[tpope-markdown]: https://github.com/tpope/vim-markdown
