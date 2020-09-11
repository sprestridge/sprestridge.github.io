---
layout: post
title:  "Behold...Jekyll!"
date:   2017-02-14 09:43:23 -0500
categories: jekyll update
---
**Unbelievable**, but I got Jekyll installed and running locally. Now to see if I can push it to GitHub Pages. Something is still not right with my local installation as Bundler does not seem to work correctly. I had to run the server command as sudo:

```
sudo bundle exec jekyll serve
```

After experimenting for a bit and usng ctrl-c to stop, I tried to launch the server again w/ the same sudo command and got an error as follows:

```
Bundler could not find compatible versions for gem "jekyll":
  In snapshot (Gemfile.lock):
    jekyll (= 3.4.0)

  In Gemfile:
    github-pages was resolved to 120, which depends on
      jekyll (= 3.3.1)

    minima (~> 2.0) was resolved to 2.1.0, which depends on
      jekyll (~> 3.3)

    minima (~> 2.0) was resolved to 2.1.0, which depends on
      jekyll (~> 3.3)

Running `bundle update` will rebuild your snapshot from scratch, using only
the gems in your Gemfile, which may resolve the conflict.
```

In order to get around it I ran ```sudo bundle update``` and then ```sudo bundle exec jekyll serve``` again and it worked. Need to investigate.

Testing MathJax:

$$ y = mx + b $$


## Original Text Follows
You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Note to self, this is pretty useful => Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
