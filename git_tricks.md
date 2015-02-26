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
    gst     # git status
    grhh    # git reset head --hard
    ggpull  # git pull origin <current branch>
    gap     # git add --pach
    gc!     # git commit -v --amend              # uses this to tack on stuff to last commit
    ggpush  # git push origin <current branch>

### The '-'

The `-` (just like with UNIX paths), stands in for the last branch. I've always used it for things
like

    gco - # Checkout the last branch

but, you can also use it in some creative ways ie:

    cd master; git merge - --ff-only

[1]:http://www.zsh.org/
[2]:https://github.com/robbyrussell/oh-my-zsh
[3]:https://github.com/robbyrussell/oh-my-zsh/blob/master/plugins/git/git.plugin.zsh
