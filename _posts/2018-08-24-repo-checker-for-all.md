---
layout: post
title: "Announcing repo-checker for all"
---

Adapted from [announcement to `opensuse-factory` mailing list](https://lists.opensuse.org/opensuse-factory/2018-08/msg00248.html):

Ever since the [deployment of the new _repository checker_](https://lists.opensuse.org/opensuse-factory/2017-08/msg00165.html), or `repo-checker` as you may be familiar, for _Factory_ last year there have been a variety of requests ([like this one opensuse-packaging](https://lists.opensuse.org/opensuse-packaging/2018-06/msg00022.html)) to [utilize the tool locally](https://github.com/openSUSE/openSUSE-release-tools/issues/1210). With the large amount of recent work done to handle arbitrary repository setups, instead of being tied to the staging workflow, this is now possible. This means _devel projects_, home projects, and `openSUSE:Maintenance` can also make use of the tool.

The tool is provided as an rpm package and is included in _Leap_ and _Tumbleweed_, but to make use of this recent work version `20180821.fa39e68` or later is needed. Currently, that is only available from `openSUSE:Tools`, but [will be in Tumbleweed](https://build.opensuse.org/request/show/630991) shortly. See the [`openSUSE-release-tools` README](https://github.com/openSUSE/openSUSE-release-tools#installation) for installation instructions. The desired package in this case is `openSUSE-release-tools-repo-checker`.

A project has been prepared with an intentionally uninstallable package for demonstration. The following command can be used to review a project and print installation issues detected.

```
$ osrt-repo-checker --debug --dry project_only home:jberry:repo-checker
```

```
[D] no main-repo defined for home:jberry:repo-checker
[D] found chain to openSUSE:Factory/snapshot via openSUSE_Tumbleweed
[I] checking home:jberry:repo-checker/openSUSE_Tumbleweed@9a77541[2]
[I] mirroring home:jberry:repo-checker/openSUSE_Tumbleweed/x86_64
[I] mirroring openSUSE:Factory/snapshot/x86_64
[I] install check: start (ignore:False, whitelist:0, parse:False, no_filter:False)
[I] install check: failed
9a77541
## openSUSE_Tumbleweed/x86_64

### [install check & file conflicts](/package/view_file/home:jberry:repo-checker/00Meta/repo_checker.openSUSE_Tumbleweed)

<pre>
can't install uninstallable-monster-17-5.1.x86_64:
  nothing provides uninstallable-monster-child needed by uninstallable-monster-17-5.1.x86_64
</pre>
```

Note that the tool automatically selected the `openSUSE_Tumbleweed` repository since it builds against `openSUSE:Factory/snapshot`. The tool will default to selecting the first repository chain that builds against the afore mentioned or `openSUSE:Factory/standard`, but can be configured to use any repository.

All OSRT tools can be configured either locally or _remotely_ via an OBS attribute with the local config taking priority. The local config is placed in the `osc` config file (either `~/.oscrc` or `~/.config/osc/oscrc` depending on your setup). Add a new section for the project in question (ex. `[home:jberry:repo-checker]`) and place the configuration in that section. The remote config is placed in the `OSRT:Config` attribute on the OBS project in question. For example, the [demonstration project has configured the architecture whitelist](https://build.opensuse.org/attribs/home:jberry:repo-checker). The format is the same for both locations.

To indicate the desired repository for review use the `main-repo` option. For example, one could set it as follows in the demonstration project.

```
main-repo = openSUSE_Leap_42.3
```

The benefit of the remote config is that it will apply to anyone using the tools instead of just your local run.

As mentioned above the list of architectures reviewed can also be controlled. For example, limiting to `x86_64` and `i586` can be done as follows.

```
repo_checker-arch-whitelist = x86_64 i586
```

There are several options available (see the code), but the only other one likely of interest is the no filter option (`repo_checker-no-filter`). The no filter option forces all problems to be included in the report instead of only those from the top layer in the repository stack. If one wanted to resolve [all the problems in `openSUSE:Factory`](https://build.opensuse.org/package/view_file/openSUSE:Factory:Staging/dashboard/repo_checker?expand=1) a project with such fixes could be created and reviewed with the no filter option set to `True` in order to see what problems remain.

Do note that a local cache of rpm headers will be created in `~/.cache/opensuse-repo-checker` which will take just over `2G` for `openSUSE:Factory/snapshot` for `x86_64` alone. You can delete the cache whenever, but be aware the disk space will be used.

Enjoy!
