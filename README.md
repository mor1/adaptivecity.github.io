# adaptivecity.github.io

This is the Git repo for [our online web content](https://adaptivecity.github.io) so any content can be editted directly on the GitHub website. Simple changes can be carried out via the GitHub web UI. For more substantial edits of the general content it makes sense to clone the repo to your desktop:

```sh
git clone https://github.com/AdaptiveCity/adaptivecity.github.io
```

On your desktop you can edit any files with the editor of your choice, push the changes back to github and view your great work at <https://adaptivecity.github.io>. Create a new branch for anything more than a simple change, and merge your branch back into `main` when you're happy with it.

Once on your desktop, you can use a local installation of [Jekyll](https://jekyllrb.com/) to view the impact of your changes as you make the edits. Hints for that are below.

## Building the site
### Using Docker

The `Makefile` encodes some runes to use a supported [`jekyll` Docker image](https://github.com/jekyll/jekyll-docker) to build the site, and to serve it up locally for test.

```sh
: mort@greyjay:www$; make help
-- help:
  list targets

-- all: site


-- clean:
  remove build artefacts

-- site:
  build site

-- test:
  serve site for testing

-- drafts:
  serve site, including draft posts
```

The latter two targets (`test`, `drafts`), serve the site at <https://localhost:$(PORT?=8080)/>. Editing content files while the site is being served will cause Jekyll to rebuild the edited files.

## Common editing tasks

### Add a Person to the People page

Put a portrait image into directory `_people/images`. If you're shy, google images "anonymous portrait" and pick one of those...

In directory `_people/` create a new file `<INITIAL>_<yourname>.md` or `<INITIAL>_<yourname>.html` where `<INITIAL>` is purely used for lexical ordering E.g. I used `L_ijl20.md`. The `md` vs. `html` choice allows you to use either format for your content although either way the page needs to start with a Jekyll frontmatter setting required variables. `office`/`phone` are unused at the moment. See example:

```
---
name: Ian Lewis
office: FE11
phone: (3)31859
image: ijl20.jpg
homepage: https://www.cl.cam.ac.uk/~ijl20
---

Ian Lewis is Director of the Adaptive Cities Programme in the Computer Laboratory. His research interests
are the real-time collection and analysis of urban sensor data and the evolution of the intelligent
Future City. Research themes include sensor networks, intelligent sensor design, real-time processing,
prediction, planning and privacy. His PhD, from the Cambridge Computer Lab, was concerned with robust
distributed parallel AI.
```

### Add a Project to the Projects page

Same routine as for People, just different directories.

Reuse or put a portrait image into directory `_projects/images`.

In directory `_projects`, create a new file `<INITIAL>_<newproject>.md` or `<INITIAL>_<newproject>.hmtl` as with Person above, e.g.:

```
---
title: ACP Web
link: https://github.com/AdaptiveCity/acp_web
contact_name: Ian Lewis
contact_link: https://www.cl.cam.ac.uk/~ijl20
image: github.png
---

Provides the 'http' interface to the Adaptive City Platform supporting
visualization of 'historical' and 'real-time' data, as well as a traditional
'request-response` set of API's to the stored data.

The web platform works with
both data held in storage (including the most recent) **and** streaming data flowing
through the platform. The latter is delivered by ACP Server via websockets to the browser.
```

## `adaptivecity.github.io` file locations / directory structure

### Homepage

`/index.html`, which uses the main `default` template, see below.

### People

`/_people/` contains all the 'person' text files and images that populate the People page. See instructions above.

`/people/index.html` contains the People page content with a templating loop that embeds the 'person' files.

`/people/assets/images` is a symbolic link to `/_people/images` which was my convention so adding a person can be done entirely in the `_people` directory instead of scattering pieces around.

### Projects

`/_projects/` similarly contains all the 'project' text files and images for the Projects page

`/projects/index.html` contains the Projects page content with a templating loop that embeds the 'project' files.

`/projects/assets/images` is a symbolic link to `/_projects/images` which was my convention so adding a project can be done entirely in the `_projects` directory instead of scattering pieces around.

## Default template and includes

Each main page contains a Jekyll **frontmatter** delimited by two `---` lines which set variables the Jekyll page-build process will use. For the Home page this is currently:
```
---
layout: default
title: Home
---
```

`_layouts/default.html` is a template for a page in the 'Cambridge' style, currently used site-wide.

In an effort of supreme elegance, this `default` template currently contains:

```
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml" lang="en" xml:lang="en" class="no-js">
<head>
{% include default_head.html %}

</head>
<body class="campl-theme-5">

{% include srg_header.html %}

<!-- PAGE CONTENT -->
{{ content }}
<!-- END PAGE CONTENT -->

{% include srg_footer.html %}

</body>
</html>
```

The 'include' files are:

`_includes/default_head.html` - a HTML fragment representing the contents of the `<head>` element of the web page, hence pulling in all the Cambridge Project Light stylesheet/js crapola. This template also has a useful hook to embed an additional stylesheet for any page using this 'include' (including via the default template), i.e. if the frontmatter of a sub-page contains:

```
css: foo
```

then a link to a stylesheet `/assets/style/css/foo.css` will be loaded in the page header.

`_includes/srg_header.html` - stuff at the top of the `<body>` that draws the rather large Cambridge header.

`_includes/srg_footer.html` - similar stuff that the bottom of the `<body>` that draws the Cambridge footer.
