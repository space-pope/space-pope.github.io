---
layout: post
title:  "Ember for Beginners, or, 'Where did my weekend go?'"
date:   2014-06-01 18:54
categories: ember
gistUrl: https://gist.github.com/space-pope/3af8fa82dd82df8614fd
---

# Exposition and disclaimer

First, let me start off by saying that I appreciate the hard work so many people put into making open source software available. It's fantastic to be able to build a project based on foundations written by others to avoid constantly reinventing the wheel. I think that just about all the projects I mention in this post are great ideas and show a ton of promise. Don't let any sarcasm that may or may not crop up sour the mood.

OK, now that that's out of the way...

## Ember (is a word for something that can burn you)

So I've decided to use [Ember][Ember] for my latest (OK, first _real_) web project. I'm not a huge fan of relying on naming conventions for functionality, but Ember's at least make sense most of the time, and so far the framework looks like a decent way to enforce (or at least encourage) some separation of concerns in the front end world.

I read a decent bit of the documentation (which seems a bit incomplete, but kudos to them for having as much as they do) and followed some of their Hello World example, changing some names as I went. That's where I ran into...

### Ember gotcha #1: `application` vs. `index`

This one is at least partially covered in the intro tutorial and documentation, but I got lost somewhere between not naming my app "App" and moving templates into their own files instead of making my eyes bleed with a 2,000-line HTML file, and I got tripped up with little detail.

Unless you're changing the names of the default `ApplicationRoute`, `IndexRoute`, etc., you'll want two templates to get yourself up and running:

#### **`application`**

The `application` template is your scaffolding. You can put it in your main HTML file inside a `<script type="text/x-handlebars"></script>` block (note the lack of a `data-template-name` attribute), or you can put it in an `application.hbs` (or whatever your extension of choice). A simple example of the `application` template might look like this:
  <script src="https://gist.github.com/space-pope/3af8fa82dd82df8614fd.js?file=application.hbs"></script>
Notice the {%raw%}{{outlet}}{%endraw%} tag (or "helper"). That's where your dynamic content will be rendered—dynamic content that will first come from . . .

#### **`index`**

The `index` template (again, unless you've changed names/routes, but you're a beginner like me, so you're not doing things like that . . . right?) is the first template/view that will be rendered into your {%raw%}{{outlet}}{%endraw%} when a visitor hits `/`, your root URL. An example (OK, a really _bad_ example) of an `index` template might be:
<script src="https://gist.github.com/space-pope/3af8fa82dd82df8614fd.js?file=index.hbs"></script>
You, um, probably want more than this here, but you sort of get the idea. You don't need an {%raw}{{outlet}}{%endraw%} helper here unless you're planning on having subroutes that you'll want to render _inside_ this template.

### Ember gotcha #2: Array controllers and templates

I don't have much of an introduction for this one, but I'm going to add it in here anyway. If you're looping over the contents of an array in a model (say, a fixture you've set up with multiple members for a mock-up), you might not be able to get the {%raw%}{{#each}}{%endraw%} helper to work quite the way the docs say it'll work. In order to properly loop over a plain model I set up, I ended up using {%raw%}{{#each item in controller}}{%endraw%}—using the literal word `controller`—instead of the {%raw%}{{#each photo in photos}}{%endraw%} syntax (where `photos` is the model name) used in the docs.

OK, that's enough noobery for one post. Next up:  Making all this play nicely(ish) with [Vagrant][Vagrant].


[Ember]: http://emberjs.com/
[Vagrant]: http://www.vagrantup.com/