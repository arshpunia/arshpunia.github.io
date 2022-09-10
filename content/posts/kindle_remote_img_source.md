---
title: "Kindle, Remote Assets, and EPUBs
"
description: "EPUBs are more than just ZIP files"
date: 2022-09-09T13:40:56-04:00
draft: false
---


I recently created an HTML file, converted it to EPUB, and sent it to my Kindle. While the files were loading correctly (including images) in both formats on my desktop, the Kindle would not display the images. Instead, it was showing a camera thumbnail as a placeholder. 

Presumably, the issue lay with how the Kindle was handling the EPUB. Some preliminary research took me to [Kindle Direct Publishing's image guidelines](https://kdp.amazon.com/en_US/help/topic/G75V4YX5X8GRGXWV). In the code samples here, all the inline images were stored locally. The `src` attributes of the `img` tags in my original HTML, however, were all referencing assets on remote servers. 

Upon pointing the `src` attributes to a local image of a [chicken nugget](https://i.etsystatic.com/18862914/r/il/9ddd2d/3355087118/il_fullxfull.3355087118_rgbz.jpg), the images rendered as expected on the Kindle. 

## Lessons Learnt ##

There's two key lessons I drew from this experience. First, EPUBs can effectively be thought of as ZIP files [^1] with a few added bells and whistles ([more details in the next section](#appendix---understanding-epubs)). This makes it easier to consider one's e-books as self-contained respositories of text, metadata, and assets; as opposed to a single HTML file. 

Second, as a developer, it is important to consider the constraints and limitations of the system that will eventually run your code. It was erronous judgement on my part to not consider that most Kindles are not always online and would therefore not support remote asset retrieval.   

## Appendix - Understanding EPUBs ##

I admit the `epub.equals(zip) = true` treatment is a bit of an oversimplification so I am going to try and add some context here. The EPUB Open Container Format (OCF) "defines a file format and processing model for encapsulating the set of related resources that comprise an EPUB Publication into a single-file container, the EPUB Container" ([Source](https://www.w3.org/publishing/epub3/epub-ocf.html)). It is this EPUB container that is the "packaging and distribution format for EPUB publications" ([Source](https://www.w3.org/publishing/epub3/epub-spec.html#dfn-epub-container)). It is important to note here that the OCF itself is built upon Open Document Format (ODF) ZIP technologies, with added features for obfuscating embedded resources, such as fonts. 

In other words, some folks got together and built the OCF on top of ZIP. OCF takes your text, assets, and metadata, and ultimately yields an EPUB container that you can then send to your Kindle.

[^1]: Try changing the extension from `.epub` to `.zip`. You'll actually be able to view all the text, media, and metadata files that constitute your EPUB.
