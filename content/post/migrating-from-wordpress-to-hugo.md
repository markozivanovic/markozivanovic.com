+++
author = "Marko Zivanovic"
title = "Migrating from WordPress to Hugo"
date = "2021-06-27"
images = ["images/migrate_wordpress_hugo.png"]
tags = [
    "wordpress", "hugo", "migration"
]
categories = [
    "self-hosting", "blog"
]
+++

For some time now, I have wanted to set up a blog using a static site generator. There was no real reason behind that wanting. It was just the plain old curiosity combined with the drive to learn new things.

I'm not sure when exactly the trend began, but I remember articles named "I switched my blog to [[Hugo](https://gohugo.io/)/[Jekyll](https://jekyllrb.com/)] " started popping up across the interweb frequently some two years ago.

It wasn't until I had to write a documentation page for one of my [side projects](https://www.tooloodr.com/documentation/index.html) that I started to think seriously about SSGs.

I ended up writing documentation manually in HTML at that time. I just wanted to push it out ASAP, and I rejected the thought of reading the documentation of any SSG, even for measly 10 minutes.

Even though I didn't use SSG for that task, it was at that moment that I decided I will most definitely use a static site generator for my web properties that are existing purely for displaying static content. Hell, <a href="/images/poweruser_unicorn.jpeg">I'm a developer</a>, no excuses not to.

## Starting with WordPress

When I decided to write <a href="/dont-ignore-rejection-emails">my first blog post</a> last year in April, I wrote it in vim. It was sitting on my disk for two days because I had no idea what to use to publish it online.

SSGs were popular, but the decision fell on WordPress because I have installed it many, many times before. It was familiar, and I felt comfortable with it. I knew the plugin ecosystem, and I knew I could connect to the WP dashboard from anywhere and type out a post inside its WYSIWYG editor and publish it - without having to think about anything else.

I connected to the VPS I rent, installed PHP, MySQL, and Nginx. I extracted the WP zip and went through few screens of the installation procedure. After that, I took the freely available [theme](https://andersnoren.se/teman/lovecraft-wordpress-theme/) and pasted my blog post into the WP editor. That was all I had to do - 10 minutes from start to finish. Easy!

If I wasn't obsessed with optimizing things that don't matter much, and I didn't consider it as a fun way to spend Saturday evenings, I would leave it at that. I didn't because I am obsessed with things some would consider stupid - like optimizing a blog that holds a dozen blog posts.

## Things that started to frustrate me

When I wrote the <a href="/screw-it-ill-host-it-myself">first blog post that got some attention</a>, hundreds of people started commenting on it. There were some positive comments and some comments that were not so nice.  I didn't care about any of them but one. It was the comment that said my blog was slow.

I was thinking - here I am, a guy that talks about optimizing stuff all the time, and my website consisting only of text is slow AF. And it was. There was no excuse. I was making people download a megabyte of data only to display a few lousy paragraphs of text. 

Just for the front page where the last ten blog posts were listed, your browser had to:

- download **1.04 MB** of data
- make **38** requests
- wait for **1.56** seconds

And that was after an [optimization plugin for WordPress](https://wordpress.org/plugins/hummingbird-performance/) combined and minimized the assets and cached what it could. **Awful!** The only thing I could do that was worse would be not displaying the text to people that have Javascript disabled. I made sure that wasn't the case. I don't want to end up in hell.

Now, to be clear, this is not a WordPress problem. It was my problem. If I had built the theme from scratch, I would have more control. In that case, I could've been more careful about the assets I was linking. 

I could've had a better caching mechanism and maybe a CDN in front. That would make the experience better for the reader. It would only mask the underlying problem, though. I'm serving only text - why the fuck do I need so many resources? 

## Hello Hugo

It boiled down to the decision between Hugo and Jekyll in the end. I forgot why I chose the former. Maybe it was someone's tweet or a blog post I read. 

I posted the following tweet on May 17th:

{{< tweet 1394355058586923010 >}}

Note: [tweet shortcode](https://gohugo.io/content-management/shortcodes/#tweet) that displays this tweet comes with Hugo

Initially, I thought I'll need to spend at least a day going through the documentation. I was mistaken. The documentation is well written, and almost everything has an example.

In the end, I didn't even use any of the [WordPress migration tools](https://gohugo.io/tools/migrations/). I don't have many posts (not yet!), so I decided to convert them manually to markdown files. Also, it gave me the chance to re-read the content I wrote and check if I can find any typos to fix.

I love writing in markdown. These are two tweets I wrote about it recently:

{{< tweet 1394350837049200641 >}}
{{< tweet 1409026581952147456 >}}

If documentation is easy to follow, you may ask **why the hell it took me over a month to finish the migration**, because I'm posting this on Jun 27th and I started migrating the blog on May 17th. 

It was mostly being occupied with other things. I planned to finish this in two months, putting in an hour or two per week into it. I've been investing more of my off time into writing - something I will do more of this year (and now I have a faster blog!).

In total, I spent between ten and twelve hours on migration. It was mostly me changing some CSS settings and reversing them. I don't have an eye for design, and it's always been hard for me.

I started with a theme I thought looks cool - [Anatole](https://themes.gohugo.io/anatole/), and then I added my custom styling on top of it. In the process, I played with the layouts and learned most of the insides of Hugo and its theming. It will come in handy in the future, I have no doubt about that.

Regarding the process of writing a post and publishing it on the blog, it's as simple as it can be

1. Write a blog post in markdown
2. Run **hugo** to build it
3. SCP to the server

When I say **simple**, I'm saying it's simple and easy for me - a person with a programming background that feels comfortable with markdown and command line.

I'm not trying to say that this is something that everyone should do. I believe that WordPress provides a great platform for aspiring bloggers that don't want to or don't know how to go through this process. 

## Just give me the stats

Here are the stats for the website after the migration. I can say it's much better now. There is still room for improvement, of course, but I'll refrain from going fully mad and trying to cut down more stuff. I'm happy with it for now.

To show you the front page of my blog, your browser now needs to:

- download **77.06kB** of data (<span style="color: green"> -92.76%</span> size)
- make **12** requests (<span style="color: green"> -68%</span> requests)
- wait for **467** miliseconds (<span style="color: green"> -70%</span> time)