Title: Why Blog-Git (www.blogithub.com)?
Subtitle: Blogging fun with github.

# Why?

So why does the world need another blogging engine? Honestly, it probably doesn't. However,
it seemed that none of the existing blogging platforms gave me exactly what I wanted. I wanted a
blogging engine that matched my coding flow and used the same tools that I use when I write code.

So... I wanted a blog where I could:

1. Write the text with VIM. This pretty much eliminates all blogs where you edit the posts on the
   web.
2. Allowed me to edit posts in Markdown.
2. Push updates by pushing to github. I don't want to have to copy and paste text, ever.
3. Could track history using git without it being totally polluted by stylistic changes. This
   eliminates [Jekyll][1] and [Octopress][2].
3. Didn't look like total sh\*t.
4. Written in ruby. Happy developers work on push changes.
5. Had good support for code formating.

I'm pretty sure I've achieved all these goals with www.blogithub.com. Try it out yourself.

**Note** The [Wheat][3] project came close, but it works with a git repo, not github. And it's
written in node.js, not ruby.

# How to create your own blog on blogithub.com

1. Create a new github repo called blog i.e. `<your github>/blog`. Make sure there is a `README.md` as this is used for your
   welcome page.
2. Create markdown files in the repo for your blog posts.
3. Push to github
4. Go to `http://www.blogithub.com/<your github>`

[1]:http://jekyllrb.com/
[2]:http://octopress.org/
[3]:https://github.com/creationix/wheat
[4]:https://github.com/bguest/blog_git
