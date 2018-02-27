---
layout: post
title: Announcing Tumbleweed snapshot review site
---

Adapted from [announcement to `opensuse-factory` mailing list](https://lists.opensuse.org/opensuse-factory/2018-02/msg01152.html):

Following up on my [prior announcement](https://lists.opensuse.org/opensuse-factory/2017-11/msg00591.html) of Tumbleweed Snapshots, introducing
a [snapshot review site](http://review.tumbleweed.boombatower.com/). By utilizing a variety of sources of feedback
pertaining to snapshots a stability score is estimated. The goal is to err on
the side of caution and to allow users to avoid troublesome releases. Obviously,
there are many enthusiasts who enjoy encountering issues and working to resolve
them, but others are looking for a relatively stable experience.

[![review release list]({{ "image/review.tumbleweed-2018-02-13.png" | absolute_url }})]({{ "image/review.tumbleweed-2018-02-13.png" | absolute_url }})

Releases with a low score will continue to impact future release scores with a
gradual trail-off. Given that issues generally are not fixed immediately in the
next release this assumes the next few releases may still be affected. If the
issue persists and is severe it will likely be mentioned again in the mailing
list and the score again reduced.

Major system components that are either release candidates or low minor releases
are also considered to be risky. For example, recent Mesa release candidates
caused white/black screens for many users which is not-trivial to recover from
for less-technical users. Such issues come around from time to time since openQA
will not catch everything.

Release stability is considered to be pending for the first week after release
to allow time for reports to surface. This of course depends on enthusiasts who
update often, encounter, and report problems.

The scoring is likely to be tweaked over time to reflect observations. It may
also make sense to add a manual override feature to aid scoring when something
critical is encountered.

Integrating the scoring data into the tumbleweed-cli would allow users to pick a
minimum stability level or score and only update to those releases. Such a
mechanism can be vital for systems run by family members, servers, or the wave
of gamers looking for the latest OSS graphics stack.

For more details see the [code behind the site](https://github.com/boombatower/tumbleweed-review). Currently, you can see the
very low scores for the releases laden with shader cache issues and those
therafter. This is the first iteration of the site so nothing too fancy and the
score is fairly basic.

The site also provides a [machine readable (YAML) version of the data](http://review.tumbleweed.boombatower.com/data.html).

As a side-node, Tumbleweed Snapshots are limited to 50 snapshots due to a
[hosting restriction](http://release-tools.opensuse.org/2018/02/09/w05-06.html), but that should generally be over two months worth.

Hopefully others find this useful, enjoy!

[![review individual release]({{ "image/review.tumbleweed-2018-02-03-release.png" | absolute_url }})]({{ "image/review.tumbleweed-2018-02-03-release.png" | absolute_url }})
