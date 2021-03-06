# Gitlab intro

We have a common instance of Gitlab [git.dtone.xyz](https://git.dtone.xyz) to handle source code or rather all kinds of documents using git.

Gitlab itself has pretty [comprehensive user documentation](https://docs.gitlab.com/ce/user/index.html). Check it out. Gitlab guys are pushing new versions quite often but we try to keep our Gitlab installation quite [fresh](https://git.dtone.xyz/help) and safe.

Any other problems? Please contact [Gitlab Admins](mailto:gitlab-admins@dtone.com).

## What is it good for?

* Source code tracking and versioning
* But really any text (using Git) and even binary formats or big blobs (using [Git LFS](https://git-lfs.github.com/)).
* [Merge requests](merge-requests)
  * Very useful merge requests (MR) UI allows for effective discussion over changes.
  * Due to integrated Edit/IDE functionality, it's possible to make them directly from UI.
* CI/CD - essentially ability to run stuff when source changes, can be used to run:
  * code checks (linters)
  * tests of code (unit test, integration tests, ...)
  * building artifacts / libraries / docker images
  * even automatic deployments
  * check our [introduction video to Gitlab CI/CD](https://drive.google.com/file/d/1X_kESdwmQksc3KIj1uJC7lz9J5MUQoE0/view) if you want to know more
* Pages - auto-publish content from repository - e.g. our [handbook](https://handbook.dtone.engineering) from [office/handbook](https://git.dtone.xyz/office/handbook) repository.
* Docker registry - publishing built docker images

## Basic structure

Whole Gitlab is oriented around notion of groups. Projects are placed into groups. Access control is based on membership in the groups. Groups can have subgroups. Groups can be watched, mentioned in merge request discussions. Think of Gitlab groups as something like projects in Phabricator. People can be members/watchers of projects and repositories too.

There are some limitations and differences compared to Phabricator's approach with generic tags. Unfortunately repository can be in one and only one group (or rather path from top-level to subgroups) which takes away a bit of flexibility of projects in Phabricator. So you cannot classify project as both e.g. `python` and `data` across whole codebase. But that's Gitlab's design decision and we need to live with it somehow.

Currently all Gitlab repositories are automatically synchronized automatically into [Phabricator](https://phabricator.dtone.com/diffusion/), so if you need this flexibility for something, it can probably be managed there.

## Unused parts of Gitlab

Please do not use following parts of Gitlab. Some parts of Gitlab aren't fitting our needs well or integrate badly into our environment. We use [Phabricator](https://phabricator.dtone.com) instead.

* Issues - Use [Phabricator tickets](https://phabricator.dtone.com/) for bigger ones, for smaller ones, maybe `FIXME(jane.doe)` or `TODO(john.smith)` in the source is enough.

* Wiki - Use Phabricator's [Phriction](https://phabricator.dtone.com/w/) (please consider if project's `README.md` isn't enough or you want to draw bigger picture among more that single Gitlab project).

* Snippets - Use Phabricator's [Paste](https://phabricator.dtone.com/paste/)

Having the same thing on multiple places causes fragmentation, there's a higher chance people will miss the information they need. Especially for people that are not integral part of the development team using git like people managers or project managers who often need to get insights of whole team or particular effort across teams to hit that deadline, plan team's time or headcount or are just interested.

Unfortunately Gitlab is build around the notion that basic organization unit is project and group of projects, but many teams have multiple repositories and projects to take care of and it might happen they're across multiple groups.

Links to these objects are not global but contain groups and project names. If you need to mention them in other project links will break when project is moved or renamed etc... Gitlab tries hard to keep links working, but it's not
always possible.

Phabricator excels in this regard (link aren't changed or re-used and mentions are well integrated into tickets e.g. `{T1234}` `{P1}` or `[w/wikipage]` and thus easy to read without too much cognitive load).

During [git.dtone.xyz](https://git.dtone.xyz/) inception we were heavily investigating the possibility to use just one tool however found out that in some cases it's not possible or really cumbersome.

## Account and Access

[Gitlab doc](https://docs.gitlab.com/ce/user/profile/)

To login, you just need your DTOne Google account. Navigate to [git.dtone.xyz](https://git.dtone.xyz) and you will be automatically logged in or asked to login into your google account.

After logging in you can setup your account by clicking the weird icon in the top-right corner. You can

* edit your _status_
* browse your _personal page_
* tune various _settings_ regarding your account

You can also reach your profile settings directly as [git.dtone.xyz/profile](https://git.dtone.xyz/profile)

Projects are created as with _internal_ visibility by default. That means you'll be able to browse all or most of projects freely upholding our open culture ideals. You need to be at least __developer__ to create a [merge request](#merge-requests-mr) to enhance our codebase so by default you need to [fork](https://docs.gitlab.com/ce/workflow/forking_workflow.html#creating-a-fork) the project first. You can always ask to be included in any particular [group](https://git.dtone.xyz/explore/groups) (This is a shortcoming of gitlab and should be [addressed soon](https://phabricator.dtone.com/T3322).

The best way is to ask owners of those groups (select _members_ on a group homepage, for example check out [ACS group](https://git.dtone.xyz/groups/acs/-/group_members) or the [Administrators](mailto:gitlab-admins@dtone.com).

### Useful account settings

If you look at your [profile settings page](https://git.dtone.xyz/profile), there's a lot of tunables!

* Please setup your [profile picture](https://git.dtone.xyz/profile), so colleagues can quickly recognize all your awesome contributions! :)

* From time to time one needs to access Gitlab via API. Unless you're a heavy integrator, you'll probably only need API tokens to access docker image registry from your local machine.

## Creating projects

Ask [Gitlab admins](mailto:gitlab-admins@dtone.com) in case you need to add another project group.

In the future we'll move to an automated as-a-code solution handling Gitlab permissions. [T3322](https://phabricator.dtone.com/T3322). Currently we depend on sane defaults (_internal_ project visibility) and group membership.

### Naming

Please give a thought to project's name first. As DTOne isn't a small company and handles many projects across different domains, project names are important for communication. Give your project a descriptive name, but not too long, just few
words.

For projects to be easily found by others, or just to be a good sport, give your project the same name as project slug. (see e.g. [office](https://git.dtone.xyz/office) group). Using lowercase letters and numbers with underscores and hyphens make Gitlab look very neat and projects easy to find and reference and avoid slug conflicts. Projects are automatically synchronized to Phabricator, please avoid creating projects with same name in different groups.

If your project is to be used as a package in a certain language, match your project name with the package name; prepend with the language name and a hyphen if multiple projects with the same base name are expected or clarity needed (e.g. `python-examplelib`). Some languages (like Julia) have particular naming conventions - follow them (in the Julia case, e.g. `MyLibrary.jl`).

Fill in the short description if needed. On most pages, rendered description is just one liner. Try to keep it 60 - 80 characters long or put the important information in the first part. For longer, please use project's `README.md`.

### Visibility

Use _Internal_ visibility by default to allow for our open culture. There's a story that one of the hard engineering problems in Google revolving around concurrency issues in the code was solved by their Marketing team. If your project contains secrets you must absolutely hide, contact [#acs-fresco](https://app.slack.com/client/TH44CUB2M/CKJVCEC4E) on Slack. They might already have a better solution for handling secrets. This is a global default, please do not change it unless absolutely necessary.

### `README.md`

Oh, what's this project? I get the description and understand the general vibe, but where do I start working with it? You know this situation? Allow others to get involved with a thorough `README.md` for your project. It should contain all basic information like e.g. project's principles, organization, usage, configuration, requirements, tooling, installation or any gotchas.

### Personal projects

You can create personal project for experiments and random stuff. Feel free to set these project's visibility to _private_ in case you don't want to cooperate with others. Bear in mind that these are still accessible by server administrators so do not share your **personal** secrets there. Personal project is the default placement when you click _create project_ on Gitlab's HP. Project can be moved later to some global group.

You might also use [landfill group](https://git.dtone.xyz/landfill) for experimental stuff that might be useful or interesting to others in our company.