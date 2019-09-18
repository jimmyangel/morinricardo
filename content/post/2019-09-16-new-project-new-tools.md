---
title: "New project, new tools! Yay!"
tags:
  - leaflet
  - vue
  - gridsome
  - bulma
  - buefy
  - netlify
date: 2019-09-16T00:00:00.000Z
---
### New Project

I am really excited about a new project that I am starting for [Provita](http://www.provita.org.ve/). Provita is an environmental Non-Government Organization (NGO) in Venezuela which has been working hard for decades for the conservation of endangered species and their habitats in my country of birth. I am honored to have been accepted as a volunteer in this great organization, and I am looking forward to contributing my efforts to the mission.

As soon as I have some results to show, I will be posting the details here. For the time being, I wanted to take this opportunity to  discuss the requirements of the project and how I went about selecting the development tools that I chose.

<!--more-->

Here are the requirements of the new project:

* **Web Application** - Accessible from standard, modern Web browsers

* **Content heavy** - Lots of formatted text, pictures and maps, including a mixture of highly structured data as well as a ton of free text with embedded images (e.g., chapters of books)

* **Infrequent updates** - Most updated will be about correcting content, refining the appearance of the application and adding features over time

* **Interactive maps** - Use maps and layers to access summary and detailed information

* **Hands-off maintenance** - After deployment, the site should be as easy as possible to keep up, regardless of the number of users hitting the site

* **Low cost deployment** - Preferably free!

### New Tools

While the application requirements would suggest building a static site, the team wanted to be able to expand the functionality of the site over time. Also, the requirement of incorporating interactive maps, makes the application a bit more dynamic than a purely static site. I have already gained some experience with static site generators: both this blog and [Hilde's web site](https://www.hildemorin.com/) are built using [Hugo](https://gohugo.io/). Hugo is great! It is blazingly fast, but when it comes to the dynamic aspects of the application, you are basically on your own. I also have experience building purely dynamic Single Page Applications (SPA) like [HOWL](https://oregonhowl.org/), [Logging in Oregon](https://logging.oregonhowl.org/) and [Rikitraki](https://www.rikitraki.com/), but not having any development framework other than [JQuery](https://jquery.com/), I had to deal with modularization, state management, routing, and [DOM](https://en.wikipedia.org/wiki/Document_Object_Model) updates on my own.

For this project, I want to start with a nice framework that takes care of a lot of these details for me (and some more). I want a framework that is easy to use, and that incorporates the latest thinking in modularization, reusability and extensibility.

At the same time, I want the advantages of pre-rendering pages: easy and cost effective deployment, extremely low maintenance (no databases or servers to babysit!), high performance and scalability ([learn more about the JAMStack](https://jamstack.org/)). Since this site will have large parts that consist of static content, it lends itself very well to the static site generation approach. But, is it possible to combine the best of both worlds and have a static site generator that can be extended with dynamic applications components? Fortunately, yes! But, we have to make our choice of dynamic framework first.

#### The web development framework: [Vue](https://vuejs.org/)

The top two web development framework contenders out there today are [React](https://reactjs.org/) and [Vue](https://vuejs.org/). Of the two, I chose Vue because it is lightweight and easy to learn. Vue is newer than React, but it already enjoys a pretty large and healthy community -- just as an indicator, Vue has 148K github stars vs React's 136K!). React just feels like overkill for this application (and many others).

#### The pre-render framework: [Gridsome](https://gridsome.org/)

Ok, with our choice of web development framework made, what are the options for generating static pages (i.e., pre-rendering)? After some digging, I was able to identify the following three Vue-based tools: [VuePress](https://vuepress.vuejs.org/), [Nuxt](https://nuxtjs.org/) and [Gridsome](https://gridsome.org/). So I went ahead and did a fairly quick hands-on evaluation, and the winner was Gridsome. VuePress is really easy to use, but it was created mainly to generate documentation sites and while it is possible to make it do other things, it is not that easy to find examples beyond documentation use-cases. Nuxt is big, and it goes well beyond static generation -- you can choose to deploy your app as an SPA, a Server-Side Rendered (SSR) app, and Static Generated (pre-rendered), really too complex for our needs. But Gridsome really hits the sweet spot between dynamic and static, between ease of use and flexibility. Much like Goldilocks and the three bears story, Gridsome felt just right!

<p align="center">
	<img src="/images/uploads/goldilocks.jpg"/><br>
  <small>The three bears</small>
</p>

The Gridsome folks came with a very interesting approach to make the data available for use in the application. At build time, Gridsome gathers all of your data (e.g., [markdown](https://en.wikipedia.org/wiki/Markdown) files, [JSON](https://www.json.org/) files, [APIs](https://en.wikipedia.org/wiki/Application_programming_interface)) and makes it available in a [GraphQL](https://graphql.org/) layer for you to query in a special section (i.e., ```<page-query>```) of your application modules (pages, templates, layouts, and components). This felt a bit intimidating at first, but after taking it for a spin, I found it to be very easy to use. Gridsome even provides a live GraphQL tool to test out your queries ahead of time. By-the-way, at runtime, the GraphQL layer is gone, and there is your data, populated in your web pages, the way you wanted it. For the dynamic parts of the application (i.e., live usage of APIs), you can use AJAX -- that's because the generated site is still a bona fide Vue application, after all.

#### The maps: [Leaflet](https://leafletjs.com/)

To create interactive maps the answer is [Leaflet](https://leafletjs.com/). It is the leading open source tool for 2D mapping. I have used it extensibly and I am very happy with it. It is lightweight and has pretty good performance. If we ever get into 3D mapping with this application, I would definitely use [Cesium](https://cesiumjs.org) again. I already have some thoughts about interesting 3D visualizations we could do in this app, but it is definitely not a high priority at the moment.

#### The look and feel: [Bulma](https://bulma.io/)

Dealing with CSS is hands-down one of the most frustrating aspects of web development. CSS is big, very complex and absolutely not intuitive. Fortunately, there are good choice of pre-created CSS styles (aka, CSS frameworks) that work well with Vue. I considered [Tailwindcss](https://tailwindcss.com/), [Vuetify](https://vuetifyjs.com) and [Bulma](https://bulma.io/). Like Goldilocks again, Tailwindcss is too low level, Vuetify is a little too big and intrusive, but Bulma felt just right. Bulma (and its Vue companion [Buefy](https://buefy.org/)) is lightweight and leaves me a lot of freedom to customize the styling of the application.

#### Hosting: [Netlify](https://www.netlify.com/)

There are a number of low cost to free options for hosting. But [Netlify](https://www.netlify.com/) is absolutely the best of all. It integrates with your Github repository and automatically generates and deploys the site whenever you push your changes to the repo. It also generates a preview of your site every time you create a pull request, so that your team can give the thumbs up or down before merging your changes. You can use custom domains with HTTPS at no extra cost. And look ma, they have a free plan!

#### Content management: [Netlify CMS](https://www.netlifycms.org/)

Last but not least, an important aspect of a site like this is to enable your authenticated customers to update the content on their own, without any technical knowledge. Fortunately, Netlify offers [Netlify CMS](https://www.netlifycms.org/) for free. Netlify CMS is an open source tool that you can integrate into your site to enable a forms-based capability to edit the content in markdown, [yaml](https://en.wikipedia.org/wiki/YAML), and/or JSON format. These files are the data that feed Gridsome, which automatically rebuilds the site after committing to the repository.

### Issues to consider

Well, there are always issues to deal with. By far the biggest one in the toolset above is the fact that Gridsome is really, really new, and still under very active development. One drawback at the moment is, for example, the fact that the tool does not currently support internationalization out of the box. I know I can handle it on my own, but it looks like it is a highly requested feature whose importance has been acknowledged by the developers. I know that  making this application multilingual is very important, but it won't happen for months, so I am banking on Gridsome to get this right by then.

Another issue of dealing with a framework like Vue is that you always have some "barrier" to access native web features. You really have to follow the rules and that sometimes can be painful, especially if you know how easy something can be if you dealt with it directly. For example, right now I am using the [vue2-leaflet](https://korigan.github.io/Vue2Leaflet/#/) components, which wrap leaflet APIs, and sometimes I feel trapped in the vue2-leaflet box, partly because its documentation is not in great shape right now. But I know I can use "naked" leaflet if I need to. So far I have been able to accomplish everything I have needed without having to dump vue2-leaflet.

### Summary

Wrapping up, we have a diagram, because why not?

<p align="center">
	<img src="/images/uploads/tools.svg"/>
</p>

<br>

|Tool|Purpose|
|-----------|-----------|
|Vue|Extending functionality, adding dynamic capabilities|
|Gridsome|Pre-render static site|
|Leaflet|Interactive mapping|
|Bulma/Buefy|CSS, styling, look and feel|
|Netlify|Hosting|
|Netlify CMS|Content Management|

Application development has already started, and I will share the results soon. Stay tuned!

-----

That's it for now. Until next post!
