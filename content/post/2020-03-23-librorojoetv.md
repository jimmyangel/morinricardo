---
title: "Venezuelan Red List of Ecosystems (Libro Rojo de los Ecosistemas Terrestres de Venezuela)"
tags:
  - leaflet
  - vue
  - gridsome
  - bulma
  - buefy
  - netlify
  - provita
date: 2020-03-23T00:00:00.000Z
---
### New Project Results

Back in September, I got involved in a new project with the [Provita](http://www.provita.org.ve/) Non-Government Organization (NGO) to publish the content of the Venezuelan Red List of Ecosystems (Libro Rojo de los Ecosistemas de Venezuela) on the web.

Today, after a few months of teamwork, I am excited to share the results:

<!--more-->
<p align="center">
	<a href="https://ecosistemasamenazados.org/" ><img src="/images/uploads/librorojoetv-home.png"/></a>
</p>

The project sources can be found [here](https://github.com/jimmyangel/librorojoetv).

I would like to thank the Provita team for giving me this opportunity. I would also like to thank [Marlene Alvarado](https://twitter.com/maasucha) for contributing the graphic design of the site.

### Tools and frameworks

I ended up using exactly the tools and frameworks that I described in my [previous post](/post/2019-09-16-new-project-new-tools/), with the small addition of a few helper libraries and Vue components (listed at the [bottom of the post](post/2020-03-13-librorojoetv#third-party-components)).

|Tool|Purpose|
|-----------|-----------|
|[Vue](https://vuejs.org/)|Base development framework, components, HTML templating and Javascript|
|[Gridsome](https://gridsome.org/)|Content transformation, feed to Vue templates and static site pre-rendering|
|[Leaflet](https://leafletjs.com/)/[vue2&#8209;leaflet](https://vue2-leaflet.netlify.com/)|Interactive mapping|
|[Bulma](https://bulma.io/)/[Buefy](https://buefy.org/)|CSS, styling, look and feel|
|[Netlify](https://www.netlify.com/)|Hosting|
|[Netlify CMS](https://www.netlifycms.org/)|Content Management|

<style>
  table {
    margin: 0 auto;
  }
</style>

### Site organization

We organized the site to make it easy to browse, by grouping related content under three menu groups in the navigation bar: Vegetation, Methods, and Risk of Collapse, as shown on the diagram below. The circled numbers map to the book chapters that contain the source content.

<p align="center" class="is-embedded">
	<img src="/images/uploads/librorojoetv-sitemap.svg"/>
</p>

<style>
  .is-embedded {
    width: 100%;
  }
  @media only screen and (min-width: 1024px) {
    .is-embedded {
      margin-left: -18%;
      width: 136%;
    }
  }
</style>

The source data comes from the original [325 page PDF document](https://ecosistemasamenazados.org/files/libro_rojo_ecosistemas_terrestre.pdf) (63 MB). The text was copied/pasted into [markdown](https://en.wikipedia.org/wiki/Markdown) files, with structured fields in [yaml](https://en.wikipedia.org/wiki/YAML) front matter. Simple tables were re-created using markdown and complex tables were re-created using HTML embedded in the markdown. In addition, the original images were retrieved by Provita from their archive storage.

### Content organization

The content of the site is organized in page bundles. A bundle is a directory which contains one or more markdown files (.md) and a collection of media files (.jpg, .png, .svg) referenced in the markdown.

<p align="center" class="is-embedded">
	<img src="/images/uploads/content-bundle.svg"/>
</p>

Bundles are laid out in the directory structure shown below, for easy access and maintenance. Each leaf node corresponds with a content bundle.

<p align="center" class="is-embedded">
	<img src="/images/uploads/content-organization.svg"/>
</p>

We use the `@gridsome/source-filesystem` plug-in which, upon site build, transforms the content files into GraphQL entities that are available for use in Vue templates, pages, and components.

Let's walk through an example using one of the case bundles in a "collection" of cases. The subdirectory [content/cases/c1](https://github.com/jimmyangel/librorojoetv/tree/master/content/cases/c1) contains a single markdown file ([content.md](https://github.com/jimmyangel/librorojoetv/blob/master/content/cases/c1/content.md)) and a number of .jpg files referenced in the content for the page [/casos/c1](https://ecosistemasamenazados.org/casos/c1).

The Gridsome configuration file ([gridsome.config.js](https://github.com/jimmyangel/librorojoetv/blob/master/gridsome.config.js)) includes the following code which instructs Gridsome to take the ```content.md``` files, transform them into GraphQL objects, and associate them to the [CaseCard.vue](https://github.com/jimmyangel/librorojoetv/blob/master/src/templates/CaseCard.vue) template using the url path which includes the ```case``` field as "key":

```
{
  use: '@gridsome/source-filesystem',
  options: {
    path: 'content/cases/**/content.md',
    typeName: 'CaseCard',

    ...

  }
}

...

templates: {
  ...

  CaseCard: '/casos/:case'

  ...
}
```

The template ```CaseCard.vue``` is then used to build the pages based on the field `case` (case identifier). In the ```CaseCard.vue``` template we use a GraphQL page query to retrieve the data associated with the corresponding ```case```, as follows:

```
<page-query>
  query CaseCard ($path: String!) {
    caseCard: caseCard (path: $path) {
      title
      authors
      citationPre
      citationPost
      content
      cardimage
      ...
    }
  }
</page-query>
```

The data is then available for use in the ```CaseCard.vue``` template (e.g., ```$page.caseCard.title```) for all cases in a "collection."

For individual pages, i.e., pages that are not part of a "collection," such as the About page or the Contact page, the corresponding .vue pages simply issue a GraphQL query using a unique Id known to the page (e.g, the About page uses the GraphQL HomeData object with an Id of "home").

### Vue Components

I developed a number of Vue components to maximize reuse, eliminate redundancies and ensure consistency:

* **PageBanner** - Lays out the page hero banners in a consistent way throughout the site.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![](/images/uploads/librorojoetv-banner.png)

* **BannerInfo** - Displays the banner image caption information icon and the correspoding dropdown content.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![](/images/uploads/librorojoetv-bannerinfo.png)

* **InteractiveMap** - Displays a Leaflet map and its associated legend block.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![](/images/uploads/librorojoetv-map.png)

* **TextWithRefsAndPhotos** - Post-process markdown output to inject dropdown references, inline images, and image modals.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![](/images/uploads/librorojoetv-text.png)

* **References** - Displays list of references associated with the page.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![](/images/uploads/librorojoetv-ref.png)

* **SideBar** - Provides the open/close sidebar functionality used to navigate collections such as Lansdcape Units, Risk Cards and Case Studies.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![](/images/uploads/librorojoetv-side.png)

* **CollapsibleList** - Lays out hierarchical navigation lists that are placed inside of the sidebars with the capability of expanding/collapsing child items.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![](/images/uploads/librorojoetv-clist.png)

### Third Party Components

In addition to the base frameworks and tools, I incorporated a number of 3rd party components that provide useful functionality saving both effort and time:

|Component|Purpose|
|-----------|-----------|
|[Axios](https://github.com/axios/axios)|HTTP client used to access remote data (e.g., map geojson files)|
|[Clipboard](https://github.com/zenorocha/clipboard.js)|Used to copy content to the clipboard (e.g., citations)|
|[v-runtime-template](https://github.com/alexjoverm/v-runtime-template)|Adds the ability to use Vue components in a template generated at run time|
|[vue-backtotop](https://github.com/caiofsouza/vue-backtotop)|Sticky button used to scroll to the top of the page|
|[vue-compare-image](https://github.com/junkboy0315/vue-compare-image)|Used to compare two images using a slider; note: I contributed a Pull Request to this component to add image labels|
|[vue-social-sharing](https://github.com/nicolasbeauvais/vue-social-sharing)|Used to share links on Twitter and Facebook|

---

In future posts I will be discussing a variety of topics, including: CMS configuration, Netlify deployment, lessons learned, pros/cons of the frameworks chosen, and improvement opportunities.

For now, that's all folks!

# Stay home, stay healthy!
