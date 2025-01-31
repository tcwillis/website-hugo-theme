---
title: "Why Pixboost Is the Best Image Cdn"
date: 2021-05-24T13:01:55Z
draft: true
---

We've got this question on Twitter and I thought that this is a huge gap that we haven't fill that elaborates our vision. Special thanks to @TODO for pointing this out.

TODO: Tweet card

I decided to structure this post in two sections. In the first part I'll talk about our open source image service which is the heart of Pixboost. I'll explain what the philosophy is and how it's different to other similar tools.

In the second part I'll go into our SaaS offering and why you could prefer it to our competitors.

The TLDR: Making things our way allows us to have a solid solution that works for most Web projects out there. The missions is to do one small thing, but be the best at it.

## Transformimgs Open Source Image CDN

### Images service built for web

Our main focus is to build image service that covers needs of typical web project that gets loaded on the different browsers by people from different location on the different devices and different network quality.

Being web-first allows us to make some decisions like supporting formats that browsers support, or we could decide to not support some feature that are not widely adopted, e.g. client hints.

Targeting web also enables us to eliminate some features that we think should be done by other tools. For example, making image round should be done by CSS and not API.

### Opiniated API or simple is power

Our API has just 4 endpoints, and you don't have much choice when using them. This is done intentionally and opens a few doors for us:

* Maintain API compatibility is easy. Without having thousands of options allow us to not worry about backward compatibility meaning the users don't need to look out for deprecated options and update their integration.
* Making algorithms smarter behind the scene. For instance, you can't specify the output format. Instead, we use the best possible format behind the scene. One of the examples would be AVIF and images with text. We found that on those images AVIF might generate some visible artifacts, so it's better to use WebP for those.
* Less code is a great advantage that generates less bugs. 

We don't say that we will never add more operations, but we'd like them to fit very well into our [web first strategy](#images-service-built-for-web). For example, we don't think that watermarks are user experience for product images in online shop, so we wouldn't add it to the service.

### Zero (almost) config

This is very close to the previous point, but is different. We still don't have config file, and you could only change behaviour using command line flags. We will try to keep it that way as long as possible.

For instance, some projects don't support next generation formats, like WebP or AVIF in the default configuration. In our case, we do support them, and you can't turn them off. 

### Containers optimised runtime

We choose to use Golang because you could compile it and get a lightweight staticly linked executable that could be easily moved around. We provide docker image and that is the best way to deploy the service into environment.

Containers became a pretty much default runtime nowadays, so you could easily deploy images service on all popular clouds and manage infra by yourself (cost is usually lower, setup and service more complex) or use serverless offerings (running cost is higher, but managing is trivial).


### CDN optimised

Given we have [opinion](#opiniated-api-or-simple-is-power) on everything we also think you are better to deploy the image API behind Content Delivery Network. Transformimgs supports Accept and Vary HTTP headers to serve images in the next generation formats and taking advantage of Save-Data client hint.

Given that assumption we could also streamline design decisions which makes the service implementation simpler and minimises risk of issues.

## Pixboost SaaS offering

Pixboost is a SaaS offering on top of the transformimgs service which brings all the above benefits and adds more. Let's see what they are.

### SaaS

Just quickly reiterate what benefits SaaS have.

* We take care of updates and security
* We make sure the service runs 24/7 and have [SLA]()
* We have a very thorough testing plan and before releasing a new version we go through hundreds of images and see if the main metrics are only improving.
* Image optimisation is quite CPU heavy workload, so it's a tough challenge to run it cost effectively. However, I think we nailed it, so we can offer a reasonable price for our service.
* Easy to use dashboard with the latest stats and usage information

### CDN Included

We include CDN in our offering, and we took our time to pick the best one. Currently we use Google CDN and there are reasons:

* HTTP/3 support. Google was one of the initiator of the standard and was one of the first who added it to their Content Delivery Network

* Google is Network. Google is the company who actually takes care of the network end to end. They put cable [down the ocean](https://cloud.google.com/blog/products/infrastructure/introducing-the-echo-subsea-cable), put Fibre to your house and make the most popular browser. So, we thought they can be trusted :)

* They have more than [100 points of presence](https://cloud.google.com/cdn/docs/locations), so you can make sure the images will be close to your users.

* Google is always one of the leader in all top [latency benchmarks](https://www.cdnperf.com/)

We constantly monitor the performance and competitors and have a clear plan of how we could migrate Pixboost to a different CDN. So, if there will be a better option then we'll switch there to provide our customer with the best available option.

### Tools and libraries

In addition to API that you could easily use in your HTML markup we also provide JS and React libraries that provide some feature like lazy loading out of the box.

On the other side of content delivery we also have Snippet Generator tool that helps content managers to put optimised images on the landing pages and articles.

TODO: Screenshot

### End 2 End solution

Because we provide end to end solution including tools and CDN we're also covering some of the edge cases like cache invalidation and warmup that could be integrated into more complex workflow. You could find the example of cache invalidation on workflow for AWS S3 integration here - https://github.com/Pixboost/aws-s3-invalidate-cdn.

### Simple pricing

You pay only for weight of the images that been delivered to your users. That's really it. So, it's pay for what you use.

What also would surprise you is if you serve the same number of images it becomes cheaper for you cause we improve the algorithms that make images smaller. For instance, when we [introduced AVIF format]() some of our clients started paying by up to 30% less than before.

### Open Source

At Pixboost we try to make everything Open Source: image processing API, libraries, website.

I'm a true believer in Open Source and think that it's the best business model for IT projects out there. Some of the highlights why open source is better:

* Quality is better. You publish the source code to the whole World, so it's essential to have a clean and well documented code.
* Community that you build around your project helps a lot with everything really. Finding issues quicker, brainstorming, product roadmap are some of the things you could get help with. 
* I do think that the Internet is a more that just source of the information. Some of the essential things like shopping, issuing passport or booking a doctor appointment is done over the Internet. Open source helps to make it transparent, so we can make sure that there is no fraud or privacy in the code.

## Conclusion

I re-read the article and hey, I like what's in there! Let me sign up to this Pixboost and see what it is :) If you think the same then give it a go. If you have concerns or think differently - that's good and maybe there is a solution out there that suits your better.

