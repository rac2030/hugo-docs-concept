---
title: Lists of Content in Hugo
linktitle: List Page Templates
description: Lists have a specific meaning and usage in Hugo when it comes to rendering your site homepage, section page, taxonomy list, or taxonomy terms list.
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-02-01
categories: [templates]
tags: [lists,sections,rss,taxonomies,terms]
weight: 22
draft: false
aliases: [/templates/list/,/layout/indexes/]
toc: true
---

## What is a List Page Template?

A list page template is a template used to render multiple pieces of content in a single HTML page. The exception to this rule is the homepage, which is still a list but has its own [dedicated template][homepage].

Hugo uses the term *list* in its truest sense; i.e. a sequential arrangement of material, especially in alphabetical or numerical order. Hugo uses list templates on any output HTML page where content is traditionally listed:

* [Taxonomy terms pages][taxterms]
* [Taxonomy list pages][taxlists]
* [Section list pages][sectiontemps]
* [RSS][rss]

The idea of a list page comes from the [hierarchical mental model of the web][mentalmodel] and is best demonstrated visually:

![Image demonstrating a hierarchical website sitemap.](/images/site-hierarchy.svg)

## List Defaults

### Default Templates

Since section lists and taxonomy lists (N.B., *not* [taxonomy terms lists][taxterms]) are both *lists* with regards to their templates, both have the same terminating default of `_default/list.html` or `themes/<THEME>/layouts/_default/list.html` in their lookup order. In addition, both [section lists][sectiontemps] and [taxonomy lists][taxlists] have their own default list templates in `_default`:

#### Default Section Templates

1. `layouts/_default/section.html`
2. `layouts/_default/list.html`

#### Default Taxonomy List Templates

1. `layouts/_default/taxonomy.html`
2. `themes/<THEME>/layouts/_default/taxonomy.html`


## Example List Templates

### Section Template

This list template is used for [spf13.com](http://spf13.com/). It makes use of [partial templates][partials]. All examples use a [view](/templates/views/) called either "li" or "summary."

{{% code file="layouts/section/post.html" %}}
```html
{{ partial "header.html" . }}
{{ partial "subheader.html" . }}

<section id="main">
  <div>
   <h1 id="title">{{ .Title }}</h1>
        <ul id="list">
            {{ range .Data.Pages }}
                {{ .Render "li"}}
            {{ end }}
        </ul>
  </div>
</section>
{{ partial "footer.html" . }}
```
{{% /code %}}

### Taxonomy Template

{{% code file="layouts/_default/taxonomies.html" download="taxonomies.html" %}}
```html
{{ define "main" }}
<section id="main">
  <div>
   <h1 id="title">{{ .Title }}</h1>
    {{ range .Data.Pages }}
        {{ .Render "summary"}}
    {{ end }}
  </div>
</section>
{{ end }}
```
{{% /code %}}

## Ordering Content

Hugo lists render the content based on metadata provided in the [front matter](/content-management/front-matter/)..

Here are a variety of different ways you can order the content items in
your list templates:

### Default: Weight > Date

{{% code file="layouts/partials/order-default.html" %}}
```html
<ul class="pages">
    {{ range .Data.Pages }}
        <li>
            <h1><a href="{{ .Permalink }}">{{ .Title }}</a></h1>
            <time>{{ .Date.Format "Mon, Jan 2, 2006" }}</time>
        </li>
    {{ end }}
</ul>
```
{{% /code %}}

### By Weight

{{% code file="layouts/partials/by-weight.html" %}}
```html
{{ range .Data.Pages.ByWeight }}
    <li>
    <a href="{{ .Permalink }}">{{ .Title }}</a>
    <div class="meta">{{ .Date.Format "Mon, Jan 2, 2006" }}</div>
    </li>
{{ end }}
```
{{% /code %}}

### By Date

{{% code file="layouts/partials/by-date.html" %}}
```html
{{ range .Data.Pages.ByDate }}
    <li>
    <a href="{{ .Permalink }}">{{ .Title }}</a>
    <div class="meta">{{ .Date.Format "Mon, Jan 2, 2006" }}</div>
    </li>
{{ end }}
```
{{% /code %}}

### By Publish Date

{{% code file="layouts/partials/by-publish-date.html" %}}
```html
{{ range .Data.Pages.ByPublishDate }}
    <li>
    <a href="{{ .Permalink }}">{{ .Title }}</a>
    <div class="meta">{{ .PublishDate.Format "Mon, Jan 2, 2006" }}</div>
    </li>
{{ end }}
```
{{% /code %}}

### By Expiration Date

{{% code file="layouts/partials/by-expiry-date.html" %}}
```html
{{ range .Data.Pages.ByExpiryDate }}
    <li>
    <a href="{{ .Permalink }}">{{ .Title }}</a>
    <div class="meta">{{ .ExpiryDate.Format "Mon, Jan 2, 2006" }}</div>
    </li>
{{ end }}
```
{{% /code %}}

### By Last Modified Date

{{% code file="layouts/partials/by-last-mod.html" %}}
```html
{{ range .Data.Pages.ByLastmod }}
    <li>
    <a href="{{ .Permalink }}">{{ .Title }}</a>
    <div class="meta">{{ .Date.Format "Mon, Jan 2, 2006" }}</div>
    </li>
{{ end }}
```
{{% /code %}}

### By Length

{{% code file="layouts/partials/by-length.html" %}}
```html
{{ range .Data.Pages.ByLength }}
    <li>
    <a href="{{ .Permalink }}">{{ .Title }}</a>
    <div class="meta">{{ .Date.Format "Mon, Jan 2, 2006" }}</div>
    </li>
{{ end }}
```
{{% /code %}}


### By Title

{{% code file="layouts/partials/by-title.html" %}}
```html
{{ range .Data.Pages.ByTitle }}
    <li>
    <a href="{{ .Permalink }}">{{ .Title }}</a>
    <div class="meta">{{ .Date.Format "Mon, Jan 2, 2006" }}</div>
    </li>
{{ end }}
```
{{% /code %}}

### By Link Title

{{% code file="layouts/partials/by-link-title.html" %}}
```html
{{ range .Data.Pages.ByLinkTitle }}
    <li>
    <a href="{{ .Permalink }}">{{ .LinkTitle }}</a>
    <div class="meta">{{ .Date.Format "Mon, Jan 2, 2006" }}</div>
    </li>
{{ end }}
```
{{% /code %}}

### By Parameter

Order based on the specified front matter parameter. Content that does not have the specified front matter field  will use the site's `.Site.Params` default. If the parameter is not found at all in some entries, those entries will appear together at the end of the ordering.

The below example sorts a list of posts by their rating.

{{% code file="layouts/partials/by-rating.html" %}}
```html
{{ range (.Data.Pages.ByParam "rating") }}
  <!-- ... -->
{{ end }}
```
{{% /code %}}

If the front matter field of interest is nested beneath another field, you can
also get it:

{{% code file="layouts/partials/by-nested-param.html" %}}
```html
{{ range (.Date.Pages.ByParam "author.last_name") }}
  <!-- ... -->
{{ end }}
```
{{% /code %}}

### Reverse Order

Reversing order can be applied to any of the above methods. The following uses `ByDate` as an example:

{{% code file="layouts/partials/by-date-reverse.html" %}}
```html
{{ range .Data.Pages.ByDate.Reverse }}
<li>
<a href="{{ .Permalink }}">{{ .Title }}</a>
<div class="meta">{{ .Date.Format "Mon, Jan 2, 2006" }}</div>
</li>
{{ end }}
```
{{% /code %}}

## Grouping Content

Hugo provides some functions for grouping pages by Section, Type, Date, etc.

### By Page Field

{{% code file="layouts/partials/by-page-field.html" %}}
```html
{{ range .Data.Pages.GroupBy "Section" }}
<h3>{{ .Key }}</h3>
<ul>
    {{ range .Pages }}
    <li>
    <a href="{{ .Permalink }}">{{ .Title }}</a>
    <div class="meta">{{ .Date.Format "Mon, Jan 2, 2006" }}</div>
    </li>
    {{ end }}
</ul>
{{ end }}
```
{{% /code %}}

### By Page date

{{% code file="layouts/partials/by-page-date.html" %}}
```html
{{ range .Data.Pages.GroupByDate "2006-01" }}
<h3>{{ .Key }}</h3>
<ul>
    {{ range .Pages }}
    <li>
    <a href="{{ .Permalink }}">{{ .Title }}</a>
    <div class="meta">{{ .Date.Format "Mon, Jan 2, 2006" }}</div>
    </li>
    {{ end }}
</ul>
{{ end }}
```
{{% /code %}}

### By Page publish date

{{% code file="layouts/partials/by-page-publish-date.html" %}}
```html
{{ range .Data.Pages.GroupByPublishDate "2006-01" }}
<h3>{{ .Key }}</h3>
<ul>
    {{ range .Pages }}
    <li>
    <a href="{{ .Permalink }}">{{ .Title }}</a>
    <div class="meta">{{ .PublishDate.Format "Mon, Jan 2, 2006" }}</div>
    </li>
    {{ end }}
</ul>
{{ end }}
```
{{% /code %}}

### By Page Param

{{% code file="layouts/partials/by-page-param.html" %}}
```html
{{ range .Data.Pages.GroupByParam "param_key" }}
<h3>{{ .Key }}</h3>
<ul>
    {{ range .Pages }}
    <li>
    <a href="{{ .Permalink }}">{{ .Title }}</a>
    <div class="meta">{{ .Date.Format "Mon, Jan 2, 2006" }}</div>
    </li>
    {{ end }}
</ul>
{{ end }}
```
{{% /code %}}

### By Page Param in Date Format

{{% code file="layouts/partials/by-page-param-as-date.html" %}}
```html
{{ range .Data.Pages.GroupByParamDate "param_key" "2006-01" }}
<h3>{{ .Key }}</h3>
<ul>
    {{ range .Pages }}
    <li>
    <a href="{{ .Permalink }}">{{ .Title }}</a>
    <div class="meta">{{ .Date.Format "Mon, Jan 2, 2006" }}</div>
    </li>
    {{ end }}
</ul>
{{ end }}
```
{{% /code %}}

### Reversing Key Order

The ordering of the groups is performed by keys in alphanumeric order (A–Z, 1–100) and in reverse chronological order (newest first) for dates.

While these are logical defaults, they are not always the desired order. There are two different syntaxes to change the order, both of which work the same way. You can use your preferred syntax.

#### Reverse Method

```html
{{ range (.Data.Pages.GroupBy "Section").Reverse }}
```

```html
{{ range (.Data.Pages.GroupByDate "2006-01").Reverse }}
```


#### Providing the Alternate Direction

```html
{{ range .Data.Pages.GroupByDate "2006-01" "asc" }}
```

```html
{{ range .Data.Pages.GroupBy "Section" "desc" }}
```

### Ordering Within Groups

Because Grouping returns a `{{.Key}}` and a slice of pages, all of the ordering methods listed above are available.

In the following example, groups are ordered chronologically and then content
within each group is ordered alphabetically by title.

{{% code file="layouts/partials/by-group-by-page.html" %}}
```html
{{ range .Data.Pages.GroupByDate "2006-01" "asc" }}
<h3>{{ .Key }}</h3>
<ul>
    {{ range .Pages.ByTitle }}
    <li>
    <a href="{{ .Permalink }}">{{ .Title }}</a>
    <div class="meta">{{ .Date.Format "Mon, Jan 2, 2006" }}</div>
    </li>
    {{ end }}
</ul>
{{ end }}
```
{{% /code %}}

## Filtering and Limiting Lists

Sometimes you only want to list a subset of the available content. A common request is to only display “Posts” on the homepage. You can accomplish this with the `where` function.

### `where`

`where` works in a similar manner to the `where` keyword in SQL. It selects all elements of the array or slice that match the provided field and value. `where` takes three arguments:

1. `array` or a `slice of maps or structs`
2. `key` or `field name`
3. `match value`

{{% code file="layouts/_default/.html" %}}
```html
{{ range where .Data.Pages "Section" "post" }}
   {{ .Content }}
{{ end }}
```
{{% /code %}}

### `first`

`first` works in a similar manner to the [`limit` keyword in SQL][limitkeyword]. It reduces the array to only the `first N` elements. It takes the array and number of elements as input. `first` takes two arguments:

1. `array` or `slice of maps or structs`
2. `number of elements`

{{% code file="layout/_default/section.html" %}}
```html
{{ range first 10 .Data.Pages }}
  {{ .Render "summary" }}
{{ end }}
```
{{% /code %}}

### `first` and `where` Together

Using `first` and `where` together can be very powerful:

{{% code file="first-and-where-together.html" %}}
```html
{{ range first 5 (where .Data.Pages "Section" "post") }}
   {{ .Content }}
{{ end }}
```
{{% /code %}}


[directorystructure]: /getting-started/directory-structure/
[homepage]: /templates/homepage/
[homepage]: /templates/homepage/
[limitkeyword]: https://www.techonthenet.com/sql/select_limit.php
[mentalmodel]: http://webstyleguide.com/wsg3/3-information-architecture/3-site-structure.html
[pagevars]: /variables/page/
[partials]: /templates/partials/
[RSS 2.0]: http://cyber.law.harvard.edu/rss/rss.html "RSS 2.0 Specification"
[rss]: /templates/rss/
[sections]: /content-management/sections/
[sectiontemps]: /templates/section-templates/
[sitevars]: /variables/site/
[taxlists]: /templates/taxonomy-templates/#taxonomy-list-templates/
[taxterms]: /templates/taxonomy-templates/#taxonomy-terms-templates/
[taxvars]: /variables/taxonomy/
[views]: /templates/views/
