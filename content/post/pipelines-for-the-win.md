+++
date = "2017-06-27T22:02:08-07:00"
draft = false
description = ""
title = "Using Bitbucket pipelines for fun and profit"

+++

More than a year has now passed since my last blog, and a lot has moved on from a technology perspective. [Aerobatic](https://aerobatic.io) changed their pricing model, and although still extremely reasonable, it because unmaintainable for me to use on my personal projecs. Never fear though - a solution was here in the form of [Bitbucket Pipelines](https://bitbucket.org/product/features/pipelines) and some cool developments from [AWS S3+Cloudfront](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/MigrateS3ToCloudFront.html).

I have decided to migrate my automated build and deploy scripts from using [Aerobatic](https://aerobatic.io) to using [Bitbucket Pipelines](https://bitbucket.org/product/features/pipelines) (w/ [Docker](https://www.docker.com/)) and [AWS S3+Cloudfront](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/MigrateS3ToCloudFront.html) - partially cause I want to see how complex this will be to manage, and partially cause I believe I can save a bunch of money comparatively speaking!

Here are the steps that I took:

1. Create an AWS Cloudfront and S3 distribution to host my website
2. Link the website to my domain name (in this case blog.superfucking.cool)
3. Create a new IAM role for my deployment user
4. Setup my environment variables in Bitbucket Pipelines
5. Setup Bitbucket Pipelines using a sample docker container for Hugo
6. Deploy website using Bitbucket Pipelines
7. Cross fingers

Here is the breakdown of each step along the way.

## Create AWS Cloudfront and S3 distribution

This part is super simple now that AWS has improved their GUI. Just log in to the [AWS console](https://console.aws.amazon.com/console/home?region=us-east-1), under "build a solution" select "[host a static website](https://console.aws.amazon.com/quickstart-website/home)", add a new website (named to your choosing) and then click create!

## Link to my domain name

Once the setup has been completed above, I can then link an existing domain name to my new website. Click buy domain (confusingly labelled button??), then select "choose existing domain", select one of your domains already registered with Route53 and specify a subdomain. Then click Associate Domain and you're done!

## Create an appropriate IAM role

In order to complete this step, I need to get the full bucket name that I will be uploading data to, before I start going to create an new role. From the current website solution screen, click "Manage files in Amazon S3" to get to the related bucket, and grab the associated bucket name (e.g. aws-website-xxxx-xxxx) from the URL string.

Then go to the [IAM section of AWS](https://console.aws.amazon.com/iam/home?region=us-east-1), go to Users, Add User, specify a user name (I suggest something like BBPipelinesDeploy), and choose programmatic access. Continue to the next permissions screen, and select "Attach existing piolicies directly", and then select "Create policy". This will allow you to specify specific buckets and permissions to be granted, allowing highly restricted permissions to be given to Bitbucket and nothing else.

The policy you will want to create looks like:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "s3:List*",
                "s3:Put*",
                "s3:DeleteObject"
            ],
            "Resource": [
                "arn:aws:s3:::aws-website-xxxx-xxxx",
                "arn:aws:s3:::aws-website-xxxx-xxxx/*"
            ],
            "Effect": "Allow",
            "Sid": "AllowPipelinesDeployAccess"
        }
    ]
}
```

Then click "next" to review the setup, and finally click create user. On the next screen I would advise downloading a CSV of the credentials so you have them for later reference. If you do not record these details now they are gone forever (and you need them in the next step).

## Add environment variables to Bitbucket

In your [Bitbucket](https://bitbucket.org) repository, you should then go to your repository settings, and go to the [pipelines environment variables section](https://bitbucket.org/your_username/xxxx/admin/addon/admin/pipelines/repository-variables). Once in here you need to create 3 new variables:

```
AWS_ACCESS_KEY_ID = the aws access key generated in the step above (set as secured)
AWS_SECRET_ACCESS_KEY = the aws secret key generated above (set as secured)
AWS_DEFAULT_REGION = the region name that you created your website in (e.g. us-east-1)
```

## Setup Bitbucket Pipelines

Once you have done all of the above, you're now ready to get started with [Bitbucket Pipelines](https://bitbucket.org/product/features/pipelines) itself.
Click on the "Pipelines" action in the side bar of [Bitbucket](https://bitbucket.org), and click enable.

On the next screen you can choose any language and then click next - it is irrelevant as we're going to override the resulting data with a script provided below.
Shout out to [Karel Bemelmans](https://www.karelbemelmans.com/) for his awesome Hugo Docker image that is used for this blog and for my other static sites!

Once you get to the editable bitbucket-pipelines.yml file, enter the following:

```
image: karelbemelmans/pipelines-hugo

pipelines:
  default:
    - step:
        script:
          - apk add git
          - cd themes && git clone https://github.com/aerohub/hugo-orbit-theme && cd ..
          - hugo --theme=hugo-orbit-theme
          - aws s3 sync --delete public s3://aws-website-xxxx-xxxx
```

Note that you can remove the `-apk add git` and `cd themes && git clone https://github.com/aerohub/hugo-orbit-theme && cd ..` commands if you have a local copy of your desired theme already checked out in your repo. Otherwise these will ensure that your theme is pulled and compiled before your data is published.

## Commit the file

Once you commit the change to the bitbucket-pipelines.yml file, the Pipelines engine will kick off and compile your hugo site and (hopefully) deploy it to AWS. Two last things to do, check whether the build was successful by opening the Pipelines tab, and seeing what the status of the last build was. If it was successful, you site has been deployed (woohoo)!! If that all worked to plan, you should be able to now view your new site on AWS by hitting your domain name as specified in the steps above!

That's all for now! Happy static-site-generating!!
