---
title: "S3 Presigned URLs and Unity"
date: 2022-02-15T23:03:27-05:00
draft: false
---

I recently learnt that you can share objects in private S3 buckets by using pre-signed URLs.

> The \[S3\] object owner can share objects with others by creating a presigned URL - using their own security credentials to grant time-limited permission to download the objects. 

Source: [S3 User Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/ShareObjectPreSignedURL.html)

This functionality has been really useful in getting a `VideoPlayer` GameObject in Unity to directly play a video hosted on S3. You just generate a pre-signed URL for the video [^1] and attach it to the `VideoPlayer`'s URL component. No need to deconstruct frames into byte-arrays and such - Unity takes care of it all AND your objects are never made public.


[^1]: This can be done through the AWS Console or in-script. Here is a [.NET code sample](https://docs.aws.amazon.com/AmazonS3/latest/userguide/ShareObjectPreSignedURL.html#.net)