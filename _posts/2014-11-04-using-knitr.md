---
layout: post
title:  "knitr demonstration"
date:   2014-11-04
comments: true
output:
  html_document:
    keep_md: yes
---



You can generate blog posts using the knitr R package. Don't be fooled, even
though it is an R package, it allows one to run commands in Python and bash.
Before you get going in knitr, be sure to see the Resources page for tutorials
on how to use knitr.

To get going, a template file should be copied from
`_post_templates/YYYY-MM-DD-knitr-post-name.Rmd`, put into the `_knitr` folder,
and renamed appropriately. Be sure to change the YAML front matter including the
date and the title of the post. At this point there are two approaches you can
take: RStudio or the command line.

### RStudio
This approach has the appeal of developing in the "safe" RStudio environment. But it does come at a cost of being somewhat clunky down the road. Using the YAML material at the top of this page and the iniital code chunk, which has been hidden, you can easily press "Knit HTML" and viola `*.md` and `*.html` files are generated. For the blog we are really only interested in the `*.md` file. If you look at the top of this file when it's conveted to a `*.md` file you'll see this:

    # knitr demonstration
    2014-11-04  

You need to delete these two lines and re-insert the YAML header:

    ---
    layout: post
    title:  "knitr demonstration"
    date:   2014-11-04
    comments: true
    ---

Save the `*.md` file and move it to the `_posts` folder. Delete the `*.html` file that was created. This strategy will work for converting any `*.Rmd` file to the Jekyll-compatible `*.md` file that you need to build a blog post.


### Command line
The first time you ever compile a knitr document from the command line you will need to run the following commands:

    chmod +x render_post.R
    chmod +x render_post.sh
    pip install watchdog

Now you have three options for your workflow. The first is to bang out your knitr
document and then run:

    ./render_post.R 2014-11-04-using_knitr.Rmd

This will run the Rmd file and copy the markdown file into the `_posts` folder.
Alternatively, you could run:

    ./render_post.sh 2014-11-04-using_knitr.Rmd

If you already have the local version of the website loaded or you go ahead and
load it now, then whenever you hit refresh on your browser the page will be
updated.

----

Below is a minimal R markdown example that I've swiped from the [knitr GitHub site](https://github.com/yihui/knitr-examples/raw/master/001-minimal.Rmd) plus
some other goodies I've added to demonstrate that you can use bash and Python
commands within knitr documents.

----

# A minimal R Markdown example

A quote:

> Markdown is not LaTeX.


## code chunks

A _paragraph_ here. A code chunk below (remember the three backticks):


{% highlight r %}
1 + 1
{% endhighlight %}



{% highlight text %}
## [1] 2
{% endhighlight %}



{% highlight r %}
0.4 - 0.7 + 0.3  # what? it is not zero!
{% endhighlight %}



{% highlight text %}
## [1] 5.551e-17
{% endhighlight %}

## graphics

It is easy.


{% highlight r %}
plot(1:10)
{% endhighlight %}

<img src="../figs/unnamed-chunk-21.png" title="center" alt="center" style="display: block; margin: auto;" />

{% highlight r %}
hist(rnorm(1000))
{% endhighlight %}

<img src="../figs/unnamed-chunk-22.png" title="center" alt="center" style="display: block; margin: auto;" />

## inline code

Yes I know the value of pi is 3.1416, and 2 times pi is 6.2832.



## nested code chunks

You can write code within other elements, e.g. a list

1. foo is good
    
    {% highlight r %}
    strsplit("hello indented world", " ")[[1]]
    {% endhighlight %}
    
    
    
    {% highlight text %}
    ## [1] "hello"    "indented" "world"
    {% endhighlight %}
2. bar is better


## bash code chunks

{% highlight bash %}
ls *.Rmd
{% endhighlight %}




{% highlight text %}
## 2014-11-04-using-knitr.Rmd
## 2014-11-07-Heuristic-for-Logit-Models.Rmd
## 2014-11-07-Schloss-Fantasy-Football-Update.Rmd
{% endhighlight %}


## python code chunks

{% highlight python %}
print "Hello, World!"
{% endhighlight %}




{% highlight text %}
## Hello, World!
{% endhighlight %}

## perl code chunks

{% highlight python %}
print "Hello, World!\n"
{% endhighlight %}




{% highlight text %}
## Hello, World!
{% endhighlight %}


## conclusion

Nothing fancy. You are ready to go. When you become picky, go to the [knitr website](http://yihui.name/knitr/).

![knitr logo](http://yihui.name/knitr/images/knit-logo.png)
