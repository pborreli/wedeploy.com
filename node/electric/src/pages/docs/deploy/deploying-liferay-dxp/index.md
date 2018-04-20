---
title: "Deploying Liferay DXP"
description: "Launch a Liferay DXP Trial application in few seconds."
headerTitle: "Deploy"
layout: "guide"
weight: 7
---

### Liferay DXP Trial

###### {$page.description}

<article id="1">

## Introduction

[Liferay DXP](https://www.liferay.com/digital-experience-platform) provides an architecture for companies to digitize business operations, deliver connected customer experiences, and gather actionable customer insight, with the ultimate goal of providing better customer experiences for their clients.

We give you a free 15 day trial period to test and develop your DXP instance.

</article>

<article id="2">

## Try it yourself

Want to see the process of deploying Liferay DXP Trial step by step?

<div class="guide-btn-cta">
  <a class="btn btn-accent btn-sm" href="/tutorials/liferay-dxp/" target="_blank">
    <span class="icon-16-external"></span>Try the tutorial
  </a>
</div>

<div class="guide-aux-cta">
	or see a <a href="https://github.com/wedeploy-examples/liferay-dxp-example" target="_blank">source code example</a>.
</div>

</article>

<article id="3">

## Configuring

<aside>

All WeDeploy projects use similar configuration files to prepare your projects for deployment. Learn more on our page about <strong><a href="/docs/deploy/getting-started/">Getting Started with Deploying</a></strong>.

</aside>

Below is an example of a `wedeploy.json` for a Liferay DXP service. The `id` for your services are uniquely determined by you.

```application/json
{
  "id": "myservice",
  "image": "wedeploy/liferay:@site.version.image.liferay@",
  "volume": "/opt/liferay/data",
  "memory": 4096,
  "cpu": 3
}
```

</article>

<article id="4">

## Hot Deployment

Liferay DXP's hot deploy mechanism allows you to install themes, portlets, OSGi modules, and even a license by simply including WAR, JAR, and XML files into a `deploy` folder.

For example, if you wanted to deploy a custom JAR file, this is how your directory could look like:

```xml
myservice
├── deploy
│   └── com.liferay.wedeploy.samples.portlet-1.0.0.jar
└── wedeploy.json
```

Under the hood, those files will be copied into the `$LIFERAY_HOME/deploy` folder and automatically deployed on startup.

<aside>

###### <span class="icon-16-star"></span> Pro Tip

You can provide your own activation key to your Liferay DXP instance on WeDeploy. Simply put your license or activation key inside the deploy folder with the name `license.xml`. If this file exists, we will use your license instead of starting a 30-day DXP trial.

</aside>

</article>

<article id="5">

## OSGi configurations

OSGi framework allows you to configure OSGi modules by simply including `.cfg` and `.config` files into an `osgi-configs` folder.

For example, if you wanted to deploy a configuration for the Elasticsearch OSGi bundle, this is how your directory could look like:

```xml
myservice
├── osgi-configs
│   └── com.liferay.portal.search.elasticsearch.configuration.ElasticsearchConfiguration.config
└── wedeploy.json
```

Under the hood, those files will be copied into the `$LIFERAY_HOME/osgi/configs` folder and automatically applied on startup.

</article>

<article id="6">

## Liferay DXP Configuration

Liferay DXP can be configured using a properties file named `portal-ext.properties` and located in portal's classpath. WeDeploy allows you to deploy a portal-ext.properties to your Liferay DXP service simply including it next to the wedeploy.json file.

For example, if you wanted to deploy the configuration for your Liferay DXP, this is how your directory could look like:

```xml
myservice
├── portal-ext.properties
└── wedeploy.json
```

Under the hood, that file will be copied into the `$LIFERAY_HOME` folder and automatically applied on startup.

<aside>

###### <span class="icon-16-star"></span> Pro Tip

In order for Liferay DXP to work properly on WeDeploy, you need two properties to be set in your custom portal-ext.properties file:

```properties
web.server.protocol=https
redirect.url.security.mode=domain
```

Otherwise, Liferay DXP won't calculate the URLs properly under our load balancer with HTTPS protocol enabled.

</aside>

</article>

<article id="7">

## Shared libraries

Sometimes you want to make visible several Java libraries to both Tomcat internal classes and to all web applications. This is the case for JDBC drivers, and portlet or servlet API's, as example. For this reason Liferay DXP adds the `lib/ext` directory to Tomcat's **Common** classloader, and puts there several libraries for common use. WeDeploy allows you to upload your shared libraries to your Liferay DXP service simply including them into a `shared-libs` directory present next to the wedeploy.json file.

For example, if you wanted to add an Oracle JDBC driver to your Liferay DXP instance, this is how your directory could look like:

```xml
myservice
├── shared-libs
│   └── ojdbc6.jar
└── wedeploy.json
```

Under the hood, those files will be copied into the `$CATALINA_HOME/lib/ext` folder and automatically loaded on startup.

</article>

## What's next?

Now you can start building your Liferay DXP Trial application.
