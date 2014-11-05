---
layout: post
title:  "What is the purpose of this blog?"
date:   2014-11-01 13:55:47
comments: true
---

How frequently do you update your lab notebook? Do you have a notebook for
working on the computer? What is the point of a lab notebook even? How dependent
are you on tools that utilize a graphical user interface (GUI)? If you were
asked to regenerate a figure from your last lab meeting, how long would it take?
We will be using blog posts as a way to maintain an electronic lab notebook in
conjunction with project-specific GitHub repositories for our bioinformatics
analyses.

### The Notebook
Technically speaking, a laboratory notebook is something of a legal document
that the PI of each lab owns. The PI depends on the laboratory notebook in cases
where there are concerns over methods and results. It is often seen as a safety
measure like keeping the receipts from the past 5 years in case you get an
audit. But these are all negative reasons to maintain a laboratory notebook and
don't explain why you - the researcher - should keep a good laboratory notebook.

Imagine you are the only investigator in the lab. Why would you want to maintain
a notebook? It's a document that contains the many fragments that you will
hopefully compile into a paper or a presentation. Traditional bound, paper and
pen notebooks are more dangerous than helpful for informatics projects and are
probably past their prime for use with wet-lab experiments. How much would you
have to be paid to write out, long hand, your commands and the output? That
would suck and even worse, it would be highly prone to transcription errors. The
beauty of a computational notebook is that the code, output, and text are all
together. Furthermore, it is possible to incorporate multimedia including
images, videos, plots, tables, links to raw data, etc. It becomes an
organizational tool that works for you.

### Collaboration
Perhaps the most undervalued role of the laboratory notebook is in
collaboration. Someone once said that your best collaborator is you... in six
months. I would add that the second most important collaborator is your PI now,
but especially once you leave the lab. As I mentioned above, the traditional
laboratory notebook is frequently viewed as a legal document that will spend
most of its life in storage. It is  essentially the end of the road. But that is
not the way science is supposed to work. That notebook should be a step towards
another project. By using a blog framework with a commenting engine, issue
tracking, pull requests, and code review your notebook and repository instantly
become collaborative.

### Mechanics
Each blog post should provide a record of your daily goals, what you
accomplished, what you learned, and how you went about it for each of your daily
objectives. These blog posts are comment enabled to allow for collaboration
between people in the lab and Pat. They should be pushed to your GitHub
repository at least once a day (preferably at the end of the day). People who
fail to update their blog notebook regularly will be publicly flogged.

We have started to create various templates for different types of blog posts in
the `_post_templates` folder. To start a new post, you will usually copy one of
those templates to the `_posts` or `_knitr` folders and rename it to be
`YYYY-MM-DD-post-title`. The elements of the date should be separated by dashes
and in place of spaces in the blog title put dashes as well. These posts are
written in markdown, which is a pretty easy to learn formatting method.
