---
layout: post
title:  "Start a Blog with Github and Jekyll"
date:   2015-07-01 05:29:60 +0530
categories: jekyll update
---

Coming to Github, it was a pleasant treat to see they give you a cool
place to host your own static website, and an added bonus of Jekyll
if you're interested in blogs. So this blog.

Here's how i got up and running with the basic configuration:



#### 1. Create a GitHub repository

The repository name must be in the [exact following syntax][syntax-for-github-repo-name]:  
`username.github.io`

![GitHub repo name](/assets/images/2015/07/01/Screenshot 2015-07-01 01:27:57.png)


#### 2. Clone the repository onto your desktop.

You will need to have [Git installed][git-scm]. Open a Terminal, and
type in:
{% highlight bash %}
git clone https://github.com/username/username.github.io
{% endhighlight %}
![Clone the repository using git clone](/assets/images/2015/07/01/Screenshot 2015-07-01 01:32:35.png)

You might want to set up your user name and email, if you haven't
set it up globally.
{% highlight bash %}
git config user.name "Gulielmus Cartesius"
git config user.email "gulielmus.cartesius@jekyll+github.dev"
{% endhighlight %}
![Setting up your Git user name and email](/assets/images/2015/07/01/Screenshot 2015-07-01 01:36:32.png)

This is how your workspace will look like. In case you're wondering,
it appears empty, with two hidden items: a `.git` directory, and a
`.gitignore` file.
![GitHub repo workspace](/assets/images/2015/07/01/Screenshot 2015-07-01 01:41:05.png)


#### 3. Go Jekyll

Install Jekyll by [following the instructions][install-jekyll] on their website. Then, create the Jekyll blog in your workspace…
{% highlight bash %}
jekyll new username.github.io
{% endhighlight %}
![Create the Jekyll blog](/assets/images/2015/07/01/Screenshot 2015-07-01 02:19:28.png)

…serve it…
{% highlight bash %}
cd ~/username.github.io/
jekyll serve
{% endhighlight %}
![Serve the Jekyll blog](/assets/images/2015/07/01/Screenshot 2015-07-01 02:23:23.png)

…and navigate your browser to `http://127.0.0.1:4000`. See your
awesome title!
![Jekyll blog default preview](/assets/images/2015/07/01/Screenshot 2015-07-01 02:29:48.png
)

The default workspace looks like this:
![Jekyll blog workspace](/assets/images/2015/07/01/Screenshot 2015-07-01 02:26:54.png)


#### 4. Personalise your blog

1. Modify `_config.yml` to suit your blog details.
![Modify _config.yml](/assets/images/2015/07/01/Screenshot 2015-07-01 02:52:10.png)

2. Let others know who you are. It's in `about.md`.
![Modify about.md](/assets/images/2015/07/01/Screenshot 2015-07-01 03:22:39.png)

3. See Jekyll do the magic as soon as the changes to the file are
saved.
![Modified about.html](/assets/images/2015/07/01/Screenshot 2015-07-01 03:23:44.png)


#### 5. Now you got a blog. What next?

You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll’s dedicated Help repository][jekyll-help].


#### 5. Alright, now we need to post!

1. Head over to `_posts` directory, and add (actually, just rename)
the file like how Jekyll said so in the previous point 5.
![Rename the post file](/assets/images/2015/07/01/Screenshot 2015-07-01 05:29:60.png)

2. Type in your post. There won't be a screenshot for that!~


#### P.S. If you're a master of perfection…

…you might want to correct the date format. The two visible places
are in `_layouts/post.html` and `index.html`.
![Correct the date format](/assets/images/2015/07/01/Screenshot 2015-07-01 05:29:60​.png)


#### 6. Don't forget to push your changes to GitHub

{% highlight bash %}
cd ~/username.github.io/
git add --all
git commit
git push -u origin master
{% endhighlight %}

You can now view your blog at `https://username.github.io`.




​

Thank you for scrolling to the bottom of this page! Do let me know
how it was.

*******

[syntax-for-github-repo-name]: https://pages.github.com/#user-site
[git-scm]: https://git-scm.com/
[install-jekyll]: http://jekyllrb.com/docs/installation/

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
