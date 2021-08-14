# Chapter-01

### Kicking the Tires

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

<!--  ==============================  -->

# Chapter-02

### Building a Basic Theme

$ hugo new theme basic

$ mv layouts/index.html themes/basic/layouts/index.html

$ mv layouts/_default/single.html themes/basic/layouts/_default/single.html

<!--  ==============================  -->

# Chapter-03

### Adding Content Sections

// ================================================
"archetypes/projects.md"

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

// ================================================

$ hugo new projects/awesomeco.md

$ hugo new projects/jabberwocky.md

// ================================================

## Creating the List Layout

“themes/basic/layouts/_default/list.html”

{{ define "main" }}
<h2>{{ .Title }}</h2>
  <ul>
	    {{ range .Pages }}
	      <li><a href="{{ .RelPermalink }}">{{ .Title }}</a></li>
	    {{ end }}
	  </ul>

{{ end }}

// ================================================

## Creating More Specific Layouts

$ mkdir themes/basic/layouts/projects/”

Then, create the file themes/basic/layouts/projects/single.html.

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

* or a little different

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

// ================================================

## Adding Content to List Pages

$ hugo new projects/_index.md

---
title: "Projects"
draft: false
---

Open themes/basic/layouts/_default/list.html and add code to include the content into the layout:

<h2>{{ .Title }}</h2>

{{ .Content }}

// ================================================

## Customizing the Project List

Create the file themes/basic/layouts/projects/list.html.

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

// ================================================

<!--  ==============================  -->

# Chapter-04

## Working with Data

// ================================================

### “Using Site Configuration Data in Your Theme”

working_with_data/portfolio/config.toml

[params]
author = "Brian Hogan"
description = "My portfolio site"

“working_with_data/portfolio/themes/basic/layouts/partials/head.html”

<meta name="author" content="{{ .Site.Params.author }}">
<link rel="stylesheet" href='{{ "css/style.css" | relURL }}'>

Below that, add one for the description:
<meta name="description" content="{{ .Site.Params.description }}">

// ================================================

### “Populating Page Content Using Data in Front Matter”

“working_with_data/portfolio/archetypes/projects.md”

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

// ================================================ 
$ hugo new projects/linkitivity.md
“/Users/brianhogan/portfolio/content/projects/linkitivity.md created”

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

// ================================================ 
“working_with_data/portfolio/content/projects/awesomeco.md”

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

// ================================================
“working_with_data/portfolio/themes/basic/layouts/projects/single.html” added

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

// ================================================
“working_with_data/portfolio/themes/basic/layouts/projects/list.html” added

<section class="projects">
{{ range .Pages }}
<section class="project">
	      <h3><a href="{{ .RelPermalink }}">{{ .Title }}</a></h3>
  <p>{{ .Summary }}</p>
</section>
  {{ end }}
</section>
{{ end }}

// ================================================

### Conditionally Displaying Data

“working_with_data/portfolio/themes/basic/layouts/partials/head.html” replace

<meta name="description" content="{{- with .Page.Description -}}{{ . }}{{- else -}}{{ .Site.Params.description }}{{- end -}}">

// ================================================ 
“working_with_data/portfolio/content/projects/_index.md” add

---
title: "Projects"
draft: false 
description: A list of Serg's projects.
---

// ================================================
“working_with_data/portfolio/themes/basic/layouts/partials/head.html” replace

<title>
{{- if .Page.IsHome -}}
{{ .Site.Title }}
{{- else -}}
{{ .Title }} – {{ .Site.Title }}
{{- end -}}
</title>

// ================================================

### Using Local Data Files

“working_with_data/portfolio/data/socialmedia.json” create

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

// ================================================ 
“working_with_data/portfolio/layouts/_default/contact.html” create

{{ define "main" }}
<h2>{{ .Title }}</h2>
{{ .Content }} {{ end }}

“working_with_data/portfolio/layouts/_default/contact.html” below the {{ .Content }} line, add this code:

<h3>Social Media</h3>
<ul>
  {{ range .Site.Data.socialmedia.accounts }}
    <li><a href="{{ .url }}">{{ .name }}</a></li>
  {{ end }}
</ul>

"/portfolio/layouts/_default/contact.html” result

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

// ================================================ 
“working_with_data/portfolio/content/contact.md” add

---
title: "Contact"
date: 2020-01-01T12:55:44-05:00 draft: false layout: contact
---

// ================================================

### “Pulling Data from Remote Sources”

"working_with_data/portfolio/config.toml" add

[params]
author = "Your User Name"
description = "My portfolio site"

gh_url = "https://api.github.com/users"
gh_user = "your-gh-user"

// ================================================ 
“working_with_data/portfolio/themes/basic/layouts/_default/opensource.html” create

{{ define "main" }}
  <h2>{{ .Title }}</h2>
{{ .Content }} 
{{ end }}

// ================================================ 
“working_with_data/portfolio/themes/basic/layouts/_default/opensource.html” insert

{{ define "main" }}
<h2>{{ .Title }}</h2>
{{ .Content }} 
{{ $url := printf "%s/%s/repos" .Site.Params.gh_url .Site.Params.gh_user }} 
{{ end }}

// ================================================ 
“working_with_data/portfolio/themes/basic/layouts/_default/opensource.html” add

{{ $repos := getJSON $url }}

“working_with_data/portfolio/themes/basic/layouts/_default/opensource.html”

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

// ================================================
$ hugo new opensource.md

---
title: "Opensource"
date: 2021-08-13T18:30:50+03:00
draft: true
layout: opensource
---

My Open Source Software on GitHub:

// ================================================
“working_with_data/portfolio/themes/basic/static/css/style.css” add

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

// ================================================
“working_with_data/portfolio/themes/basic/layouts/projects/list.html” add

<section class="projects">
  <section class="project">
    {{ with .Site.GetPage "/opensource.md" }}
    <h3><a href="{{ .RelPermalink }}">{{ .Title }}</a></h3>
    <p>{{ .Summary }}</p>
    {{ end }}
  </section>
</section>

// ================================================
### Syndicating Content with RSS

“working_with_data/portfolio/themes/basic/layouts/partials/head.html” add

<link rel="alternate" type="application/rss+xml" href="http://example.com/feed" >

{{ range .AlternativeOutputFormats -}}
  {{- $link := `<link rel="%s" type="%s" href="%s" title="%s">` -}}
  {{- $title := printf "%s - %s" $.Page.Title $.Site.Title -}}
  {{- if $.Page.IsHome -}}
  {{ $title = $.Site.Title }}”
  {{- end -}}
  {{ printf $link .Rel .MediaType.Type .Permalink $title | safeHTML }}
{{- end }}

// ================================================
“working_with_data/portfolio/themes/basic/layouts/projects/list.json” create

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

// ================================================
“working_with_data/portfolio/content/projects/_index.md” edit

description: A list of Serg's projects.
outputs:
- HTML
- JSON
- RSS

“Save the file and then run hugo server. Visit http://localhost:1313/projects/index.json and you’ll see your file.”

// ================================================
<!--  ==============================  -->
# Chapter-05

## Adding a Blog
// ================================================
