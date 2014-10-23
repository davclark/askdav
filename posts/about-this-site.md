<!-- 
.. link: 
.. description: What is davclark aka Dav Clark doing here?
                Why is he using Nikola?
.. tags: about
.. date: 2014/01/19 22:06:08
.. title: About This Site
.. slug: about-this-site
-->

## What's this about?

This post is largely my own "kicking the tires" on
[Nikola](http://getnikola.com). The important information from this post should
make its way to an about page or a sidebar.

I currently work in the [D-Lab](http://dlab.berkeley.edu), with a strong focus
on public-facing science. Notably, I'm working with [Oroeco](http://oroeco.com)
and others on climate and behavior, and generally interested in science with
social impacts, currently focused on [Stats for
Change](http://statsforchange.com) (a Bay Area hub) and the [Data Science for
Social Good Fellows Program](http://dssg.io). I'm also interested in the role of
personal development, ranging from education to "mindful practice" (like
meditation, taiji, the Feldenkrais Method®, and Contact Improv).

This design of this site is meant to support & cross-reference that work!

<!-- TEASER_END -->

## Why Nikola? (And what's my recipe?)

[Nikola](http://getnikola.com) and [Pelican](http://blog.getpelican.com) are the
heavy contenders in python static site generation. Both currently support
IPython notebooks as sources for rendered pages, which is rad. Pelican requires
the use of one of
[two](https://github.com/getpelican/pelican-plugins/tree/master/liquid_tags)
[plugins](https://github.com/danielfrg/pelican-ipythonnb), and while there is a
plugin on GitHub for Nikola, the current version has been moved into the core
(and the author [intends to keep it
there](https://github.com/getnikola/nikola/issues/824)).

I'm interested in seeing how far we can abuse the static approach to avoid
reliance on application servers.  As such, the availability of
[Mako](http://www.makotemplates.org/) in Nikola gives me the full power of
Python right there in my templates, and doesn't require me to learn much
about Nikola's build process to do so (except that I know my Mako templates will
run).

### The recipe

I'm still up in the air about using [Anaconda](http://continuum.io/downloads)
for stuff like this (instead of a virtualenv), but that's what I did. In case
you're wondering, "Problems with Anaconda" (which are obviously not
deal-breakers!) include:

 1. the fact that it does not automatically install pip in a new environment, as
    virtualenv does, which leads to confusion when installing packages,
 2. the fact that if you do things the simple way (i.e., just give conda a list
    of packages you want), it won't necessarily do the smart thing anyway and
    get the binary versions, and
 3. to my knowledge the free edition doesn't compile numerics against
    Accelerate.framework (or OpenBLAS) on OS X (not a big issue if you're an
    academic / can afford the confusingly named continuum Accelerate package
    that includes MKL). This last point shouldn't matter much at all here!

Anaconda, contrary to popular conceptions, works out of the box without
registration, and is [freely redistributable since version
1.4](http://continuum.io/blog/anaconda-1-4-released).

Since I use Anaconda alot, I was actually able to mostly guess correctly which
dependencies are available in anaconda, so the way I set up my environment is
like so:

    conda create -n askdav ipython pip requests markdown pillow pygments docutils lxml pytz

After that, you should be GTG with a simple `conda install nikola`. Recent
versions of `conda` will automatically invoke `pip` if the binary package is
unavailable.
