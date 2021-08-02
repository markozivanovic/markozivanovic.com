+++
author = "Marko Zivanovic"
title = "Screw it, I’ll host it myself"
date = "2021-04-07"
images = ["images/mydataflow.png"]
tags = [
    "privacy", "data", "ownership"
]
categories = [
    "backup", "self-hosting"
]
+++

It’s all fun and games until someone loses an eye. Likewise, it’s all fun and games until someone loses access to their private and/or business data because they trusted it to someone else.

You don’t have to be an expert [seeker](http://biostatisticien.eu/www.searchlores.org/introd.htm) to be able to quickly duck out (it’s like the verb ‘googling’, but used to describe searching the interwebs through a decent search engine, like <a href="https://duckduckgo.com/" target="_blank">DuckDuckGo</a>) all the stories about little guys being fucked over by ***“don’t be evil”*** type of corporate behemoths.


You know what? Let me duck it out for you:

- [That time I got locked out of my Google account for a month](https://techcrunch.com/2017/12/22/that-time-i-got-locked-out-of-my-google-account-for-a-month)
- [What it’s like to get locked out of Google indefinitely](https://www.businessinsider.com/google-users-locked-out-after-years-2020-10?r=DE&IR=T)
- [Apple Card disabled my iCloud, App Store, and Apple ID accounts](https://dcurt.is/apple-card-can-disable-your-icloud-account)
- [GitHub blocks the entire company because one employee was in Iran](https://twitter.com/sebslomski/status/1344219609923276801)

A drinking game recommendation (careful, it may and probably will lead to alcoholism): take a shot every time you find out how someone’s data has been locked and their business was jeopardized because they didn’t own, or at least back up their data.

## Owning your data and your tools

Owning your data is more than just having backup copies of your digital information. It’s also about **control** and **privacy**. It’s about **trust**. I don’t know about you, but I don’t trust a lot of services with my data (the ones I do are few and far between).

As this is a post about self-hosting, I won’t start preaching (trust me, it’s hard for me not to) how you should consider switching from [WhatsApp to Signal](https://www.androidcentral.com/facebook-wants-your-whatsapp-data-heres-why-you-should-switch-telegram-or-signal), Google Maps to [OpenStreetMap](https://www.openstreetmap.org/), or how you should quit [Instagram](https://www.bloomberg.com/news/articles/2020-09-18/facebook-accused-of-watching-instagram-users-through-cameras) and [Facebook](https://www.businessinsider.com/stolen-data-of-533-million-facebook-users-leaked-online-2021-4?r=DE&IR=T). You’re creating a lot of data there, and they [don’t do pretty things with it](https://www.bbc.com/news/business-49099364). Fuck, I’m already preaching. Sorry about that.

Note: I’m not fully off social media. I’m using Twitter and LinkedIn. Everything on Twitter is public/disposable and I don’t use their private messaging feature. LinkedIn is there for professional correspondence and I will start to taper it off slowly, but that one is tough to quit.

<span class="text-disclaimer">
<b>Disclaimer:</b><br/>
I’m aware most of the people are not <a href="/images/poweruser_unicorn.jpeg">power users</a>, and not everyone will want to spend time learning to set up their own alternatives to the services mentioned and create the backup strategies as I’ve done. It does take some time (mostly to set everything up) and some money. If you’ll take anything from this post, it should be to always back up your data (yes, even though it’s replicated across 5 Google’s datacenters). If shit hits the fan, it may take you a while to adopt new tools or flows, but you will still have your backup. Do your backups early and often.
</span>

## What’s my setup?

I created a simple diagram to roughly show how my personal setup works. Before you say anything – I’m aware that there’s a group of people that wouldn’t consider my self-hosting as pure self-hosting. I’m using [Vultr](https://www.vultr.com/?ref=8305838)* to host my web-facing applications and not a server in my house. Unfortunately, the current situation doesn’t allow me to do that (yet).

So, here’s the diagram. The detailed explanation continues below.

{{< figure src="/images/mydataflow.png" alt="diagram of my self-hosting setup">}}

I’ve separated the diagram into 4 parts – each part represents a different physical location of the data.

The part that gets the most action is the yellow part, living in the cloud.

## Vultr

I’m living in Germany, so the obvious choice was to spin up my instances in [Vultr](https://www.vultr.com/?ref=8305838)‘s* data center in Frankfurt, as ping is the lowest to that center for me.

Right now, I have six compute instances running there. You can see types of cloud compute instances in the picture below. It’s pretty similar to what you would get from DigitalOcean or AWS EC2.

Why did I choose [Vultr](https://www.vultr.com/?ref=8305838)*? They have pretty good customer service there, and I just happened to stumble on them before DigitalOcean got big and popular and AWS became the leader in the cloud computing game. Having said that, for purely private use, I wouldn’t opt for AWS even if I had to choose now. I’ll leave it at that.

{{< figure src="/images/vultr_screenshot.png" alt="vultr cloud compute instances offering" >}}

The breakdown looks like this:

- 1 x $10/mo VPS + 1 x $5/mo object storage – [Nextcloud](https://nextcloud.com/)
- 1 x $10/mo VPS – [Gitea](https://gitea.io/)
- 1 x $5/mo VPS – [Monica CRM](https://www.monicahq.com/) / [Kanboard](https://kanboard.org/)
- 1 x $5/mo VPS – various dev tools + analytics ([Plausible](https://plausible.io/))
- 2 x $10/mo VPS – several web projects that I run for myself and friends

Everything combined costs me $55 per month.

## Nextcloud

Nextcloud is the powerhouse of my everyday data flow and manipulation. With the addition of [apps](https://apps.nextcloud.com/), it’s a really powerful one in all solution to serve as an alternative to widely popular offerings of the FAANG crowd. Once properly set up, not much maintenance is needed.

- [Tasks](https://apps.nextcloud.com/apps/tasks) are the alternative to Todoist or Any.do which I used previously.
- [Notes](https://apps.nextcloud.com/apps/notes) are the alternative to Google Keep. Not as fully featured as Evernote or OneNote I have also tried out at one point, but it’s good enough for me.
- [Calendar](https://apps.nextcloud.com/apps/calendar) is the alternative to Google Calendar I used previously.
- [Contacts](https://apps.nextcloud.com/apps/contacts) are the alternative to Google/Samsung contacts I used previously.

I’m also able to stream music from Nextcloud to my phone, using [Nextcloud music](https://github.com/owncloud/music). For the client, you can use anything compatible with Ampache or Subsonic. My choice is [Power Ampache](https://f-droid.org/en/packages/com.antoniotari.reactiveampacheapp/). I’m not streaming a lot of music though. I always have 30-40 GB of MP3s on my phone that I rotate every now and then.

All the data from Nextcloud is in sync with Synology at my home via CloudSync. A big plus is a nice dark theme for the web UI:

{{< figure src="/images/nextcloud_ui.png" alt="nextcloud dark theme" >}}

## Gitea

I’m a developer and more than I need the air to breathe and coffee to drink, I need version control. My weapon of choice is git, which is lucky because there are a lot of hosting solutions for it out there. I was thinking about this one for a while and it boiled down to [GitLab](https://about.gitlab.com/) vs [Gitea](https://gitea.io/).

For my needs, GitLab was overkill, so I went with Gitea. It’s lightweight, easy to update, and just works. Its interface is clean and easy to understand, and because UI is similar to that one of GitHub, people that I collab with find it as a seamless switch. On the negative side, if you want to customize it, it can be a pain in the butt.

## Monica CRM / Kanboard

[Monica](https://www.monicahq.com/) is a personal CRM. Some people think I’m weird because I’m using a personal CRM. I find it awesome. After I meet people, I often write down some information about them that I would otherwise forget. I sometimes make notes about longer phone calls if I know the information from the call will come in handy later on. Colleagues’ and friends’ birthdays, ideas for their presents, things like that – they go into the CRM.

I mention Monica in my post on how <a href="/dont-ignore-rejection-emails">you should not ignore rejection emails</a>, where you can see another example of how it helps with my flow.

[Kanboard](https://kanboard.org/) is a free and open-source Kanban project management software. I use it to manage my side projects, but I also use it for keeping track of books I read, some financial planning, study progress tracking, etc. It’s written in PHP, it’s easily customizable and it supports multiple users. Usually, when I do some collaborations, I will immediately create an account for that person on both Gitea and Kanboard.

## Development tools & Analytics

[Plausible](https://plausible.io/) is my choice for analytics that I use on several websites that I own. It’s lightweight, it’s open-source, and most importantly – [it respects your privacy](https://plausible.io/privacy-focused-web-analytics). There’s a how-to that I wrote on how you can <a href="/howto/how-to-install-plausible-analytics-on-ubuntu-20-10">install it on an Ubuntu machine</a> yourself. The bonus thing is that I really like developers’ approach to running a business. They have a [cool blog](https://plausible.io/blog) where you can read about it.

Development tools that I’m mentioning are basically a bunch of scripts I have developed and gathered over time. Text encoders/decoders, color pickers, WYSIWYG layout builders, Swagger editor, etc. If I use something often and it’s trivial to implement on my own, I’ll do it.

## Home

Desktop PC and NAS are part of my ‘home’ region.

Desktop is nothing special. I don’t play video games, and I don’t do any work that needs a lot of computing performance. It’s the 8th generation i5 with integrated graphics, 1TB SSD, and 16 gigs of RAM. OS I’m using is Ubuntu – the latest LTS version. It’s installed on both the desktop and laptop.

Everything except the OS and the apps is synced in real-time to Synology by using the Synology Drive Client.

[Synology NAS](https://www.synology.com/en-global/products/DS220j) I’m using is the DS220j. It’s not the fastest thing, but again, it works for me. I have two [Western Digital Red](https://shop.westerndigital.com/products/internal-drives/wd-red-sata-hdd#WD20EFAX) drives (shocking, huh?), 2TB each.

Every last weekend of the month, I will manually backup all the data to Blu-ray discs. Not once, but twice. One copy goes to a safe storage space at home and the other one ends up at a completely different location.

## Offsite backup

This is my ‘everything is fucked, burnt or stolen’ situation remedy. I’m not particularly happy with the physical security I’ve set up at home, so one of the concerns is the theft of disks and backups. Other than moving to a different location where it would be easier to work on upgrading the physical security, my hands are tied regarding this subject (not for long hopefully).

Other things could happen, like fire, flood, etc. Of course, it’s a bit of a hassle, but I believe in being prepared for any type of situation, no matter how improbable it may seem.

## Laptop & Smartphone

When you’re self-hosting, it will, naturally, also reflect on the apps that you use on your portable devices. Previously, my phone’s home screen was filled with mostly Google Apps – calendar, keep, maps, drive. There were also Dropbox, Spotify/Deezer. It’s different now.

I have [De-Googled](https://www.androidpolice.com/2021/03/27/install-use-custom-rom-no-google-apps/) my phone, using [/e/](https://e.foundation/about-e/) and [F-Droid](https://f-droid.org/en/). There are compromises you’ll have to make if you choose to go down this path. Sometimes it will go smoothly, but sometimes it will frustrate the hell out of you. It was worth it for me. I value my freedom and privacy so much more than an occasional headache caused by buggy software.

This is the list of the apps I use frequently that are related to self-hosting:

- [OsmAnd~](https://f-droid.org/en/packages/net.osmand.plus/) – global mobile map viewing & navigation for offline and online OSM maps
- [Nextcloud Notes](https://f-droid.org/en/packages/it.niedermann.owncloud.notes/) – client app for the Nextcloud Notes app
- [PowerAmpache](https://f-droid.org/en/packages/com.antoniotari.reactiveampacheapp/) – lets me stream music from my cloud
- [PulseMusic](https://f-droid.org/en/packages/com.hardcodecoder.pulsemusic/) – my main music app that I use to listen to the music collection stored directly on my phone (30-40GB at any time that I rotate from time to time)
- [Nextcloud](https://f-droid.org/en/packages/com.nextcloud.client) – this is the sync client for the phone and a file browser
- [K-9 Mail](https://f-droid.org/en/packages/com.fsck.k9/) – really, really ugly looking email client that is also the best email client for Android I have ever used

As I mentioned previously, my Laptop is running the latest Ubuntu LTS, just like the desktop PC. To have things partially synced to the NextCloud, I’m using the [official desktop client](https://nextcloud.com/install/#install-clients). Listing other tools that I use as a developer that may be tangibly related to self-hosting would be another two thousand words, so I won’t go into that right now.

## Conclusion

Is it worth the time and hassle? Only you can answer that for yourself.

Researching alternatives to commercial cloud offerings, and setting everything up surely took some time. I didn’t track it, so I can’t say precisely how much time, but it was definitely in the double digits. If I had to guess, I would say ~40 hours.

Luckily, after that phase, things run (mostly) without any interruption. I have a monthly reminder to check for the updates and apply them to the software I’m running. I don’t bother with minor updates, so if it’s not broken, I’m not fixing it.

If I motivate even one person to at least consider the option of self-hosting, I will be happy. Feel free to drop me a message if you decide to do that!

<span class="text-disclaimer">
* Links to [Vultr](https://www.vultr.com/?ref=8305838) contain my referral code, which means that if you choose to subscribe to Vultr after you clicked on that link, it will earn me a small commission.
</span>