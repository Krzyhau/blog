---
title: Creating my own blog - manifesto
---
I've created a small blog. This is it! You're reading it! Heyo! 👋

Every once in a while, I do something cool (well, for me at least). Then, I have an urge to post it somewhere, but I don't have a place to do it. I could record a video about it and post on [my YouTube channel](https://www.youtube.com/@Krzyhau) but not everything deserves to be presented in a low-effort video form in my eye. I could probably slap it somewhere on a public chat within a certain community, and actually that's what I've been doing most of the time. But then, I'm kind of spread out between communities in my creative work, and some work don't even have a suitable place to be posted that way.

I don't really have a place to call home, to post whatever crazy idea I manage to realize into existence, send it away into the void of the Internet and let some random strangers admire it. It's kind of discouraging, to the point most of my small projects haven't been posted anywhere, and I can't flex with them to friends, co-workers or random people I meet during events.

I always wanted to make a small blog which would fulfill such purpose, but never gotten around doing it. However, this weekend, when I lost motivation to do literally anything more productive, I've done it! So, to test this idea out, here's a write-up of how I did it.
## The plan

Here's what I expected my blog to be:
 - **Minimalistic** - no unnecessary bloat. I wanted to allow readers to get straight to the point and focus on what matters - the juicy content.
 - **Easy to setup and manage** - why bother having to host an entire CMS like WordPress if all I want to do is to have some text written up and displayed online?
 - **Easy to use** - I want to be able to get a quick access to the insides of the blog and send new content into it, regardless of where I am or what device I'm using.
 - **Configurable** - While I could use some external websites for blogging like Medium.com, I like to have as much control over the platform I'm posting on as possible. I don't care about exposure in this case, I just want to post random stuff!
 - **Preservable** - When I ever decide to switch the back-end to something else, I want the process to be as swift as possible.
 - **Interactive** - A nice bonus for being able to read some random ramblings written by a polish guy is to be able to comment below them to tell him what a bumbling buffoon he is!

## The execution

While I could've written my own simple CMS for this blog, it's an entire project on itself, and I want to start posting stuff rather than have it blocked by ambition. So I went for rather simple yet quite interesting combo.

For start, I'm hosting this blog on [GitHub Pages](https://pages.github.com/). Fantastic feature - you can turn any repository into a working website, all for free! While I am paying for a custom domain, GitHub can give you your own (`[username].github.io`). Probably a better use case would be a landing page for your project (it's amazing how awful GitHub is to navigate for casual users - the number of times I have to explain how one can download released executable from a GitHub repository is kind of surprising) but I've seen people host their blogs on it, so why shouldn't I?!

What's more important is that GitHub Pages is powered by [Jekyll](https://jekyllrb.com/) in a background, so I can take advantage of an ease of writing stuff in [Markdown](https://www.markdownguide.org/basic-syntax/). My posts written in this quite simple format can then be generated into a static HTML pages based on a previously prepared template. This is fantastic for two reasons. One is that I could easily migrate to any other solution, whether it's another instance of Jekyll lying on a VPS or a completely custom solution made from scratch (because I fully expect to go crazy with such idea at some point in the future). But more important benefit is that I can use [Obsidian](https://obsidian.md/) to easily write the posts in a WYSIWYG fashion. It's a tool that I've already used for making my own personal notes, so it's quite convenient for me to introduce it into another part of my life!
Look! Here's me writing this very page in Obsidian right now!

![](assets/2025-02-02-blog-creation-manifesto/writing-recursive.jpg)

Being a good static content generator, Jekyll gives you tools to fully customize the looks of your website. For now, I've decided to just ~~shamelessly steal~~ acquire a default [Hacker](https://github.com/pages-themes/hacker) theme and heavily customize it to my needs (in fact, I'd argue there's not a lot left from that theme). I ended up with something fairly simple and hopefully easy to consume, but I might tweak it in the future if I change my mind. I guess I'll leave a screenshot of how it's looking right now for historic preservation:

![](assets/2025-02-02-blog-creation-manifesto/first-screenshot.jpg)

With all of that set up, the last thing that's missing is the interactivity part. You may think that, being a static website, it'll be difficult to set up any kind of comment section system. Apparently not!

After a little bit of googling, I found [utterances](https://utteranc.es/), a small JavaScript widget adding a comment section based on GitHub Issues. Since I'm already hosting blog on a GitHub repository, it's the exact puzzle piece I needed! So now, not only you can completely ignore this entire post by scrolling all the way down, but also post a mean comment about how much you dislike my content (also with an ability to be formatted in Markdown!), and even use emoji reactions!

![](assets/2025-02-02-blog-creation-manifesto/comment-screenshot.jpg)

I'm already quite happy with this setup and I'm eager to write some stuff to fill it in with **the juice™️**!  So, uh... I guess I'll see ya in the next post!