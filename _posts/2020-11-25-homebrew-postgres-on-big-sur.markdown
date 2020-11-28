---
layout: post
title: "Homebrew Postgres on Big Sur"
date: 2020-11-25 09:34
comments: true
categories: web-development
published: true
---
I recently ran into an issue with Postgres after upgrading to Big Sur. This is pretty typical for me after OS updates; the feedback loop between updates is too long for my memory. After flailing and staring at [countless](https://stackoverflow.com/questions/13410686/postgres-could-not-connect-to-server) [Stack](https://stackoverflow.com/questions/24627701/a-server-is-already-running-check-tmp-pids-server-pid-exiting-rails) [Overflow](https://stackoverflow.com/questions/17822974/postgres-fatal-database-files-are-incompatible-with-server) [threads](https://stackoverflow.com/questions/19076980/postgres-versions-are-not-compatible), I found my way out of the woods. Here's my particular breadcrumb trail.

<!-- more -->

After starting my local Rails server after the OSX upgrade, I was greeted with the dreaded error "connection to database failed: could not connect to server: No such file or directory". I rushed through my typical fix of deleting the postmaster.pid which usually gets left behind when my machine restarts only to find that [this can cause worse problems when done incorrectly](https://superuser.com/questions/553045/fatal-lock-file-postmaster-pid-already-exists). I then when through my typical debugging questions:
1. Which install of Postgres am I running?
2. And which version of Postgres am I running?

### Debugging
When I get stuck, I take a deep breath and gather up my assumptions. In this case, I know that Postgres can be installed a number of different ways, and I want to verify how the software is installed. I like installing Postgres via homebrew. With a quick command in the terminal, `which postgres`, I see that it's installed in `/usr/local/bin` which I know is where homebrew installs. Next, I check the version with `postgres --version` and I get back 13.x which is not the 12.x I expected.

Now I had two options:
1. Bring my install up to the latest, 13.x or
2. Revert my install to 12.x.

I'm a believer in intentional upgrades and intentional changes. I want to minimize the surface area and variables between known working versions of my software. The best way to do that would be to walk my local copy back to 12.x.

### Downgrading Postgres
This was the trickiest part. I needed to install 12.x and then point homebrew at the install.

Installing the older version of Postgres wasn't too bad. `brew install postgres@12.5`
got the old version my app was expecting onto my machine. But checking the postgres version still returned the new Postgres install. That makes some sense. Homebrew wasn't installing the old version of Postgres over the existing 13.x. It was allowing both to be installed on my machine. After fumbling with a few commands and various Stack Overflow posts, I discovered `brew link --overwrite postgresql@12`. This pointed the `postgres` command at the latest version. Checking the version through the terminal confirmed this. But we're not quite there yet.

### Running Postgres
Finally, I had to restart Postgres. I began by checking which brew services were running with `brew services list`. From here, I could see that postgres@12 was stopped. A quick start kicked the service off `brew services start postgresql@12`. It's important to note that even though I had linked brew to the old postgres version, when I was running brew commands, I still needed to reference the appropriate brew Postgres install, hence the `@12`.

### Next Steps
With this, I could restart Rails and get my app up and running again. A few notes:
* Postgres 13 is still installed on my machine. I should remove it or otherwise plan on upgrading my app to it. I'm really unsure of what state the install is in, and it's likely to trip me up in the future if I don't handle it here while I have this context in my hed.
* I went this route as I really wanted to hold on to the data I had in my local DBs. If I was just getting started or I didn't care about losing that data, I likely would have blown everything away and run `rails db:prepare` and been up and running with Postgres 13.

Hopefully, you see some of your own Postgres situation in my particular path, and you can hop on somewhere and get to your own solution. If not, reach out. I'd love to learn more about how to debug Postgres installs.

### References
1. [https://stackoverflow.com/questions/3987683/homebrew-install-specific-version-of-formula](https://stackoverflow.com/questions/3987683/homebrew-install-specific-version-of-formula)
2.  [https://stackoverflow.com/questions/13573204/psql-could-not-connect-to-server-no-such-file-or-directory-mac-os-x](https://stackoverflow.com/questions/13573204/psql-could-not-connect-to-server-no-such-file-or-directory-mac-os-x)
