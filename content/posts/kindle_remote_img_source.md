---
title: "Kindle_remote_img_source"
date: 2022-09-04T13:40:56-04:00
draft: true
---

Let's say you have a nicely formatted HTML document with a few inline images. The `src` attribute of your `img` tags point to an image hosted on a public server. You see that your local HTML page nicely displays the images. You even open it in Calibre after you've converted it to an epub and you still see the image. Then you email the converted book to your Kindle, and strangely enough, the images disappear. The Kindle, however, is kind enough to show you a thumbnail of a camera as a substitute for the iamge that would have normally been there. 

I reckoned that there must have been something wrong with how I was formatting my images till I read through the example code on Kindle Direct Publishing's image guide: https://kdp.amazon.com/en_US/help/topic/G75V4YX5X8GRGXWV

All of them refer to local images. I tried that with a local image of a chicken nugget and it worked. 

The lesson learnt here is to question your assumptions on internet connectivity. I suppose you could say this was my own version of "falsehoods programmers believe about internet connectivity".
