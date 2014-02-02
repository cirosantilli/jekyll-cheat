---
title: index title
layout: layout0
---

`site.markdown` = {{ site.markdown  }}

#TOC

Does not seem to be currently possible server side on any Github Pages supported Markdown engine.

So lets just JS it up:

<ul data-toc></ul>

#Intro

Jekyll is a static site generator that can use many markup interpreting engines, in particular `kramdown`.

The generated site is put under `_site` directory, which should be ignored.

The Liquid template engine is used. It is meant to be client facing safe and fast, and therefore limited by design.

What Jekyll does is to add many variables automatically to the templates and then possibly compile the result via some markdown format to make Liquid into a blog / website.

The main force behind Jekyll is that Github Pages hosts it for free.

#Markup

Markup is decided based on file extension.

You *must* use the triple slash metadata header for markdown to be interpreted on the index file.

The default markdown engine was Maruku, but the project was discontinued. Kramdown is recommended as replacement for Maruku.

Pandoc will not be making it to GH Pages anytime soon: <https://github.com/jekyll/jekyll/issues/1973>

#Pages

[Page 0](page0.html)
[Page 1](page1.html)
[dir/Page 0](dir/page0.html)

#Posts

{% for post in site.posts %}
- `post.title` = [{{ post.title }}]({{ post.url }})

    `post.url` = {{ post.url }}

    `post.date` = {{ post.date }}

    `post.categories` = {{ post.categories | array_to_sentence_string }}

    `post.tags` = {{ post.tags | array_to_sentence_string }}

    `post.excerpt` = `{ post.excerpt }`

    `post.content` = `{ post.content }`
{% endfor %}

`site.categories.category0` = {{ site.categories.category0 | array_to_sentence_string }}

`site.categories.tag0` = {{ site.categories.category0 | array_to_sentence_string }}

#Layout

There is no current way to specify a default layout, but there is a PR on its way: <https://github.com/jekyll/jekyll/pull/1527>

#Data

Works like an YAML text database.

{% for entry in site.data.data0 %}
- entry.key0 = {{ entry.key0 }}
- entry.key1 = {{ entry.key1 }}
{% endfor %}

Data in `_config.yml` (not reparsed by `--watch`, must rebuild):

`site.custom-key0` = site.custom-key0

`site.custom-key1` = site.custom-key1

#Image

Kramdown:

![image]({{ site.url }}assets/flower.jpg){:width="300"}

#Tags

There are extra tags added by Jekyll to Liquid.

##Code

To have syntax highlighting, you need the corresponding css file included: the highlighter only adds clases.

{% highlight ruby %}
def f
    1
end
{% endhighlight %}

{% highlight ruby linenos %}
def f
    1
end
{% endhighlight %}

Kramdown fenced code block TODO: how to syntax highlight (HTML classes not being added).

~~~ ruby
def f(x)
  x + 1
end
~~~

##Include

`include includes0.md key="val0"` =

{% include includes0.md key="val0" %}

`include includes1.md key="val0"` =

{% include includes1.md key="val1" %}

##Gist

`gist 8749681` =

{% gist 8749681 %}

`gist 8749681 0` =

{% gist 8749681 0.rb %}

##Gist

`post_url 2000-01-01-post0` = {% post_url 2000-01-01-post0 %}

#Filters

These are extra filters that Jekyll adds to Liquid:

`site.time` = {{ site.time }}

`site.time | date_to_string` = {{ site.time | date_to_string }}

#Math

The best possibility without manual pre push pre processing seems to be to:

- `markdown: kramdown` on the `_config.yml`
- MathJax javascript on the header

We am yet to find a server side option (MathML or images), specially one that will run on Github Pages.

Inline $$x^2$$ math.

Block:

$$
x^2
$$

$$x^2$$

<https://github.com/jekyll/jekyll/issues/1888> seems solved:

$$
\begin{align}
  m &= |r|\\
  n &= |h|\\
\end{align}
$$
