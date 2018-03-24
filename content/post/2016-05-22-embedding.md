---
title: Embedding RikiTraki in your blog or web site
date: 2016-05-22
tags: ["rikitraki"]
---
### If you have a blog or a web site, it is pretty easy to embed a [RikiTraki](https://www.rikitraki.com) trail in it, here is how.

---
You can easily embed a track that you have uploaded to [RikiTraki](https://www.rikitraki.com), by using the html `iframe` tag. Simply insert an `iframe` tag in your post or page with a link to the trail, like shown in the example below. Notice how it is not merely an image of the track, but the active RikiTraki site which you can fully interact with!

<!--more-->

<iframe width="100%" height="600" src="https://www.rikitraki.com/?track=4khD5bJGZ" frameborder="0"></iframe>

The code to display the track above is here:

`<iframe width="100%" height="600" src="https://www.rikitraki.com/?track=4khD5bJGZ" frameborder="0"></iframe>`

Simply replace the link `src="https://www.rikitraki.com/?track=4khD5bJGZ"` with the url of the track that you wish to show. You can also show it in 3D by appending `&terrain=yes` to the url, but I do not recommend it because some machines may not be able to support the 3D visualization, so it is better to let the user click on the 3D button to view it -- if their browser does not support 3D, the button will not even show!

Note that by specifying the width in percentage instead of pixels, [RikiTraki](https://www.rikitraki.com) will resize correctly on mobile devices. You can play with the height attribute to make it look nice in your specific site/blog layout.

I hope this trick is helpful. Feel free to comment below or send me a tweet if you have any questions.

Ah and as promised, in my next post, I will give a summary of my overall [FOSS4G-NA](https://2016.foss4g-na.org/) experience.
