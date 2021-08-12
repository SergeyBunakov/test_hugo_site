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

$ mv layouts/_default/single.html themes/basic/layouts/_
default/single.html

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

### Working with Data

// ================================================

// ================================================
