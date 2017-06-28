+++
date = "2017-06-27T22:45:00-07:00"
draft = false
description = ""
title = "Change your AWS Cloudfront Origin to get local index.html files working!"

+++

Once I had my website deployed using [Bitbucket Pipelines](https://bitbucket.org/product/features/pipelines) and [AWS S3+Cloudfront](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/MigrateS3ToCloudFront.html) (see my previous blog here), I ran into a really annoying issue which meant all of my blog posts published by hugo became inaccessible. The root of my website was fine, but all the subfolders (where the posts are stored), were not accessible! I kept seeing errors like this when viewing my posts:

```
<Error>
<Code>AccessDenied</Code>
<Message>Access Denied</Message>
<RequestId>abcdef</RequestId>
<HostId>
jargon hostID value
</HostId>
</Error>
```

I pulled my hair out googling for the solution, and finally came across it burried in the answers of a stackoverflow thread. The default origin that AWS uses is not allowed to apply a default docroot to sub folders, which leads to problems like what I had experienced, where bucket policies and cloudfront appears to be setup correctly, but subfolders refuse to render. Full credit for the solution to [JBaczuk](https://stackoverflow.com/a/33087418), even though his answer didn't get selected - it certainly did the trick for fixing my problem.

All I had to do was change the AWS Cloudfront origin from the s3 bucket `aws-website-xxxx-xxxx.s3.amazonaws.com`, to the static website url `aws-website-xxxx-xxxx.s3-website-us-east-1.amazonaws.com`.

Once the distribution propagated to the edges, the problem was resolved! Hurrah!