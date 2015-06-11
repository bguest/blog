Title: Git Tricks  
Comments: true  

I'll admit it, I'm a little particular when it comes to git. I liked things squashed and rebased, I
force push and `git add -p`.

Here are some things Ive picked up,

### ZSH + Oh My Zsh

I use [ZSH][1] with [oh my zsh][2], I highly recommend that you do too. Don't forget to turn on the
`git` part of oh-my-zsh; in your `.zshrc` file you should have a line that looks like.

    plugins=( ... git ... )  # Note: the ... represents other plugins

This gives you access to some fabulous [git shortcuts][3], some ones that I use all the time:

    gco     # git checkout
    gcb     # git checkout -b
    gst     # git status
    grhh    # git reset head --hard
    ggl     # git pull origin <current branch>
    ga -p   # git add --pach
    gc!     # git commit -v --amend              # uses this to tack on stuff to last commit
    ggp     # git push origin <current branch>
    ggf     # pit push -f origin <current branch>

I've also added the following aliases to my `~/.zprofile`

    alias gap="git add -p"

### The '-'

The `-` (just like with UNIX paths), stands in for the last branch. I've always used it for things
like

    gco - # Checkout the last branch

but, you can also use it in some creative ways ie:

    cd master; git merge - --ff-only

### GITX

It's a totally awsome GUI for Mac that allows you to quickly visualize git...

Download the [correct][4] version. The original is abandonware.

After you've downloaded the correct version, launch the GUI and under the apple menu select `Enable
Terminal Useage...`, now you can type

    gitx

In your terminal to instantly launch gitx for that repository.

Make sure download the
correct branch as the original

[1]:http://www.zsh.org/
[2]:https://github.com/robbyrussell/oh-my-zsh
[3]:https://github.com/robbyrussell/oh-my-zsh/blob/master/plugins/git/git.plugin.zsh
[4]:http://rowanj.github.io/gitx/
