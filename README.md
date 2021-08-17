# Chapter-01

### Kicking the Tires

```
$ brew install hugo

$ choco install -y hugo-extended

$ hugo version

$ hugo new site portfolio

$ cd portfolio $ hugo server

$ hugo new about.md

$ hugo new contact.md

$ hugo new resume.md

$ mkdir layouts/_default/

$ cp layouts/index.html layouts/_default/single.html

$ hugo server

$ hugo

$ rm -r public && hugo

$ hugo --cleanDestinationDir --minify
```

  ==============================  

# Chapter-02

## Building a Basic Theme

```
$ hugo new theme basic

$ mv layouts/index.html themes/basic/layouts/index.html

$ mv layouts/_default/single.html themes/basic/layouts/_default/single.html
```

  ==============================  

# Chapter-03

### Adding Content Sections

// ================================================
"archetypes/projects.md"

```
---
title: "{{ replace .Name "-" " " | title }}"
draft: false
---

 ![alt](https://via.placeholder.com/640x140) 

Description...

### Tech used

* item
* item
* item
```

// ================================================

```
$ hugo new projects/awesomeco.md

$ hugo new projects/jabberwocky.md
```

// ================================================

## Creating the List Layout

“themes/basic/layouts/_default/list.html”

```html
{{ define "main" }}
<h2>{{ .Title }}</h2>
  <ul>
	    {{ range .Pages }}
	      <li><a href="{{ .RelPermalink }}">{{ .Title }}</a></li>
	    {{ end }}
	  </ul>

{{ end }}
```

// ================================================

## Creating More Specific Layouts

```
$ mkdir themes/basic/layouts/projects/”
```

Then, create the file themes/basic/layouts/projects/single.html.

```html
{{ define "main" }}
<div class="project-container">
    <section class="project-list">
        <h2>Projects</h2>
    </section>
    <section class="project">
        <h2>{{ .Title }}</h2>
	      {{ .Content }}
    </section>
</div>
{{ end }}
```

* or a little different

```html
{{ define "main" }}
<div class="project-container">
    <section class="project-list">
        <h2>Projects</h2>
        <ul>
            {{ range (where .Site.RegularPages "Type" "in" "projects").ByDate.Reverse }}
            <li><a href="{{ .RelPermalink }}">{{ .Title }}</a></li>
            {{ end }}
        </ul>
    </section>
    <section class="project">
        <h2>{{ .Title }}</h2>
        {{ .Content }}
    </section>
</div>
{{ end }}
```

// ================================================

## Adding Content to List Pages

```
$ hugo new projects/_index.md
```

```
---
title: "Projects"
draft: false
---
```

Open themes/basic/layouts/_default/list.html and add code to include the content into the layout:

```html
<h2>{{ .Title }}</h2>

{{ .Content }}
```

// ================================================

## Customizing the Project List

Create the file themes/basic/layouts/projects/list.html.

```html
{{ define "main" }}
<h2>{{ .Title }}</h2>
{{ .Content }}
<section class="projects">
    {{ range .Pages }}
    <section class="project">
        <h3><a href="{{ .RelPermalink }}">{{ .Title }}</a></h3>
    </section>
{{ end }}
</section>
{{ end }}
```

// ================================================

# Chapter-04

## Working with Data

### “Using Site Configuration Data in Your Theme”

working_with_data/portfolio/config.toml

```
[params]
author = "Brian Hogan"
description = "My portfolio site"
```

“working_with_data/portfolio/themes/basic/layouts/partials/head.html”

```html
<meta name="author" content="{{ .Site.Params.author }}">
<link rel="stylesheet" href='{{ "css/style.css" | relURL }}'>

Below that, add one for the description:
<meta name="description" content="{{ .Site.Params.description }}">
```

// ================================================

### “Populating Page Content Using Data in Front Matter”

“working_with_data/portfolio/archetypes/projects.md”

```
---
title: "{{ replace .Name "-" " " | title }}"
draft: false image: //via.placeholder.com/640x150 alt_text: "{{ replace .Name "-" " " | title }} screenshot"
summary: "Summary of the {{ replace .Name "-" " " | title }} project"
tech_used:

- JavaScript
- CSS
- HTML
---

Description of the {{ replace .Name "-" " " | title }} project...
```

// ================================================ 

```
$ hugo new projects/linkitivity.md
```

“/Users/brianhogan/portfolio/content/projects/linkitivity.md"   created

```
---
title: "Linkitivity"
draft: false image: //via.placeholder.com/640x150 alt_text: "Linkitivity screenshot"
summary: "Summary of the Linkitivity project"
tech_used:

- JavaScript
- CSS
- HTML
---

Description of Linkitivity project...
```

// ================================================ 

“working_with_data/portfolio/content/projects/awesomeco.md”

```
---
title: "Awesomeco"
draft: false image: //via.placeholder.com/640x150 alt_text: "Awesomeco screenshot"
summary: "Summary of the Awesomeco project"
tech_used:

- JavaScript
- CSS
- HTML
---

Description of the Awesomeco project...
```

// ================================================

“working_with_data/portfolio/themes/basic/layouts/projects/single.html”   added

```html
<section class="project">
	  <h2>{{ .Title }}</h2>
	  {{ .Content }}
  <img alt="{{ .Params.alt_text }}" src="{{ .Params.image }}">
	  <h3>Tech used</h3>
  <ul>
	    {{ range .Params.tech_used }}
	      <li>{{ . }}</li>
	    {{ end }}
	  </ul>
</section>
```

// ================================================

“working_with_data/portfolio/themes/basic/layouts/projects/list.html” added

```html
<section class="projects">
{{ range .Pages }}
<section class="project">
	      <h3><a href="{{ .RelPermalink }}">{{ .Title }}</a></h3>
  <p>{{ .Summary }}</p>
</section>
  {{ end }}
</section>
{{ end }}
```

// ================================================

### Conditionally Displaying Data

“working_with_data/portfolio/themes/basic/layouts/partials/head.html” replace

```html
<meta name="description" content="{{- with .Page.Description -}}{{ . }}{{- else -}}{{ .Site.Params.description }}{{- end -}}">
```

// ================================================ 
“working_with_data/portfolio/content/projects/_index.md” add

```
---
title: "Projects"
draft: false 
description: A list of Serg's projects.
---
```

// ================================================

“working_with_data/portfolio/themes/basic/layouts/partials/head.html” replace

```html
<title>
{{- if .Page.IsHome -}}
{{ .Site.Title }}
{{- else -}}
{{ .Title }} – {{ .Site.Title }}
{{- end -}}
</title>
```

// ================================================

### Using Local Data Files

“working_with_data/portfolio/data/socialmedia.json” create

```
{ "accounts" : [
    {
        "name": "Twitter",
        "url": "https://twitter.com/bphogan"
    }, 
    {
        "name": "LinkedIn",
        "url": "https://linkedin.com/in/bphogan"
    },
  ]
}
```

// ================================================ 

“working_with_data/portfolio/layouts/_default/contact.html” create

```html
{{ define "main" }}
<h2>{{ .Title }}</h2>
{{ .Content }} {{ end }}
```

“working_with_data/portfolio/layouts/_default/contact.html” below the {{ .Content }} line, add this code:

```html
<h3>Social Media</h3>
<ul>
  {{ range .Site.Data.socialmedia.accounts }}
    <li><a href="{{ .url }}">{{ .name }}</a></li>
  {{ end }}
</ul>
```

"/portfolio/layouts/_default/contact.html” result

```html
{{ define "main" }}
<h2>{{ .Title }}</h2>
{{ .Content }}
<h3>Social Media</h3>
<ul>
{{ range .Site.Data.socialmedia.accounts }}
	<li><a href="{{ .url }}">{{ .name }}</a></li>
{{ end }}
</ul>
{{ end }}
```

// ================================================ 

“working_with_data/portfolio/content/contact.md” add

```
---
title: "Contact"
date: 2020-01-01T12:55:44-05:00 draft: false layout: contact
---
```

// ================================================

### “Pulling Data from Remote Sources”

"working_with_data/portfolio/config.toml" add

```
[params]
  author = "Your User Name"
  description = "My portfolio site"

gh_url = "https://api.github.com/users"
gh_user = "your-gh-user"
```

// ================================================ 

“working_with_data/portfolio/themes/basic/layouts/_default/opensource.html” create

```html
{{ define "main" }}
  <h2>{{ .Title }}</h2>
{{ .Content }} 
{{ end }}
```

// ================================================ 

“working_with_data/portfolio/themes/basic/layouts/_default/opensource.html” insert

```html
{{ define "main" }}
<h2>{{ .Title }}</h2>
{{ .Content }} 
{{ $url := printf "%s/%s/repos" .Site.Params.gh_url .Site.Params.gh_user }} 
{{ end }}
```

// ================================================ 

“working_with_data/portfolio/themes/basic/layouts/_default/opensource.html” add

```html
{{ define "main" }}
<h2>{{ .Title }}</h2>

{{ .Content }}
{{ $url := printf "%s/%s/repos" .Site.Params.gh_url .Site.Params.gh_user }}
{{ $repos := getJSON $url }}

  <section class="oss">
    {{ range $repos }}
      <article>
        <h3><a href="{{ .html_url }}">{{ .name }}</a></h3>
        <p>{{ .description }}</p>
      </article>
    {{ end }}
  </section>
{{ end }}
```

// ================================================

```
$ hugo new opensource.md
```


```
---
title: "Opensource"
date: 2021-08-13T18:30:50+03:00
draft: true
layout: opensource
---

My Open Source Software on GitHub:
```

// ================================================

“working_with_data/portfolio/themes/basic/static/css/style.css” add

```css
.oss {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

.oss article {
  border: 1px solid #ddd;
  box-shadow: 3px 3px 3px #ddd;
  margin: 0.5%;
  padding: 0.5%;
  width: 30%;
}
```

// ================================================

“working_with_data/portfolio/themes/basic/layouts/projects/list.html” add

```html
<section class="projects">
  <section class="project">
    {{ with .Site.GetPage "/opensource.md" }}
    <h3><a href="{{ .RelPermalink }}">{{ .Title }}</a></h3>
    <p>{{ .Summary }}</p>
    {{ end }}
  </section>
</section>
```

// ================================================

### Syndicating Content with RSS

“working_with_data/portfolio/themes/basic/layouts/partials/head.html” add

```html
<link rel="alternate" type="application/rss+xml" href="http://example.com/feed" >

{{ range .AlternativeOutputFormats -}}
  {{- $link := `<link rel="%s" type="%s" href="%s" title="%s">` -}}
  {{- $title := printf "%s - %s" $.Page.Title $.Site.Title -}}
  {{- if $.Page.IsHome -}}
  {{ $title = $.Site.Title }}”
  {{- end -}}
  {{ printf $link .Rel .MediaType.Type .Permalink $title | safeHTML }}
{{- end }}
```

// ================================================

“working_with_data/portfolio/themes/basic/layouts/projects/list.json” create

```
{
  "projects": [
	{{- range $index, $page := (where .Site.RegularPages "Type" "in" "projects") }}
      {{- if $index -}} , {{- end }}
	  {
        "url": {{ .Permalink | jsonify }},
        "title": {{ .Title | jsonify }}
	  }
    {{- end }}
  ]
}
```

// ================================================

“working_with_data/portfolio/content/projects/_index.md” edit

```
description: A list of Serg's projects.
outputs:
- HTML
- JSON
- RSS
```

“Save the file and then run hugo server. Visit http://localhost:1313/projects/index.json and you’ll see your file.”

// ================================================

# Chapter-05

## Adding a Blog

“blog/portfolio/archetypes/posts.md” create

```
---
title: "{{ "replace .Name "-" " " | title }}"
date: {{ .Date }}
draft: false
author: Serg Banderlogin
---
```

“Next, add some default content to this file. Any placeholder text will do, like Lorem Ipsum:”

```
Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
tempor incididunt ut labore et dolore magna aliqua.
<!--more-->
Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut
aliquip ex ea commodo consequat.
```

// ================================================

```
$ hugo new posts/first-post.md
```

“Visit http://localhost:1313/posts/”

// ================================================

## Creating the Post’s Layout

```
$ mkdir themes/basic/layouts/posts  
```

“blog/portfolio/themes/basic/layouts/posts/single.html”  create

```html
{{ define "main" }}
<article class="post">
  <header>
    <h2>{{ .Title }}</h2>
    <p>
        By {{ .Params.Author }}
    </p>
    <p>
        Posted {{ .Date.format "January 2.2006" }}
    </p>
    <p>
      Reading time: {{ math.Round (div (countwords .Content) 200.0) }} minutes.
    </p>
  </header>

  <section class="body">
    {{ .Content }}
  </section>
</article>
{{ end }}
```

// ================================================

Add this line to config.toml:  pygmentsUseClasses = true

```
$ hugo gen chromastyles --style=github &gt; syntax.css
```

// ================================================

## Organizing Content with Taxonomies

“blog/portfolio/archetypes/posts.md”  add

```
categories:
- Personal
- Thoughts
tags:
- software
- html
```

“blog/portfolio/content/posts/first-post.md”  added too

```
categories:
- Personal
- Thoughts
  tags:
- software
- html
```

“Visit http://localhost:1313/tags” <br>
“Visit https://localhost:1313/categories” 

// ================================================

“blog/portfolio/themes/basic/layouts/_default/tag.terms.html”  create

```html
{{ define "main" }}
  <h2>{{ .Title }}</h2>
    {{ .Content }}
    {{ range .Data.Terms.Alphabetical }}
    <p class="tag">
    <a href="{{ .Page.Permalink }}">{{ .Page.Title }}</a>
    <span class="count">({{ .Count }})</span>
    </p>
    {{ end }}
{{ end }}
```

// ================================================

```
$ hugo new tags/_index.md
```

“blog/portfolio/content/tags/_index.md”  add 

```
---
title: "Tags"
date: 2020-01-01T15:17:39-05:00
draft: false
---

These are the site's tags:
```

// ================================================

“theme/basic/layouts/_default/tags.html”  create

```html
{{ define "main" }}
<h2>{{ .Title }}</h2>
{{ .Content }}
<ul>
    {{ range .Pages }}
    <li><a href="{{ .RelPermalink }}">{{ .Title }}</a></li>
    {{ end }}
</ul>
{{ end }}
```

// ================================================

“blog/portfolio/themes/basic/layouts/posts/single.html”    added "<span>"

```html
{{ define "main" }}
<article class="post">
    <header>
        <h2>{{ .Title }}</h2>
        <p>
            By {{ .Params.Author }}
        </p>
        <p>
            Posted {{ .Date.Format "January 2.2006" }}
            <span class="tags">
                in
                {{ range .Params.tags }}
                <a class="tag" href="/tags/{{ . | urlize }}">{{ . }}</a>
                {{ end }}
            </span>
        </p>
        <p>
            Reading time: {{ math.Round (div (countwords .Content) 200.0) }} minutes.
        </p>
    </header>
    <section class="body">
        {{ .Content }}
    </section>
</article>
{{ end }}
```

// ================================================

“blog/portfolio/themes/basic/static/css/style.css”  add

```css
a.tag {
  background-color: #ddd;
  color: #333;
  display: inline-block;
  padding: 0.1em;
  font-size: 0.9em;
  text-decoration: none;
}
```

// ================================================

### Disabling Taxonomies

By default, Hugo generates category and tag pages for you. 
But if you have no interest in tags, 
you can configure your site to ignore then. 
In your "config.toml" file, add the following code:
By doing this, you’re redefining the taxonomies to exclude tags. 
If you wanted tags but not categories, you’d add this code instead:

```
[taxonomies]
  year = "year"
  month = "month"
  tag = "tags"
  category = "categories"
```

// ================================================

## Customizing the URL for Posts

"blog/portfolio/config.toml"    define a new rule that makes posts available under /posts/year/month/slug:

```
[permalinks]
  posts = "posts/:year/:month/:slug/"
  year = "/posts/:slug/"
  month = "/posts/:slug/"
```

“This gives you URLs like /posts/2020/01/first_post. ”

// ================================================

“blog/portfolio/archetypes/posts.md”  add

```
year: "{{ dateFormat "2006" .Date }}"
month: "{{ dateFormat "2006/01" .Date }}”
```

// ================================================

## “Customizing Blog List Pages”

“blog/portfolio/themes/basic/layouts/partials/post_summary.html”  create and add following code

```html
<article>
  <header>
    <h3>
      <a href="{{ .RelPermalink }}">{{ .Title }}</a>
    </h3>
    <time>
      {{ .Date | dateFormat "January" }}
      {{ .Date | dateFormat "2" }}
    </time>
  </header>
 {{ .Summary }}
</article>
```

“blog/portfolio/themes/basic/layouts/posts/list.html”   create

```html
{{ define "main" }}
  <h2>{{ .Title }}</h2>
  {{ range .Pages }}
    {{ partial "post_summary.html" . }}
  {{ end }}
{{ end }}
```
```
$ cd themes/basic/layouts
$ cp posts/list.html _default/year.html
$ cp posts/list.html _default/month.html
$ cd -
```

“blog/portfolio/themes/basic/layouts/partials/nav.html”   adding

```html
<nav>
  <a href="/">Home</a>
  <a href="/about">About</a>
  <a href="/posts">Blog</a>
  <a href="/projects">Projects</a>
  <a href="/resume">Resume</a>
  <a href="/contact">Contact</a>
</nav>
```

// ================================================
## Adding Pagination

```
$ hugo new posts/third-post.md
```

“blog/portfolio/themes/basic/layouts/posts/list_with_pagination.html”  create

```
{ define "main" }}
  <h2>{{ .Title }}</h2>
  {{ range (.Paginator 1).Pages }}
    {{ partial "post_summary.html" . }}
  {{ end }}
  {{ template "_internal/pagination.html" . }}
{{ end }}
```

“blog/portfolio/themes/basic/static/css/style.css”

```css
.pagination {
    display: flex;
    justify-content: space-between;
    list-style: none;
    margin: 1em auto;
    padding: 0;
}

.pagination > .page-item {
    border: 1px solid #ddd;
    flex: 1;
    text-align: center;
    width: 5em;
}

.pagination .page-link {
    display: block;
    color: #000;
    text-decoration: none;
}

.pagination > .page-item.active {
    background-color: #333;
}

.pagination > .page-item.active > .page-link {”
    color: #fff;
}

.pagination > .page-item.disabled > .page-link {
    color: #ddd;
}

@media only screen and (min-width: 768px) {
.pagination {
    width: 30%;
  }
}
```

Now that you know the pagination works, <br> 
open themes/basic/layouts/posts/list.html <br> 
and change the number of paginated results to 10 posts:

``` 
{{ range (.Paginator 10).Pages }}
```

Alternatively, since the default is 10, <br> 
change the range line back to the original code:

```
{{ range .Paginator.Pages }}
```


You can then control the number of results globally <br> 
in the site’s configuration by adding the Paginate field to config.toml:

```
Paginate = 10
```

// ================================================
## Adding Comments to Posts Using Disqus

blog/portfolio/config.toml

```
theme = "basic"
disqusShortname = "pp-hugo-demo"
```

blog/portfolio/themes/basic/layouts/posts/single.html

```html
<section class="body">
    {{ .Content }}
</section>
<section class="comments">
	    <h3>Comments</h3>”
   {{ template "_internal/disqus.html" . }}
</section>”
```

```
$ npx localtunnel -p 1313
```

blog/portfolio/content/posts/first-post.md

```
categories:
- Personal
- Thoughts
tags:
- software
- html
disableComments: true
```

blog/portfolio/themes/basic/layouts/posts/single.html

```html
<section class="comments">
  <h3>Comments</h3>
  {{ if .Params.disableComments }}
  <p>Comments are disabled for this post</p>
{{ else }}
{{ template "_internal/disqus.html" . }}
{{ end }}
```

// ================================================
## Displaying Related Content
// ================================================
