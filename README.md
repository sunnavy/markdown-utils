# Table of Contents

* [README](#readme)
* [Installation](#installation)
* [Usage](#usage)
  * [&#64;DATE(format)&#64;](#&#64;date(format)&#64;)
  * [&#64;GIT_USER&#64;](#&#64;git_user&#64;)
  * [&#64;GIT_EMAIL&#64;](#&#64;git_email&#64;)
* [Rendering](#rendering)
* [Credits](#credits)

__Updated 2018-03-24__ by Rob Lauer <rlauer6@comcast.net>

# README

A quick search regarding how to get a table of contents into my
markdown yieled only few hits or projects that seemed a little weighty
to me, so here's a quick 'n dirty Perl script with very few
dependencies that you might find useful.  See [Usage](#usage) for more
information.

# Installation

```
git clone https://github.com/rlauer6/markdown-utils.git
sudo ln -s $(pwd)/markdown-utils/md-utlils.pl /usr/bin/md-utils
```

[Back to TOC](#table-of-contents)

# Usage

1. Add &#64;TOC&#64; somewhere in your markdown
1. Insert the table of contents to your markdown
  ```
  cat README.md.in | md-utils.pl > README.md
  ```

...or...kick it old school with a `Makefile`

```
FILES = \
    README.md.in

MARKDOWN=$(FILES:.md.in=.md)
HTML=$(MARKDOWN:.md=.html)

$(MARKDOWN): % : %.in
	md-utils $< > $@

$(HTML): $(MARKDOWN)
	md-utils -r $< > $@

all: $(MARKDOWN) $(HTML)

markdown: $(MARKDOWN)

html: $(HTML)

clean:
	rm -f $(MARKDOWN) $(HTML)
```

...and then...

```
make all
```

## &#64;DATE(format)&#64;

Add the current date using a custom format.  Essentially calls the
Perl function `time2str`.  See `perldoc Date::Format`

Example:


&#64;`DATE(%Y-%m-%d)`&#64;

## &#64;GIT_USER&#64;
## &#64;GIT_EMAIL&#64;

If you've done something like:

```
git config --global user.name "Fred Flintstone"
git config --global user.email "fflintstone@bedrock.org"
```

...then you can expect to see those in your markdown, otherwise expect
nothing.

[Back to TOC](#table-of-contents)

# Rendering

Using the [GiHub rendering
API](https://developer.github.com/v3/markdown/), you can create HTML
pretty easily. So if you want to preview your markdown...you might try:

```
jq --slurp --raw-input '{"text": "\(.)", "mode": "markdown"}' < README.md | \
  curl -s --data @- https://api.github.com/markdown
```

__...but alas you might find that your internal links don't work in
that rendered HTML...__

Never fear...the `--render` option of this utility will go ahead and set that right for
you and munge the HTML so that internal links really work...or at
least they do for me.

```
md-utils --render README.md > README.html
```

[Back to TOC](#table-of-contents)

# Credits

Rob Lauer - <rlauer6@comcast.net>

[Back to TOC](#table-of-contents)
