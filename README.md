# Data Platform Engineering Handbook

You can view rendered version at [https://handbook.dtone.engineering](https://handbook.dtone.engineering). Be aware that it is available to the public.

## Contributing

**Everybody** is encouraged to contribute. If you want to improve single sentence, paragraph or add a whole new chapter, please open a [merge request](https://git.dtone.xyz/office/handbook/merge_requests). Handbook is meant to be a living document which is shared across the entire organization. This can be even done in the browser through [Gitlab WebIDE](https://git.dtone.xyz/-/ide/project/office/handbook/edit/master/-/). If you want to keep in touch check [changelog](https://handbook.dtone.engineering/changelog.html). To be involved in building handbook consider subscribing to the repository and engaging in reviews of new changes -- Click the little bell icon on [handbook's project page](https://git.dtone.xyz/office/handbook), select custom and check _New merge request_.

For tiny changes that do not change meaning, please add `no changelog` label to the merge request.

## Technical details around how Handbook is built

### Building handbook locally

We are using [Sphinx](https://www.sphinx-doc.org) for managing the whole document. You will need `python3` and `virtualenv` installed to build it locally. When installed, rest is just question of

```bash
make html
```

this will built a fresh static version of the site, which you can view at your browser. When pushed to `master` (please don't do it without proper MR), CI/CD pipeline will build it and publish it as a GitLab page at [https://handbook.dtone.engineering](https://handbook.dtone.engineering).

You will need to set environment variable `GITLAB_PRIVATE_TOKEN` in case you would like to generate `changelog.md`. Used token will need access to repository merge-requests (at least `Reporter` role).

### Enabled extensions

#### `recommonmark` -- Using Markdown

The standard markup used by Sphinx is called [reStructuredText](https://www.sphinx-doc.org/en/master/usage/restructuredtext/index.html) (it uses file extension `.rst`). It is very powerful, but a bit confusing for people who are used to [Markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).

We have enabled extension [recommonmark](https://recommonmark.readthedocs.io) which seamlessly integrates Markdown into Sphinx workflow. It even allows for [embedding and evaluating RST](https://recommonmark.readthedocs.io/en/latest/auto_structify.html#embed-restructuredtext), when you need something special.

#### Cloud Theme -- Extended tables

Came across really [nice extension](https://cloud-sptheme.readthedocs.io/en/latest/lib/cloud_sptheme.ext.table_styling.html) for extending ability to format tables.

### Theme

Current theme is [Sphinx Press](https://github.com/schettino72/sphinx_press_theme). Another quite usable theme seems to be one provided by [F5](https://github.com/f5devcentral/f5-sphinx-theme). It has a nice feature of having a `scrollspy` behaving side box with "What is on this page" TOC. Clearly designed for a long documentation. It will need more tuning though, so currently sticking with the `press`.

For F5 one want to add to `conf.py`:
```python
import f5_sphinx_theme        
html_theme = 'f5_sphinx_theme'
html_sidebars = {'**': ['searchbox.html', 'localtoc.html', 'globaltoc.html']}
```
and to `requirements.txt`
```
f5-sphinx-theme
```

If we would ever want to switch to `hugo`. There are is very nice themes:

  - my current favorite: https://github.com/tract-docs/tract
  - https://github.com/matcornic/hugo-theme-learn/
  - https://github.com/vjeantet/hugo-theme-docdock
  - https://github.com/linode/docs
