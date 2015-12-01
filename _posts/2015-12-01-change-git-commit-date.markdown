---
layout: post
title:  "Change Git Commit Date"
date:   2015-12-01 13:17:24 +0530
categories: git commit date
---

What happens if you're lazy (like me), and happen to only make that final commit
way past your boss's deadline? Well, you can manipulate the date!~ Before you
commit your code, simply add the following lines:
{% highlight bash %}
export GIT_AUTHOR_DATE="YYYY-MM-DDThh:mm:ss±hh:mm"
export GIT_COMMITTER_DATE="YYYY-MM-DDThh:mm:ss±hh:mm"
{% endhighlight %}

![Set Git date variables](/assets/images/2015/12/01/Screenshot 2015-12-01 13:21:15.png)

Now you can commit as usual.
{% highlight bash %}
git add --all
git commit
{% endhighlight %}

#### Revert back to wall clock time

Now that you're done with committing with your favourite [mahurat
(महुरत)][mahurat-meaning], you can come back to wall clock time by simply
closing the Terminal window; or if you're feeling a bit professional, by
unsetting the environment variables.
{% highlight bash %}
unset GIT_AUTHOR_DATE
unset GIT_COMMITTER_DATE
{% endhighlight %}


#### Wait! I've already committed with the "wrong" date

Don't fret. You can [amend a commit][git-amend-date] right off the prompt:
{% highlight bash %}
# Amend only AUTHOR_DATE. Careful, hash changes after amendment.
git commit --amend --date="YYYY-MM-DDThh:mm:ss±hh:mm" -C 28f2d7b81e037aa4fcdf45f6353cb7c2aa10e336
# Amend both COMMITTER_DATE and AUTHOR_DATE.
GIT_COMMITTER_DATE="YYYY-MM-DDThh:mm:ss±hh:mm" git commit --amend --date="YYYY-MM-DDThh:mm:ss±hh:mm" -C HEAD~2
{% endhighlight %}

![Amend date in the Terminal](/assets/images/2015/12/01/Screenshot 2015-12-01 15:05:51.png)

#### Why two dates?

> The author is the person who originally wrote the work, whereas the committer is the person who last applied the work.
―[Pro Git book][pro-git-dates]

You can read more about Git dates at [Alex's blog][alex-dates].



​

Thank you for scrolling to the bottom of this page! Do let me know
how it was.

*******

[mahurat-meaning]: https://www.oxforddictionaries.com/definition/english/mahurat
[git-amend-date]: https://stackoverflow.com/questions/454734/how-can-one-change-the-timestamp-of-an-old-commit-in-git/5017265#5017265
[pro-git-dates]: https://git-scm.com/book/ch2-3.html
[alex-dates]: http://alexpeattie.com/blog/working-with-dates-in-git/
