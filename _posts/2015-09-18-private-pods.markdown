---
layout: post
title:  "Create Private Pods"
date:   2015-09-18 11:40:22
categories: jekyll update
---
Cocoapods website has a great [tutorial][privatePodCocoapodsTutorial] on how to create a private pod. However, I had to do a few extra steps that weren't described on that tutorial.

1 - The first step is to create a private repo that will contain all the
private pods. This means that instead of publishing a public pod in the
[CocoaPods specs][cocoaPodsSpecs], it will be published on the created private
repo. I created one in Github.

{% highlight bash %}
$ mkdir MySpecs.git
$ cd MySpecs.git
$ git init --bare
{% endhighlight %}

2 - Add the repo to the cocoaPods installation.

{% highlight bash %}
pod repo add my-specs git@github.com:adsantos/MySpecs.git
{% endhighlight %}

`my-specs` should now be available in the `~/.cocoapods/repos` folder.

{% highlight bash %}
$ cd ~/.cocoapods/repos/my-specs
$ pod repo lint .
{% endhighlight %}

3 - Go to the folder of the pod you want to create and define the podspec. Make
sure the pod is valid by running the following:


{% highlight bash %}
$ cd MyProject
$ pod spec lint Project.podspec
{% endhighlight %}

If everything is fine, go to step 4.
If not, you maybe you got a message like this one:

{% highlight bash %}
[!] The spec did not pass validation, due to 1 error and 1 warning.
{% endhighlight %}

Run the same command again, but with `--verbose` in order to get details
on what went wrong.
{% highlight bash %}
pod spec lint Project.podspec --verbose
{% endhighlight %}

You might be missing some mandatory fields and if that's the case, simply add
them.

If your pod is dependent on another private pod, you have to run the command
with `--sources`:
{% highlight bash %}
pod spec lint Project.podspec
--sources='https://github.com/adsantos/MySpecs.git,https://github.com/CocoaPods/Specs' --verbose
{% endhighlight %}

If there are dependencies on public pods, the url to `CocoaPods/Specs` has to be
included as well, like in the example above.

If there are any warnings you don't want to fix for any reason, then
add `--allow-warnings`:
{% highlight bash %}
$ cd MyProject
$ pod spec lint Project.podspec
--sources='https://github.com/adsantos/MySpecs.git,https://github.com/CocoaPods/Specs'
--verbose --allow-warnings
{% endhighlight %}

Another possible problem is that your Project repo might not have a tag with
the equivalent version you defined in your podspec. For example, the podspec
below requires the Project repo to have the tag `0.1.0`.

{% highlight bash %}
Pod::Spec.new do |s|
  s.name         = "Project"
  s.version      = "0.1.0"
  ..
end
{% endhighlight %}

If the tag isn't in place, then the message below is displayed.
{% highlight bash %}
warning: Could not find remote branch 0.1.0 to clone.
fatal: Remote branch 0.1.0 not found in upstream origin
{% endhighlight %}

4 - Finally, add the Podspec to the private repo.
{% highlight bash %}
$ cd MyProject
$ pod repo push my-specs Project.podspec
{% endhighlight %}

If you want to push a podspec that contains warnings, then add the
`--allow-warnings` flag.
{% highlight bash %}
$ cd MyProject
$ pod repo push my-specs Project.podspec --allow-warnings
{% endhighlight %}

[privatePodCocoapodsTutorial]: https://guides.cocoapods.org/making/private-cocoapods.html

[cocoaPodsSpecs]: https://github.com/CocoaPods/Specs.git
