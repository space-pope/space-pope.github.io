---
layout: post
title:  "Ember for Beginners part 2: Yak-shaving Edition"
date:   2014-06-02 19:37
categories: [vagrant, ember, ember-cli]
gistUrl: https://gist.github.com/space-pope/d730f54c326cfc18c1a1
---

_**Ed. note**: If you haven't heard the phrase "shaving the yak" before, check out the definition [here][Yak-shaving]. I've found it to be apt for just about every web project I've ever seen._

I mentioned [in my last post]({% post_url 2014-06-01-ember-for-beginners %}) that you might want to put Ember's Handlebars templates into their own files. You might want to do this with a lot of files, actually. Your app is going to get real unreadable real fast if you keep it all in one or two files. Thing is, as you start to do that, you can end up with one file per controller, view, route, model, and so on . . . and they all have to make it onto your site somehow. 

And this is where we hit the wild west. The Ember docs seem to suggest that you go the separate file for everything route, but ask the docs how all the various scripts get picked up and run, and its response is, "Umm...put them in your HTML?" Yeah, let me go ahead and do that. I'll just put 75 `<script src="..."/>` tags at the bottom of my `index.html`. That looks super.

No. I'm not going to do that.

So I spent a few hours trying to shoehorn my Ember app into a [RequireJS][RequireJS] style. That 
a) doesn't feel natural, and
b) just plain doesn't work past a certain point (at least, I couldn't get model fixtures to work properly).

OK, onto the Ember-specific tools. There's a nice tool called [Ember CLI][Ember-cli], but it's still in beta. Let's be honest, though; is there anything written in Javascript that _isn't_ in beta? Ember Data itself is in beta. The Ember CLI structure looks reasonable enough, it uses pure ECMAScript modules (from ES6—that's right; we're coding in the _future_ here), and it's easy enough to install<sup>1</sup>, so let's go for it. And that's where we hit . . .

### Let's shave that yak! 

OK, I'll admit it. I use a Windows machine for development. No, wait! Don't throw that shoe! I might still be of use.

I'm also using a [Vagrant][Vagrant] box with an Ubuntu image to run the actual server. I love me some Vagrant, but not being able to install/use any npm packages was starting to drive me a little insane. I hit the Googles for answers and ended up with the conclusion that it was Windows' saddening lack of proper symlinks (or their inaccessibility to the average user, or something else about symlinks that I got too exasperated to continue investigating) that was causing the problem.

But I really wanted to use some npm goodness, and I also really wanted to keep my git repo (that really lives on my Windows box) synced to Vagrant, so I threw the kitchen sink at it.

"Install pacakges with the `--no-bin-links` (or, on some sites, `--no-bin-link`) flag", they said.

Nope.

"Provision your Vagrant box with the {%raw%}vb.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/share-name", "1"]{%endraw%} config option", they said.

Uh-uh.

Not that these things aren't worth trying; they just weren't working as advertised for me. I might've missed something. Anyway, I ended up doing what any programmer does when they're tired of fighting with something:  I brute-forced it. Here's what ya do:

1. SSH into your Vagrant box if you're not there already
2. Install Ember CLI: `npm install -g ember-cli`, then (if you don't have it) `npm install -g bower`
3. Make a new directory _not inside a shared folder_ to host your app, for example `mkdir /build-temp`
4. `cd /build-temp`, then `ember new <app-name>`
5. Copy all the files and directories generated here, minus the `node_modules` directory, to your sycned folder that's going to be the final destination for your app: `rsync -a --exclude node_modules /www/ /build-temp/<app-name>/` (assuming `/www` is your app's synced folder). `node_modules` is where all those evil symlinks live; they won't copy over to the Windows side quite like one would hope.
6. Develop to your heart's content using the files in your synced folder, using your Vagrant terminal or Windows editor of choice.
7. When you're ready to build/view your progress, run the following script:
<script src="https://gist.github.com/space-pope/d730f54c326cfc18c1a1.js?file=build-ember.sh"></script>
I'm using `rsync` because that way you're only copying the changes in the files rather than shoving the whole app back and forth every time. The `--progress` flag isn't strictly necessary, but it keeps you updated about, well, the progress of the copy. Also note the `sudo -u vagrant` bit. This is because I run this script using `sudo`—probably unnecessary, but meh. Bower doesn't like to run as `sudo`, though, so I switch back to the vagrant user to run that part of the build. The bower step itself is probably only necessary if you're changing your app's dependencies between builds, but if it's not taking you too long, why not run a complete build?

OK, that's about the gist of it (see what I did there? It was unintentional—thanks for stealing that word, Github). I'm now using whatever node packages I like _and_ developing on Windows. Whee!

---

<sup>1</sup>Or, as I say whenever I see ". . . just run `npm install...`", "Squirrel!" Because the entire Node ecosystem feels like it was put together by a group of golden retrievers with ADD. But I digress. Again, everyone's doing super-useful work . . . there's just too much of it. It's a topic for another day.

[Yak-shaving]: http://www.hanselman.com/blog/YakShavingDefinedIllGetThatDoneAsSoonAsIShaveThisYak.aspx
[RequireJS]: http://requirejs.org/
[Ember-cli]: http://iamstef.net/ember-cli/
[Vagrant]: http://www.vagrantup.com/