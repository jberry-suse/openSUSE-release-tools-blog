---
layout: post
title: "Announcing Tumbleweed Snapshots: Rolling With You"
categories: tumbleweed-snapshots
---

From [announcement to `opensuse-factory` mailing list](https://lists.opensuse.org/opensuse-factory/2017-11/msg00591.html):

Some of you may remember over a year ago when I proposed the concept of
Tumbleweed Snapshots [1] and later the prototype that I provided [2]. Today I
would like to announce hosting for the snapshots, that is not inside my house
:), and ready to use!

For those not familiar with the concept I will describe it in the form to which
it has evolved.

Tumbleweed, being a rolling distribution, is constantly changing and packages
are constantly being rebuilt against one another and updating requirements. As
such it becomes necessary to update even when undesirable. For example, one is
running snapshot 17 and the next day snapshot 18 contains a QT update that
rebuilt a large number of packages. When attempting to install an application
that depends on QT one is greeted with an ugly unresolveable error. It is then
necessary to run a full update, likely very large with many unrelated changes,
in order to simply install an application as would have been possible yesterday.

If a remote repository containing historical snapshots was available one could
simply install the application and perhaps the handful of new dependencies it
requires rather than having to update the entire system. This provides one with
the benefits of a rolling distribution without requiring the constant change. A
week later when a new kernel and DRM stack provides an exciting feature it is
still easy to update everything and be running the latest code, but the user is
not interrupted by having to update when it should not be necessary.

From another angle, the capabilities of rollback using snapper and btrfs are
widely advertised, but the cumbersome and rather unusable state in which a user
is left is not commonly discussed. If for example a kernel and/or network
manager update completely break network functionality for certain users they can
rollback, but then what. As they wait for a fix their installation falls further
behind and with that it becomes less and less likely that installing a new
package will function properly.

On a similar note, if one wanted to install debuginfo packages it is many times
impossible without first updating that application and with it many of its
dependencies.

All of these scenarios have occurred to either myself or friends or family.

These problems of course do not exist in fixed releases like Leap, but the
latter of course does not provide the latest sources. Tumbleweed Snapshots
provides the best of both worlds, the latest packages when you want them and the
one package you need in the middle of working on a project.

If you are interested in trying out the snapshots just update to any snapshot
past 20171120 like you would normally and then perform the following.

    zypper in tumbleweed-cli
    tumbleweed init

During `init` the original repo files are kept and can be restored by:

    tumbleweed uninit

If you are curious, run `zypper lr -U` before and after the init to see the
URLs change. Originally, this was done via direct manipulation of the repo
file, but was refactored to a libzypp config option [3] and later as vars.d/
by mlandres. The final implementation simply updates the text file
/etc/zypp/vars.d/snapshotVersion after changing OSS and NON-OSS repo URLS to
include the variable.

The latest snapshot will always redirect to the standard mirror network until it
is no longer the latest snapshot at which point it will be served directly. This
means that when performing rollbacks via snapper or simply waiting a few days
after updating the URL never needs to change and continues to work as expected.

All zypper operations are unaffected except that the target snapshot needs to be
changed in order to update, but that is made easy.

    tumbleweed switch

or to switch, refresh, and dup

    tumbleweed update

For full list of commands and options `tumbleweed --help` or see documentation
[4]. A short video demonstration is also available [5].

Lastly, if there are mirror administrators interested in hosting/mirror the
snapshots let me know. Using various tools for interacting with S3 it should be
easy to make a copy of the public bucket. For those interested, take a look at
the variations of the tool that creates the snapshots [6].

An aside, due to rsync.opensuse.org being regularly out-of-sync and not yet
having access to stage.opensuse.org I am relying on up-to-date mirrors that have
stage access and thus will be triggering the snapshots manually util that is
resolved. As such the latest snapshot may not always redirect immediately.

The reason for the delay was waiting for official hosting by openSUSE which has
seen no tangible progress after a year. As such I decided to just move forward
rather than needlessly delay this any longer. Depending on the interest it
should clear what sort of costs will be incurred on AWS.

I have further plans to provide more information about the snapshots and issues
pertaining to them to aid users in knowing when to update so stay tuned.

Enjoy!


- [1] https://lists.opensuse.org/opensuse-factory/2016-10/msg00591.html
- [2] https://lists.opensuse.org/opensuse-factory/2016-12/msg00025.html
- [3] https://github.com/openSUSE/libzypp/pull/68
- [4] https://github.com/boombatower/tumbleweed-cli
- [5] https://www.youtube.com/watch?v=RkDwGiS9Kcc
- [6] https://github.com/boombatower/tumbleweed-snapshot
