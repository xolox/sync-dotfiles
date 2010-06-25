# Quickly push your dotfiles from your workstation to your servers

I've long used a shell script to quickly push my dotfiles (configuration files for UNIX programs) from my workstation to the servers that I manage. The Python script [sync-dotfiles](http://github.com/xolox/sync-dotfiles/blob/master/sync-dotfiles) is the latest incarnation of that shell script and it adds one feature to the mix. When I push my dotfiles certain files are rewritten in transit so that:

  1. All interactive command prompts on a single machine use the same color.
  2. Every machine uses a unique color for interactive command prompts.

In my case the dotfiles for the [Z shell](http://en.wikipedia.org/wiki/Z_shell), the [Bash shell](http://en.wikipedia.org/wiki/Bash_%28Unix_shell%29) and the [Python](http://en.wikipedia.org/wiki/Python_%28programming_language%29), [Lua](http://en.wikipedia.org/wiki/Lua_%28programming_language%29) and [SQLite](http://en.wikipedia.org/wiki/SQLite) [interactive prompts](http://en.wikipedia.org/wiki/Read-eval-print_loop) are rewritten because I've added prompt definitions with [ANSI escape sequences](http://en.wikipedia.org/wiki/ANSI_escape_code) to the relevant configuration files.

## Screenshots

Now whenever I want to be absolutely sure I'm not executing a destructive command in the wrong context (terminal window) I only have to look at the prompt colors to know where I am. Over time this association has grown so strong that I rarely lose track of my context nowadays. Here's a screenshot of my local terminal with a blue prompt:

![Local terminal with prompt highlighted in blue](http://peterodding.com/code/sync-dotfiles/local.png)

And here's the equivalent session on one of my servers which uses a red prompt:

![Remote terminal with prompt highlighted in red](http://peterodding.com/code/sync-dotfiles/remote.png)

## Usage

To use this script you'll need to create a `~/.sync-dotfiles.conf` file. Start with a line `files:` and add the filenames of your dotfiles on the lines below that. Then add the line `hosts:` and below that line list your hosts (SSH aliases), each followed by a colon and then one of the supported color names. Make sure to also add the host `local` with the color you've used in the dotfiles on your local machine (so the script knows which ANSI escape sequences to rewrite). As an example here are my [~/.sync-dotfiles.conf](http://github.com/xolox/sync-dotfiles/blob/master/.sync-dotfiles.conf) and [~/.sqliterc](http://github.com/xolox/sync-dotfiles/blob/master/.sqliterc) files:

    $ cat ~/.sync-dotfiles.conf
    files:
      .bashrc
      .ctags
      .gitconfig
      .inputrc
      .profile
      .pythonrc
      .screenrc
      .shell_aliases
      .shell_prompt
      .sqliterc
      .vimrc
      .zshrc

    hosts:
      local: blue
      server1: red
      server2: green
      server3: magenta

    $ cat ~/.sqliterc
    .prompt "[0;34m> [0m" "[0;34m>> [0m"

Given the above configuration, this is the output of the `sync-dotfiles` script:

    $ sync-dotfiles
    Building tarball for server1:
     - .bashrc
     - .ctags
     - .gitconfig
     - .inputrc
     - .profile (customized colors)
     - .pythonrc (customized colors)
     - .screenrc
     - .shell_aliases
     - .shell_prompt (customized colors)
     - .sqliterc (customized colors)
     - .vimrc
     - .zshrc
    Uploading tarball to server1 ..
    Building tarball for server2:
     - .bashrc
     - .ctags
     - .gitconfig
     - .inputrc
     - .profile (customized colors)
     - .pythonrc (customized colors)
     - .screenrc
     - .shell_aliases
     - .shell_prompt (customized colors)
     - .sqliterc (customized colors)
     - .vimrc
     - .zshrc
    Uploading tarball to server2 ..
    Building tarball for server3:
     - .bashrc
     - .ctags
     - .gitconfig
     - .inputrc
     - .profile (customized colors)
     - .pythonrc (customized colors)
     - .screenrc
     - .shell_aliases
     - .shell_prompt (customized colors)
     - .sqliterc (customized colors)
     - .vimrc
     - .zshrc
    Uploading tarball to server3 ..
    All done!

With a dozen dotfiles and three servers the script finishes within 2 seconds.

## Contact

If you have questions, bug reports, suggestions, etc. the author can be contacted at <peter@peterodding.com>. The latest version is available at <http://peterodding.com/code/sync-dotfiles> and <http://github.com/xolox/sync-dotfiles>.

## License

This software is licensed under the [MIT license](http://en.wikipedia.org/wiki/MIT_License).  
© 2010 Peter Odding &lt;<peter@peterodding.com>&gt;.
