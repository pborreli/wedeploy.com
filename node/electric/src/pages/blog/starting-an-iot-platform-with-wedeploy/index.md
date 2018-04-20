---
title: "Starting an IoT platform with WeDeploy"
description: "In this blog post I will guide you through the steps to create a minimal IoT platform which accepts metrics from remote devices. To build this multi-service project, I will use the WeDeploy Java, Hosting, and Data services."
date: "Mar 17, 2017"
author: "Manuel de la Peña"
image: "/images/blog/post-6--0.png"
layout: "blog"
directory: "email-js"
---

<article>

{$page.description}

I'm currently studying *Research in Software Engineering and Computer Science* for my master's degree and decided to build this platform to complete some requirements for a class about *Ubiquitous Computing*.

In this class I've learned the basic concepts of [IoT](https://en.wikipedia.org/wiki/Internet_of_things), starting with the initial intent to simulate the real world with smart devices. I believe the IoT train is passing and if we don't get on, we will miss out. That's why I decided to explore more.

<figure>
  <img src="/images/blog/post-6--0.jpg" alt="IoT train is passing. Credits: Amos Bar-Zeev (https://unsplash.com/search/wait?photo=IjA4vTJ3PHY)">
</figure>

I realized that in the short period of time I had to accomplish this, I would need to learn how to use and configure a complete new tool like [Kaa](https://www.kaaproject.org) or [Machina.io](https://macchina.io). But I just wanted a place to simulate an IoT platform super fast. Then a thought occurred to me:

> Why not use WeDeploy to host my app? It already provides all the infrastructure I need.

With WeDeploy, you manage `git` repositories and wrap them into services that can be deployed into the platform. It also offers a set of useful, already-configured services such as [Data](/docs/data), [Users and Auth](/docs/auth), or even a [E-mail](/docs/email). This allows any developer to streamline a project with minimal infrastructure configuration so they can just focus on coding the application they want to develop.

#### The Stack

So, here are the requirements for my project:

* An Android app that reads current speeds from a GPS and sends it each time the current location is changed.
* A server that reads the metrics sent by the Android app and stores them into persistent storage.
* A web interface to display the metrics stored in the database, reading from the server.

The Android app is super easy to create, and has nothing to do with WeDeploy, just knowing the URL of the server where to send the metrics, so I won't go anymore into it. **The two things that really matter** are related to the API I wanted to create to accept the metrics sent by the devices and the visualisation of those metrics.

#### Creating an API with WeDeploy

In order to accept metrics from remote applications, I decided to create a REST API to handle the requests, and then it would store the metrics in a persistent storage. As WeDeploy offers a [Data](/docs/data) service, I decided that my REST API should communicate to that service to store the metrics.

Then, I chose the framework to create the API. As I'm a Java developer, I chose [Spring Boot](https://projects.spring.io/spring-boot/) to build it, which lets you create tiny applications and define REST resources in an extremely easy manner.

So I created a project on the WeDeploy dashboard, and added a [WeDeploy Java](/docs/other/java/) service named "api". I locally cloned [the java example project on Github](https://github.com/wedeploy-examples/java-example), and started coding my REST API's there.

Of course you can do this step with other technology you are more familiar with, and WeDeploy offers several server-side technologies to accomplish this including [NodeJS](/docs/other/nodejs/) and [Ruby](/docs/other/ruby/).

##### Implementing a service

In order to implement a WeDeploy service, it's mandatory to create a `wedeploy.json` file in the root of your folder, where you specify the id of the service. As this project is a Gradle project, WeDeploy will infer its type with a Java recipe, that will wrap the project up into the official Gradle Docker image.

```application/json
{
    "id": "api"
}
```

##### Defining REST resources

The following block of code demonstrates how to handle HTTP `GET` and `POST` requests in a `Spring Boot` controller.

```text/x-java
/**
 * We'll handle all HTTP request to the path '/sensors' under application context path
 * @author Manuel de la Peña
 */
@RestController
@RequestMapping("/sensors")
public class SensorsRestController {

  /*
  * Defines the resource for an specific sensor, identified by its sensorId,
  * which is passed as input parameter to the method, using the annotation @PathVariable.
  * It allows CORS requests from any domain.
  * @return A collection of model domain objects
  */
  @CrossOrigin(origins = "*")
  @RequestMapping(method = RequestMethod.GET, value = "/{sensorId}")
  public Collection<SensorRow> getMetrics(@PathVariable String sensorId) {...}
  ...
  /*
  * Define the resource for POST verb operations over this controller's path.
  * The annotation @RequestBody indicates the type of the object that will be
  * processed in the request.
  * It allows CORS requests from any domain.
  */
  @CrossOrigin(origins = "*")
  @RequestMapping(method = RequestMethod.POST)
  public ResponseEntity<?> addMetric(@RequestBody Metric metric) {...}
  ...
}
```

##### Defining the save service

Our API service needs to store the metrics in the [WeDeploy Data](/docs/data) service so I added a new service to the project on the WeDeploy dashboard, naming it `data`. Again, I cloned the [WeDeploy Data example on Github](https://github.com/wedeploy-examples/data-example) at the same level of my API service, and voilà! I have two services under my WeDeploy project.

Here it is the `wedeploy.json` file for this new data service:

```application/json
{
    "id": "data",
    "image": "wedeploy/data:@site.version.image.data@"
}
```

But this data service must be configured to create a collection of elements. These are the pieces of data I wanted to gather about the sensor:
* **Platform** (app-android, app-ios, app-console),
* **Unique identifier**
* **Coordinates** (longitude, latitude)
* **Value**
* **Name** (temperature, speed, pressure, etc.)
* **Unit of measure** (celsius degrees, kilometers per hour, atmospheres, etc.)
* **Timestamp** (the moment of request)

And I also had to define how this collection of data is accessed, and which HTTP verbs are allowed.

You can configure this in WeDeploy data service with the `api.json` [descriptor](/docs/data/configuring-data/). This block shows the three REST resources that are accepted by my data service, and their specific paths: two for retrieving data, and one for storing data.

```application/json
[
  {
    "description": "Returns all sensors that sent metrics to the platform",
    "path": "/sensors/*",
    "method": "GET",
    "data": true
  },
  {
    "description": "Returns one sensor identified by its unique sensorId",
    "path": "/sensors/:sensorId",
    "method": "GET",
    "data": true
  },
  {
    "description": "Stores one sensor's metrics",
    "path": "/sensors",
    "method": "POST",
    "data": true,
    "parameters": {
      "applicationId": {
        "type": "string"
      },
      "sensorId": {
        "type": "string"
      },
      "latitude": {
        "type": "string"
      },
      "longitude": {
        "type": "string"
      },
      "metric": {
        "type": "string"
      },
      "metricName": {
        "type": "string"
      },
      "metricUnits": {
        "type": "string"
      },
      "timestamp": {
        "type": "string"
      }
    }
  }
]
```

As you can see for the POST operation, I can validate the input parameters that are sent to the WeDeploy data service.

##### Storing in the WeDeploy Data service

Ok, once I had the data service configured, how could I communicate with it from the Java service, which represents the API? Well, the smart guys from WeDeploy team have provided a set of libraries to communicate with any WeDeploy service in an easy manner: you can consume WeDeploy's APIs with language-specific clients. In my case I will use a Java client, which I added to Gradle dependencies in the Java service.

```text/x-groovy
dependencies {
  compile("com.github.wedeploy.api-java:api:0.0.1")
  compile("com.github.wedeploy.api-java:api-client:0.0.1")
  ...
}
```

Added the dependencies, the communication with the [WeDeploy Data service](/docs/data) is performed in this block of code:

```text/x-java
public class DataRepository {

    /**
    * Name of the data service to communicate with
    */
    private static final String DATA_SERVICE_PATH = "data";
    /**
    * Path of the Sensors collection
    */
    private static final String SENSORS_DATA_PATH = "/sensors";
    ...

    /**
    * We filter the collection by sensorId column. Then, we parse the JSON object
    * that represents the body of the response into a collection of model entities.
    */
    public Collection<SensorRow> findBySensorId(String sensorId) {
        WeDeploy weDeploy = new WeDeploy(DATA_SERVICE_PATH + SENSORS_DATA_PATH);

        Response response = weDeploy.filter("sensorId", sensorId).get();

    String body = response.body();

    if (StringUtil.isBlank(body)) {
      return Collections.EMPTY_LIST;
    }

    JoddJsonParser parser = new JoddJsonParser();

    return parser.parseAsList(body, SensorRow.class);
  }

  /**
  * We retrieve the whole collection, without pagination. Then, we parse the JSON object
  * that represents the body of the response into a collection of model entities.
  */
  public Collection<SensorRow> findAllSensors() {
    WeDeploy weDeploy = new WeDeploy(DATA_SERVICE_PATH + SENSORS_DATA_PATH);

    Response response = weDeploy.get();

    String body = response.body();

    if (StringUtil.isBlank(body)) {
      return Collections.EMPTY_LIST;
    }

    JoddJsonParser parser = new JoddJsonParser();

    return parser.parseAsList(body, SensorRow.class);
  }
  ...
}
```

Here the `WeDeploy` object hides the magic of communicating with other WeDeploy services. As I'm in the same project, it's possible to resolve the service name directly with only its name, without using its whole URL, because WeDeploy directly resolves service names when services run within the same project.

##### Displaying data

At that point I had built two services: a Java REST API developed with `Spring Boot`, and a [WeDeploy Data service](/docs/data) for persistence. Then I wanted to display the metrics in a web interface. So, I created a third WeDeploy service, [Hosting](/docs/hosting), which allows uploading static files, such as HTML, CSS and JavaScript, and gave it the name `ui`. Below is the `wedeploy.json` file that I made to implement the [Hosting Service](/docs/hosting).

```application/json
{
  "id": "ui",
  "image": "wedeploy/hosting:@site.version.image.hosting@"
}
```

I know that this is not related to WeDeploy, but I wanted to display the metrics in two different ways: a datagrid with all metrics, and a `Google Maps` interface with a pin where a metric was sent. With the following code, which **is not the WeDeploy JavaScript client**, I can retrieve the JSON objects returned from the `Spring Boot` API with browser's native `fetch` capability, process the response, and draw the results in the screen.

```text/javascript
function getSensors(mode) {
  return fetch(myUrl)
    .then(function(response) {
      if(response.ok) {
        return response.json();
      }
      else if (response.status !== 200) {
        return [];
      }
    })
    .then(function(sensors) {
      if (!sensors || sensors.length === 0) {
        noResults(); // shows a not found message

        return;
      }

      if (mode == 'grid') {
        plotSensors(sensors); // draws a grid
      }
      else {
        mapSensors(sensors); // draws a google map
      }

      return this;
    });
}
```

Please remember I'm a backend developer interested in Systems, so shame on me for this JavaScript code! Hey, at least I put it in a separate JavaScript file. 😂

Thanks to the WeDeploy team, a JS client is also available. Super cool! You only need to add this script to your page and you're ready to go:

```text/html
<script src="https://cdn.wedeploy.com/api/@site.version.api.javascript@/wedeploy.js"></script>
```

#### Summary

With WeDeploy I could accomplish the task of creating a very basic stack to handle requests. I wrote a a service representing an API for accepting requests, a service to store data, and a service representing a UI for the metrics. And all of that has been accomplish just using a couple lines of code + git skills, with no infrastructure knowledge needed!

If you're curious about the end result, see:

* [**sensorsui**-mdelapenya.wedeploy.io](https://sensorsui-mdelapenya.wedeploy.io)
* [**sensorsapi**-mdelapenya.wedeploy.io](https://sensorsapi-mdelapenya.wedeploy.io/sensors)

</article>
