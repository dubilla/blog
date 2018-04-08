---
layout: post
title: "Git Submodules"
date: 2013-07-30 23:50
comments: true
categories: web-development
---
I normally feel fairly well-versed in Git, which is to say, I can commit code, I can see what's happening, and I generally know enough to not screw anything up. I thought my Git Fu was tight enough that I at least had a good grasp on all that could be done with the version control system until I came across a project that used submodules. In Git, a submodule is a connection from one repository to another. Say, for instance, you're working on a suite of similar products. You want each one to have its own repository. That makes plenty of sense. But suppose each of these products shared a common messaging platform. Instead of maintaining this same code across the suite of products, which would defeat a lot of the purpose of setting up a VCS, you can give it its own subrepo, and have each product in your suite set up a submodule to reference that repo. All seems well and good, but there are a few gotchas to keep in mind.

<!-- more -->

Let's clone a repo that uses submodules down to local. You start making some changes, gunning away on some code, and you're feeling really good. So, you decide to check your progress so far with a good old `git diff`. Everything looks exactly as you expect except for this one bit of output:
<blockquote>
--- a/lib/messaging
+++ b/lib/messaging
</blockquote>
That seems odd. There should be all sorts of good stuff in the messaging directory. And there is, but as it turns out, your main repository, or superrepo, is not keeping track of any of the minutiae inside the subrepo. All it keeps track of is the commit id associated with that repo. When you need to update your subrepo, a simple `git submodule update` command runs through your list of submodules and pulls from each of the associated subrepos. How does your superrepo know about the submodules? That brings us to the .gitmodules file in the root of your project.

The .gitmodules file is a git config file that contains references to each of your repo's submodules. Each reference consists of a name, a path to the submodule in your repo, and a link to where the subrepo can be cloned from. Going back to our example, let's take a look at how our suite of products would share a messaging library:
<blockquote>
[submodule "messaging"]
      path = lib/messaging
      url = git://github.com/dubilla/Messaging.git
</blockquote>
As you can imagine, your .gitmodules config file should be included in your repot and should NOT be listed in your .gitignore file. Solid. We're getting a decent handle on this whole Git submodules thing. Now let's go back to a simpler time when we first cloned the repo with submodule references, but this time, let's do things the right way by initializing our submodules locally.

Alright, we've cloned our repo, and we're ready to set up our submodules. We're combing through the directory structure, and we notice an empty <em>lib/messaging</em> directory. This is normal. The superrepo knows it has a directory there, it just has no concept of what's going on inside it. Let's forge that connection. From the root of your repo, run `git submodule init` to create your .gitmodules file. Then, run `git submodule update` to get all of the code for each submodule in your project. Voilà. You're up and running your cloned repo with submodules in tow.

The title of this post is "Git Submodules and Private Repos", so we're not quite out of the woods yet. There's one major caveat. If your submodule is referencing a private repo, you need to update your .gitmodules file to use a specific format for the path to that repo. That reference url must be of the format:
<blockquote>git@github.com:[user]/repo.git</blockquote>
Otherwise, you're likely to get an error stating that the repo could not be found. One last thing to note regarding updating the .gitmodules file. Sometimes editing your .gitmodules file is not enough to update your submodule references. Git submodules contain an oddly two-tiered configuration between the .gitmodules and .git/config/ files. So if you update .gitmodules and start running git submodule update and seeing an older path listed in the output, there's a good chance the .git/config file simply never got updated. You can fix this by hand in the file or running `git config submodule.[submodule].url [newurl]` to update the config file. This can be fairly annoying, but once submodules are set up correctly for your repo, you shouldn't have to update any of them on a regular basis.

Overall, submodules seem like a tool best suited for larger products and larger teams. I don't have any experience regarding referencing libraries I don't own as submodules, which could bring in a whole new world of submodule management. Imagine tweaking the library locally to fit your projects' needs, and then getting the latest on the subrepo as commits are pushed completely separate of your implementation. While it could be very useful, you'll most likely have a fair amount of merging in your future. Have you dealt with submodules in this advanced way? Are you just getting started like I am? Have you started <a href="http://codingkilledthecat.wordpress.com/2012/04/28/why-your-company-shouldnt-use-git-submodules/" title="Coding Killed the Cat Blog">bemoaning submodules</a> and <a href="http://blogs.atlassian.com/2013/05/alternatives-to-git-submodule-git-subtree/" title="Atlassian Blog">praising</a> git subtrees yet? Start some talking in the comments.

<h4>References</h4>
1. <a href="http://git-scm.com/book">Pro Git</a>: <a href="http://git-scm.com/book/en/Git-Tools-Submodules">http://git-scm.com/book/en/Git-Tools-Submodules</a>, as always is fantastic and provided most of the guidance in my Git submodule education
2. <a href="https://groups.google.com/forum/#!forum/github">Github Google Groups</a>: <a href="https://groups.google.com/forum/#!topic/github/B5VuXiO3aU0">https://groups.google.com/forum/#!topic/github/B5VuXiO3aU0</a>, paved the path to healing my private repo submodule wounds.