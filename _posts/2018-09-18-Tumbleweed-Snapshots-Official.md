---
layout: post
title: "Announcing Tumbleweed Snapshots Official Hosting"
---

Adapted from [announcement to `opensuse-factory` mailing list](https://lists.opensuse.org/opensuse-factory/2018-09/msg00075.html):

Tumbleweed Snapshots, fixed repositories containing previously released versions of Tumbleweed, are now officially hosted on [download.opensuse.org](http://download.opensuse.org/history/)! For those not familiar with the Tumbleweed Snapshots concept please see the [short introduction video](https://www.youtube.com/watch?v=CSXRreUjiIc) and the [presentation I gave at oSC18](https://www.youtube.com/watch?v=CRszp1p47BM). The overall concept is to allow Tumbleweed to be consumed as short-lived distributions that can be utilized after a new snapshot is published. The two primary benefits are not having to update just to install new packages and being able to sit on an older snapshot when avoiding problematic package updates. The latter working best with snapper rollbacks as it allows for normal system operation after rolling back.

<iframe type="text/html" width="640" height="360"
  src="https://www.youtube.com/embed/CSXRreUjiIc"
  frameborder="0" allowfullscreen="allowfullscreen"></iframe>

Given the proper method for updating between snapshots is `zypper dup` instead of `zypper up` which is normally reserved for updating between major releases of the distribution, like _Leap_, keeping the previous repositories and switching between them with the use of `dup` is quite natural. Given the latest snapshot is accessible and `zypper` operates normally there are no down-sides to this approach while providing some important advantages. If you have not already tried them I would encourage you to do so. Many users are already updating _Tumbleweed_ once a week and thus perfectly align with the design of snapshots.

The installation step and basic usage is documented in the [`tumbleweed-cli` README](https://github.com/boombatower/tumbleweed-cli). No additional resources are utilized on the local machine. If for whatever reason you want to go back just run `tumbleweed uninit` to restore the default repository setup.

If you are already using Tumbleweed Snapshots and would like to switch to the official hosting a migration command is provided in `tumbleweed-cli` `0.3.0` which will be [included in Tumbleweed shortly](https://build.opensuse.org/request/show/636472). As a side-note, bash completion is also provided!

Once running version `0.3.0` (run `tumbleweed --version` to see which is installed), simply run `tumbleweed migrate`. Keep in mind the official hosting only provides `10` snapshots while my personal hosting on S3 provides `50`. The count will hopefully be increased in the future, but given how long it has taken to get this far it may be some time. Additionally, my AWS hosting has a CDN setup and will likely remain faster until mirrors decide to host the snapshots. Based on feedback and how things progress I will decide when to stop hosting on AWS, but it will be announced and the `tumbleweed-cli` will be changed to auto-migrate.

For those interested, the difference in storage usage can be compared on [metrics.opensuse.org](https://metrics.opensuse.org).

- [Official hosting](https://metrics.opensuse.org/d/osrt_history/osrt-history)
- [Un-official hosting](https://metrics.opensuse.org/d/osrt_tumbleweed_snapshots/tumbleweed-snapshots)

Making Tumbleweed Snapshots the default is likely worth considering once everything settles and an appropriate level of adoption by mirrors is reached.

It is encouraging to see the enthusiastic discussions about Tumbleweed Snapshots in IRC and e-mails.

Enjoy!
