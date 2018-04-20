---
title: "WeDeploy Beta: Our Biggest Release Yet"
description: "We have been working tirelessly over the last months on some huge changes to WeDeploy, and today is the day we can finally tell you all about them."
date: "August 4, 2017"
author: "Eduardo Lundgren"
image: "http://wedeploy.com/images/blog/post-12--0.jpg"
layout: "blog"
---

<article>

<a href="https://console.wedeploy.com" target="_blank">
  <figure style="margin-top: -1.5rem">
    <img src="/images/blog/post-12--0.jpg" alt="Introducing WeDeploy Beta">
  </figure>
</a>

{$page.description}

But first, a little backstory.

#### Where We Started

Exactly 316 days ago, we launched the first app on WeDeploy. It was such an exhilarating time, but we had our eyes set on the future.

The thing is, WeDeploy didn't start as "WeDeploy." Almost two years ago, a few of us spent our spare time working on this project called Launchpad. It was just a shadow of what WeDeploy is today, but it was a start. It was **our** start.

After that first app was deployed, we got right back to work. Yes, we had achieved something, but it still had much room to grow. We needed to challenge our own mindset and imagination in order to create a platform that could change the way people develop software. We believe that this release puts us much closer to that goal.

---

#### What Changed?

##### 1. Completely New Infrastructure

Probably the biggest change was moving the WeDeploy infrastructure from our servers in the Liferay Headquarters to AWS. We knew that in order to continue to grow and expand globally, we had to start preparing now.

This move has greatly impacted the stability of WeDeploy and even though we don't support multi-region deployments yet, we are excited for that possibility in the near future.

##### 2. Rewritten CLI, API and Dashboard

One of our goals these past nine months was not to just build on top of what we had, but also to rethink the way we built WeDeploy. If improvement required a revamp of core pieces of code, then so be it!

We've spent weeks refactoring the CLI, API and Dashboard (which is now known as [Console](https://console.wedeploy.com)) and I can confidently say, it was worth it! By refactoring, we gave ourselves the opportunity to not only clean up the code, but to also improve performance, usability and overall design.

##### 3. HTTPS Out-of-the-Box

Yes, now every project you deploy and every service you install will automatically be given an HTTPS certificate. This means your app will safely communicate with your users without you having to configure anything.

We also make sure that those HTTPS certificates are valid by regularly renewing them. That way you can always trust that your application will be secured.

##### 4. Custom Docker Services

The container ecosystem has grown a lot in the past years and [Docker](https://www.docker.com/) is largely responsible for this success.

Today we want to bring the benefits of this technology to you and facilitate the Docker-based development workflow. That's why we're introducing `Dockerfile` support.

Back on alpha you were limited by the services we provided. Now, your imagination is the limit. This opens up the possibility to deploy pretty much anything and we look forward to see what you're going to create.

<figure>
  <img src="/images/blog/post-12--docker.gif" alt="Docker">
</figure>

*Read more in the [documentation](/docs/deploy/deploying-docker/) or follow our [step-by-step tutorial](/tutorials/docker/get-started/).*

##### 5. Volume Support

With the addition of custom Docker services, new challenges arrived. For example, let's say you deploy an app with PHP and MySQL, then you start adding entries to your database. What happens when you restart that project without a proper setup? Well, you're going to lose all the data that was saved.

In order to be able to persist and share data between services, we're introducing the concept of volumes. Quite simply, volumes are specially-designated directories that live outside of the file system where your code runs. This way, even if your application becomes unavailable due to a failure or other causes, your data persists.

##### 6. Instant Deployment

Before, you had two ways of deploying code. You could either connect with GitHub or create a remote pointing to our Git server. We noticed that people loved the convenience of integrating with GitHub so [we're keeping that](/docs/deploy/continuous-deployment/), but when using our Git server the experience was not as fluid, especially the authentication step that occurred on every push.

Because of that, we decided to rethink our CLI and introduce a new deploy command. Now you don't need to rely on Git for every deploy. All you need to do is type `we login` to authenticate only once and then run `we deploy` on your project folder.

<figure>
  <img src="/images/blog/post-12--instant-deployment.gif" alt="Instant Deployment">
</figure>

*Read more on the [documentation](/docs/deploy/getting-started/).*

##### 7. Automatic Code Detection

If you used WeDeploy before, you probably remember this `container.json` file (now renamed to `wedeploy.json`). This file describes the name of the service, the environment variables needed, and most importantly, the kind of project you are deploying.

WeDeploy needs to know what kind of project you are deploying in order to download dependencies, compile and build it in the right way.

Together with your `wedeploy.json`, we'll try to determine what language your project uses. Deploy any project using [Node.js](/docs/deploy/deploying-nodejs/), [Java](/docs/deploy/deploying-java/), [Ruby](/docs/deploy/deploying-ruby/) or a [Dockerfile](/docs/deploy/deploying-docker/) and we'll take it from there.

##### 8. Liferay Digital Experience Platform

We're very excited about [Liferay Digital Experience Platform (DXP)](https://www.liferay.com/digital-experience-platform). There are so many cool capabilities and we want to give WeDeploy users the opportunity to try them out in seconds.

Today, we're introducing Liferay DXP with a 30-day trial license.

Besides that, we're adding support for [hot deployment](https://dev.liferay.com/discover/portal/-/knowledge_base/7-0/installing-apps-manually). That means you can now create a `deploy` folder, then add themes, portlets, OSGi modules or a license by using WAR, JAR and XML files.

<figure>
  <img src="/images/blog/post-12--liferay-dxp.gif" alt="Liferay DXP">
</figure>

What about [Liferay Portal Community Edition](https://web.liferay.com/community/liferay-projects/liferay-portal/overview)? Since we have Docker support now, you can setup your own Dockerfile or grab an image from [Docker Hub](https://hub.docker.com/r/liferay/portal/) and deploy it here.

*Read more in the [documentation](/docs/deploy/deploying-liferay-dxp/) or follow our [step-by-step tutorial](/tutorials/liferay-dxp/get-started/).*

##### 9. Zero Downtime Deployments

During alpha, whenever a new deployment was triggered, your app would go offline for couple seconds until the new version was 100% ready. We knew that was not ideal, so we sat down to build a much better experience that minimizes downtime and user interruption.

Now, you don't have to worry about your app being taken down during deployment of the new code. Push updates and restart your services with zero downtime.

##### 10. Global DNS

We know how tedious it is to set up your domains. That's why we're introducing a new reliable and scalable DNS solution.

From now on, you can point your domains directly to our nameservers and we will automatically configure everything for you.

We provide our DNS service across 7 different countries, including Australia, Germany, France, Netherlands, Singapore, United Kingdom and the United States. Our goal is to ensure that you always have high availability and low latency, resulting in a great experience for your users and your business.

*Read more in the [documentation](/docs/intro/custom-domains/#2).*

---

#### What's Next?

Because of the new URL format and other changes described here, we weren't able to migrate your projects for you. That would be a risky thing to do and we don't want your users to have any problems.

All the projects you created on alpha still exist. However, on September 4th they will be **unavailable**, along with the previous Dashboard. But don't worry, we already have a Migration Guide ready so you can get started right away.

We're very excited to be able to share this new release with you! Make sure to **[try it out for yourself](https://console.wedeploy.com)** and [join our community on Slack](https://chat.wedeploy.com) to ask questions or let us know what you are building!

</article>
