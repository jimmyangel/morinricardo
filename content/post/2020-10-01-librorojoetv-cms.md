---
title: "Venezuelan Red List of Ecosystems content management"
tags:
  - netlifycms
  - netlify
  - vue
  - gridsome
  - provita
date: 2020-10-01T00:00:00.000Z
---

Back in [September](https://morinricardo.com/post/2020-09-10-librorojoetv-deploy/), I described the deployment details of the Venezuelan Red List of Ecosystems site [(Libro Rojo de los Ecosistemas Terrestres de Venezuela)](https://ecosistemasamenazados.org/).

In this post, I discuss the approach I used for content management.

<!--more-->

## Content management

In my [March post](https://morinricardo.com/post/2020-03-23-librorojoetv/#content-organization), I described how the site content is organized. Recall that it essentially consists of collections of markdown files and their associated images, organized in a directory structure under ```/content```. Upon issuing the ```npm run build``` command, [Gridsome's source plugins](https://gridsome.org/docs/fetching-data/#import-with-source-plugins) ingest the data and make it available in [GraphQL](https://gridsome.org/docs/data-layer/) for use in pre-rendered [Vue](https://vuejs.org/) pages and components.

So, content management, in the context of this application, is simply providing a friendly tool that allows admin users to edit the markdown files stored in the repository, while implementing a set of programmed rules that enforce data formats and data integrity across related content.

A developer could still go ahead and edit the markdown files directly using a plain old text editor, but it needs to be done carefully as to avoid introducing data validity, consistency and integrity issues, that could make the application bomb out with errors.

So how to provide this capability without having to write an entire suite of data editing functionality?

Can it be done for free?

## Netlify CMS

Enter [Netlify CMS](https://www.netlifycms.org/), brought to you by the [Netlify](https://www.netlify.com/) team!

Netlify CMS is a **free and open source** content management system that provides configurable data entry/editing of markdown files hosted on Git based back-end services such as Github, Gitlab or Bitbucket. In our case, we use [Github](https://github.com/jimmyangel/librorojoetv).

Even though it is not strictly required to use Netlify with the Netlify CMS, it works really well with it, mainly because it already has the hooks needed to integrate with their identity system. This is super important becase an identity system is essential to ensure that only  authorized users can edit the site's content. The free tier of Netlify allows up to five admin users that could be granted access to edit the content. For a small site like this, five admin users is more than enough.

## Installing

The simplest way to get Netlify CMS going in the site is to copy their [```index.html```](https://github.com/jimmyangel/librorojoetv/blob/master/static/admin/index.html) to the ```static/admin``` directory of the site's source (Gridsome will deploy these items, untouched, to the /admin uri of the site). The ```index.html``` file pulls the Netlify CMS application and the identity widget (for sign on) into an admin page. In addition to the admin application, we need to include a [YAML](https://en.wikipedia.org/wiki/YAML) configuration file [```(config.yml)```](https://github.com/jimmyangel/librorojoetv/blob/master/static/admin/config.yml) that defines the editing rules to be used by the content editor.

## Configuring

The configuration file has two major parts: 1) a set of site-level configuration items and general behavior of the CMS, and 2), a "collections" section, which is used to define content-specific rules for each content type (or content bundle, as defined in my [earlier post](https://morinricardo.com/post/2020-03-23-librorojoetv/#content-organization)).


### Site level configuration

```yml
backend:
  name: git-gateway
  branch: master
  squash_merges: true

locale: "es"

publish_mode: editorial_workflow
show_preview_links: false

media_folder: "static/images/uploads"
public_folder: "/images/uploads"

site_url: https://ecosistemasamenazados.org

...
```

The configuration items above should be pretty self-explanatory, and are well described in the [Netlify CMS documentation](https://www.netlifycms.org/docs/configuration-options/). However, I would like to highlight the ```publish_mode: editorial_workflow``` setting, because it is **required** in order to be able to store  images associated with the markdown content relative to the directory location of the markdown file. Without this setting, all site images would have to be stored in a gigantic directory defined in the ```media_folder``` setting, which is not very user-friendly, especially if you have hundreds or perhaps thousands of images to maintain.

### Content specific configuration

Content-specific configurations are defined as entries under the ```collections``` section of the ```config.yml``` file.

Each entry defines a collection of markdown files organized under sub-folders and following a common data structure.

Let's use an example to walk though how this works.

Starting with the actual site, the menu option used to show ```methods``` (Métodos), shows a list of pages that have different and unique content (i.e., Libros rojos, Clasificación del riesgo, Evaluación del riesgo, Prioridades de conservación) but they follow a common content pattern:

#### Page: metodos/librosrojos, and open menu ####

This shows how the page looks on the site.

<p align="center">
  <img src="/images/uploads/librorojo-cms-1.png"/>
</p>

#### Associated CMS editing page

This shows how the same content above is viewed in the CMS.

<p align="center">
  <img src="/images/uploads/librorojo-cms-2.png"/>
</p>


#### Associated markdown (edited by using the UI above)

This shows how the same content above stored in the repository.

```yml
---
id: librosrojos
method: clasificacion
title: Los libros rojos y su uso en la conservación de especies y ambientes amenazados
authors: Jon Paul Rodríguez, Kathryn M. Rodríguez-Clark, Marianne Assmüssen
citationPre: "Rodríguez J.P., K.M. Rodríguez-Clark & M. Assmüssen (2010).
  Categorías y criterios de las listas rojas de ecosistemas. Los libros rojos y
  su uso en la conservación de especies y ambientes amenazados. Pp: 93-96. En:
  J.P. Rodríguez, F. Rojas-Suárez & D. Giraldo Hernández (eds.)."
citationPost: "Pp: 93-96"
cardimage: ./pancarta-redbooks.jpg
cardimagecaption: Atardecer en el Llano, estado Apure. <i>Rebecca Miller</i>
photos:
  - photokey: Foto 1
    photourl: ./foto-1.jpg
    photocaption: "  "
---
Las listas rojas de especies amenazadas de extinción han sido fundamentales  ...

```

#### Associated Netlify CMS collection configuration

This shows how the CMS is configured to define the UI and the structure of the markdown above.

```yml
- name: "methods"
  label: "Métodos"
  editor:
     preview: false
  folder: "content/methods"
  path: "{{slug}}/content"
  create: false
  delete: false
  slug: "{{method}}"
  media_folder: ''
  fields:
    - {label: "Id", name: "id", widget: "hidden", required: true}
    - {label: "Método", name: "method", widget: "hidden", required: true}
    - {label: "Título", name: "title", widget: "string", required: true}
    - {label: "Autores", name: "authors", widget: "string"}
    - {label: "Ante-cita", name: "citationPre", widget: "string"}
    - {label: "Post-cita", name: "citationPost", widget: "string", required: false, default: ""}
    - {label: "Imagen de portada", name: "cardimage", widget: "file" , allow_multiple: false, required: true}
    - {label: "Subtítulo de la imagen de portada", name: "cardimagecaption", widget: "string", required: true}
    - {label: "Mostrar nota de evaluación de riesgo?", name: "showRiskEvalNote", widget: "boolean", required: false}
    - {label: "Imagenes", name: "photos", widget: "list", fields: [
        {label: "Etiqueta", name: "photokey", widget: "string", required: true},
        {label: "Url", name: "photourl", widget: "file" , allow_multiple: false, required: true},
        {label: "Centrar la imagen?", name: "isCentered", widget: "boolean", required: false},
        {label: "Subtítulo", name: "photocaption", widget: "string", required: true}]}
    - {label: "Texto", name: "body", widget: "text"}
```

After markdown files are saved and published, the pushed repository changes will trigger an automatic build/deploy cycle in Netlify, which after a couple of minutes, will cause the content to become available to the public.

For this site, there are a total of eight collections defined. Some of them, like the collection of ```/fichas``` (```vcards```), are a little bit involved. That is because they include a ton of structured content defined in the front matter section of the markdown.

And that's basically it!

Between this post and my previous three, I believe I have described the design and implementation of the Venezuelan Red List of Ecosystems site [(Libro Rojo de los Ecosistemas Terrestres de Venezuela)](https://ecosistemasamenazados.org/) site in a fair amount of detail. For the gory details, the code and the content are publicly available at the [Github repo](https://github.com/jimmyangel/librorojoetv).

---

In an upcoming post, I will introduce a brand new and exciting project I have been working on for the past few weeks! Stay tuned!

Au revoir!
