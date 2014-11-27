---
title: index title
layout: layout0
list:
- a: a0
  b: b0
- a: a1
  b: b2
---

`site.markdown` = {{ site.markdown  }}

Also includes Liquid specific information.

#TOC

Does not seem to be currently possible server side on any GitHub Pages supported Markdown engine.

So lets just JS it up:

<ul data-toc></ul>

#Intro

Jekyll is a static site generator that can use many markup interpreting engines, in particular `kramdown`, which is one of the best.

The generated site is put under `_site` directory, which should be ignored.

The Liquid template engine is used. It is meant to be client facing safe and fast, and therefore limited by design.

What Jekyll does is to add many variables automatically to the templates and then possibly compile the result via some markdown format to make Liquid into a blog / website.

The main force behind Jekyll is that GitHub Pages hosts it for free.

#Liquid

Good wiki docs: <https://github.com/Shopify/liquid/wiki/Liquid-for-Designers>

Set a variable to a string:

{% assign var = 'val' %}

var = {{ var }}

Int:

{% assign var = 1 %}

var = {{ var }}

List: seems not to have literals.

Workarounds:

- YAML front matter:

    {% for x in page.list %}
        {{ x.a }}
        {{ x.b }}
    {% endfor  %}

- split:

    {% assign var = 'a.b' | split:'.' %}

        var[0] = {{ var[0] }}
        var[1] = {{ var[1] }}

Fails:

{% assign var = ['a', 'b'] %}

    var[0] = {{ var[0] }}
    var[1] = {{ var[1] }}

##Filters

#Markup

Markup is decided based on file extension.

You *must* use the triple slash metadata header for markdown to be interpreted on the index file.

The default markdown engine was Maruku, but the project was discontinued. Kramdown is recommended as replacement for Maruku.

The default won't be changing too soon for backwards compatibility: <https://github.com/jekyll/jekyll/issues/126#issue/126/comment/125723>. Watch out in particular for the colon `:` on first line bug of Maruku.

Pandoc will not be making it to GitHub Pages anytime soon: <https://github.com/jekyll/jekyll/issues/1973>

#Pages

[Page 0](page0)
[Page 1](page1)
[dir/Page 0](dir/page0)
[submodule/](submodule/)

page.url = {{ page.url }}

#Posts

TODO make post URL work on GitHub pages (not using the site prefix)

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

To have syntax highlighting, you need the corresponding CSS file included: the highlighter only adds classes.

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

##Gist

`gist 8749681` =

{% gist 8749681 %}

`gist 8749681 0` =

{% gist 8749681 0.rb %}

##Include

`include includes0.md key="val0"` =

{% include includes0.md key="val0" %}

`include includes1.md key="val0"` =

{% include includes1.md key="val1" %}

##post_url

`post_url 2000-01-01-post0` = {% post_url 2000-01-01-post0 %}

#Filters

These are extra filters that Jekyll adds to Liquid:

`site.time` = {{ site.time }}

`site.time | date_to_string` = {{ site.time | date_to_string }}

#Math

The best possibility without manual pre push pre processing seems to be to:

- `markdown: kramdown` on the `_config.yml`
- MathJax JavaScript on the header

To our knowledge there is no server side option (MathML or images) that will run on GitHub Pages without local precompiling + adding output files to the Git repo.

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

#Symlinks

Pages only build it:

-   in `.nojekyls` mode

-   the symlink points into the repository: <https://help.github.com/articles/page-build-failed-symlink-does-not-exist-within-your-site-s-repository/>

    The following generate a build error:

    - `..`
    - `..\0`

    The following don't generate a build error but show 404:

    - `.git`
    - `.git/config`
    - `not-a-file`

    The following work:

    - `Gemfile`
    - `images`
    - `../images/Gemfile`

#nojekyll

To turn off Jekyll entirely, add a `.nojekyll` file to the top-level: <https://help.github.com/articles/using-jekyll-with-pages/#turning-jekyll-off>
