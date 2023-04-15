---
title: "Zsh - The enhanced bash"
datePublished: Mon Jan 21 2019 13:42:04 GMT+0000 (Coordinated Universal Time)
cuid: cl3zbfh9b00ru5rnv7btz40gj
slug: zsh-the-enhanced-bash
cover: https://cdn.hashnode.com/res/hashnode/image/unsplash/MPKQiDpMyqU/upload/v1654349771736/14F4m88CK.jpeg
tags: bash, zsh, shell

---

Some call it shell, others call it the terminal, command line. Whatever it is called, it is the lifeblood of programmers using a unix-based system.

My first encounter with the Z shell was during my internship in 2018 summer. My tech lead was troubleshooting some docker issues on my laptop and he remarked:

> "Bash sucks. Use z shell, it is so much better"

## Benefits

Every programmer's goal is to maximise productivity while minimising the amount of effort expended.

Without diving into the history of bash and Z shell, let's talk about some wins I have experienced in my workflow:

**cd tab autocomplete**  
For bash, the tab autocomplete shows a list of directories matching the current word, while Z shell allows me to cycle through the directory list with a double tab.

It is not a big deal when there is only one directory that matches the current word, but can save a tiny bit of time when there are multiple matches and the names are long.


**Bash tab autocomplete**
![bash tab autocomplete](https://cdn.hashnode.com/res/hashnode/image/upload/v1654349803026/yuEavRj86.jpeg align="left")


![zsh tab autocomplete](https://cdn.hashnode.com/res/hashnode/image/upload/v1654349838147/_owcNBmYR.jpeg align="left")

### Oh my Zsh!

We can't talk about Z shell without mentioning the awesome [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh) framework, which comes with themes and plugins that supercharges the Z shell itself.

It is similar to jetpack from wordpress, which does a lot of magic for us.

In the words of oh-my-zsh's maintainers:

> Oh My Zsh will not make you a 10x developer...but you might feel like one

**z command: smart cd based on history**  
Gone are the days when we have to manually `cd` or `Open in Terminal` in our working directory.

Just do a `z <my_working_directory>` and Z shell will (hopefully) get you to the destination in a jiffy. Not to mention that the command has a tolerance for typo error.

**Note**: The path that we want to `z` into should be manually navigated to at least once, so that Z shell will "remember" it for future use.


**zsh z cd based on history**
![zsh z cd based on history](https://cdn.hashnode.com/res/hashnode/image/upload/v1654349894363/6c4Wlw9zP.jpeg align="left")


**git branch no more**  
Let's take a moment to reflect on how much time was wasted by typing `git branch` in our entire lifetime.

With the `agnoster` theme, the current branch is clearly shown in the terminal.

**oh-my-zsh git plugin**
![oh my zsh git plugin](https://cdn.hashnode.com/res/hashnode/image/upload/v1654349951146/Bjp8o9Fnj.jpeg align="left")


**Autocomplete suggestion**  
Z shell is all about being lazy to save precious time, and this takes the cake because no one got the time to type in the same command more than once.

Autocomplete suggestions shows up as a muted gray color as we type the commands, and we just have to hit → to accept the suggestion.


**oh-my-zsh autocomplete suggestion**
![oh my zsh autosuggestions](https://cdn.hashnode.com/res/hashnode/image/upload/v1654350016381/L1-0Bf7GT.jpeg align="left")

## Summary

I'm sure these are just barely scratching the surface of Z shell and bash, but for what it's worth, these features have certainly turned me into a lazier and more productive programmer!

## References

[oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)  
[Install zsh](https://github.com/robbyrussell/oh-my-zsh/wiki/Installing-ZSH)  
[zsh autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)

**My zsh config**


**My oh-my-zsh theme**
![oh my zsh theme](https://cdn.hashnode.com/res/hashnode/image/upload/v1654350048414/lMdzGZdrt.jpeg align="left")


**My oh-my-zsh plugins**
![oh my zsh plugins](https://cdn.hashnode.com/res/hashnode/image/upload/v1654350074350/lZh6Q4v9q.jpeg align="left")
