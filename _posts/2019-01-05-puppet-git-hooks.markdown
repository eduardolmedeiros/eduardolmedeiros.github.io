---
layout: post
title: Puppet git hooks
date: '2019-01-05 19:51:58'
categories: puppet git hooks
---

> [Puppet git hooks](http://https://github.com/drwahl/puppet-git-hooks/) is a tool that helps and guide you during the development of puppet modules.

Basically, it allows you to do a lot of tests and validations of your code before to commit to your git repository.

Features:

- Puppet manifest syntax
- Puppet epp template syntax
- Erb template syntax
- Puppet-lint
- Rspec-puppet
- Yaml (hiera data) syntax
- r10k puppetfile syntax

Usage

In your git repository you can symlink the pre-commit file from this repository to the .git/hooks/pre-commit of your repository you want to implement this feature.

{% highlight shell %}
    $ ln -s /path/to/this/repo/puppet-git-hooks/pre-commit .git/hooks/pre-commit
{% endhighlight %}

For more details, please have a look on the [Puppet git hooks](http://https://github.com/drwahl/puppet-git-hooks/) .

Cheers!
