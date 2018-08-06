---
title: Frequently Asked Questions
description: Common questions asked at each stage of GraphQL adoption
---

Everyone has questions about how to properly set up a GraphQL schema, but not all questions are alike. In different stages of development, different things matter. This guide will questions that people commonly have have at every step along the journey to GraphQL in production.

<!-- -->
<!-- -->
<!-- -->
<!-- -->
<!-- -->
<!-- -->

## Learning GraphQL

> People in this phase are just beginning to learn GraphQL. They are trying to get a sample project up and running.

#### Why use GraphQL?

GraphQL can make a difference in nearly every area of development: from improving developer experience with quality tooling to improving client performance by reducing bundle sizes. Read more about the benefits of GraphQL [here](../fundamentals/benefits.html).

#### Where can I learn the basics of GraphQL?

[TODO]()

#### How can I host my schema online?

A great tool for learning and building small projects is [Glitch](https://glitch.com). Glitch allows development of a schema in the browser, and even supports cloning from and pushing to GitHub. Glitch provides a public endpoint that projects can query against. To get started with building a GraphQL schema, try using and remixing the [Apollo Launchpad](https://glitch.com/~apollo-launchpad) project.

GraphQL schemas written with `apollo-server` can be deployed anywhere that other Node.js projects can be deployed. `apollo-server` even has variants to support serverless deployment with AWS Lambda.

There are deployment guides currently written for [Heroku](https://www.apollographql.com/docs/apollo-server/deployment/heroku.html), [Lambda](https://www.apollographql.com/docs/apollo-server/deployment/lambda.html), and [Now](https://www.apollographql.com/docs/apollo-server/deployment/now.html)

#### How do I connect my client app to my schema?

There are many tools available to connect any kind of client to a schema. The most common are [Apollo Client](https://www.apollographql.com/docs/react/) for JavaScript clients,
[Apollo iOS](https://www.apollographql.com/docs/ios/) for native iOS clients, and [Apollo Android](https://github.com/apollographql/apollo-android) for native Android clients.

For Apollo Client projects, there are also many view-layer integrations, to make querying GraphQL schemas easier in [React](https://www.apollographql.com/docs/react/essentials/get-started.html), [Vue](https://github.com/Akryum/vue-apollo), and [Angular](https://www.apollographql.com/docs/angular/).

<!-- -->
<!-- -->
<!-- -->
<!-- -->
<!-- -->
<!-- -->

## Building a proof of concept

> People in this phase understand how GraphQL works, and what benefits it offers. They are trying to create a proof of concept for their projects or their company to test GraphQL's viability in production

#### How do I wrap existing APIs?

One of the best things about GraphQL is that it works excellently with existing APIs. It's possible to connect any number of existing services to your schema.

The most common source is a REST API. The [`RESTDataSource`](https://www.apollographql.com/docs/apollo-server/features/data-sources.html) is a tool that integrates with `apollo-server` to simplify fetching and caching for existing REST APIs.

Other DataSources are under development, but even without the `DataSource` API, it's possible to connect any backend to a schema. [Resolvers](https://www.apollographql.com/docs/apollo-server/essentials/data.html) can do anything, including fetch data from an SDK or ORM. For more information on how to connect to different sources [read this guide](https://www.apollographql.com/docs/graphql-tools/connectors.html)

#### How do I design the schema?

Schemas should be designed with the needs of the client in mind. Rather than modeling queries and types after the underlying services, they should be designed to make querying as easy as possible. GraphQL's resolver structure makes it possible to allow this flexibility without many performance consequences. For more, read the [schema design guide](./schema-design.html).

#### How do I discover and reproduce errors?

As with any service, it's important to track errors and their causes. There are many kinds of errors that can occur with a GraphQL Schema. Some of these include service errors, where the schema can't access underlying services, and user errors, where a user enters invalid information in a query or mutation.

GraphQL is resilient to some of these errors. Since the schema is strongly typed, the designer has the ability to restrict what type of data users can enter and what type the resolvers can return. This type system catches many errors, and requires no manual checks.

For errors not prevented by the type system, it's helpful to know what exact queries were made, and with what variables. [Apollo Engine](https://www.apollographql.com/engine) is a tool that does exactly this. It can help discover and reproduce errors by showing the exact conditions in which the error occurred.

<!-- -->
<!-- -->
<!-- -->
<!-- -->
<!-- -->
<!-- -->

## Moving a feature to GraphQL

> People in this phase have decided to use GraphQL in production. They don't want to immediately refactor the APIs or apps. They want to move a single feature over to GraphQL to learn how to use it and monitor it in production.

#### How do I set up authentication/authorization for my GraphQL schema?

Authentication and authorization are important topics to discuss with any API. GraphQL provides a very granular approch to handling these topics. But don't worry! If an API being consumed by GraphQL already has authorization built-in, it may be possible to ignore it completely! Read more in the [access control guide](./access-control.html).

#### How can I secure my schema from malicious or expensive queries?

Public APIs of any kind need some kind of safeguards against malicious queries. Since GraphQL allows for recursive queries, it wouldn't be hard to create a query that is overly complicated and acts as a DoS attack, even by accident. There are multiple ways to prevent something like this from happening, from complexity limiting to query depth limiting. Read the [guide on security](./security.html) to learn more.

#### What kinds of cache should I setup?

GraphQL can be cached in multiple places. It can be cached on the client, preventing multiple queries from being made when not necessary.

Cache can also be setup at the schema level. Whole-query caching, partial-query caching, and cache backed by a CDN can all be used to lower response times, and make a GraphQL schema as performant as possible.

Whole-query and CDN caches are most useful when an API receives many of the same queries. This commonly happens with public data, like content on pages of a site. Regardless of whether the API is used for public data or not, these caches almost always provide large performance benefits, and are highly recommended. You can read more about how to set up whole-query and CDN caching with `apollo-server` 2.0 [here](https://www.apollographql.com/docs/guides/performance.html).

Partial query caching can be achieved by caching the responses from underlying services with something like Redis or Memcache. With this strategy, even if two queries look completely different from one another, if there is any duplication of data fetched, those results can be shared, preventing unnecessary traffic. The [`RESTDataSource`](https://www.apollographql.com/docs/apollo-server/features/data-sources.html) does this automatically if the appropriate `cache-control` headers are present in REST responses.

#### How can I monitor the health of my GraphQL schema?

Many apps and sites are powered almost completely by an API such as a GraphQL schema, so it's important to make sure the API is healthy at all times. Indicators of an unhealthy service include long response times, high resource usage, and unusual traffic patterns.

[Apollo Engine](https://www.apollographql.com/engine) is a great tool to track many of these things. It allows close inspection of fields to make it easy to see both total response times as well as how long each field took to execute.

Engine also has a [DataDog integration](https://www.apollographql.com/docs/engine/integrations/datadog.html#Monitoring-with-Datadog) that makes integration with existing tools easier.

<!-- -->
<!-- -->
<!-- -->
<!-- -->
<!-- -->
<!-- -->

## Moving a product to GraphQL

> People in this phase have a good understanding of how to write, deploy, and monitor GraphQL in production. They are looking to scale GraphQL features to their entire product line.

#### How do I organize schema code to scale for a larger project?

Keeping all schema code together makes sense for smaller projects, but once a project reaches a certain size, or has many people working on it, managing conflicts in the same file and code navigation can get difficult. Splitting types and resolvers up into smaller files can make this process much easier. Read [this blog post](https://blog.apollographql.com/modularizing-your-graphql-schema-code-d7f71d5ed5f2) to learn more.

#### How can I test my client?

`react-apollo` comes with everything needed to test a client app that makes queries to a GraphQL schema. Read the [Testing React Components](./testing-react-components.html) guide to learn more.

#### How can I safely make changes to the schema?

Schemas naturally evolve over time. GraphQL schemas are more resilient to change than other APIs, but there are still occasions where breaking changes will need to happen to support new functionality. The [versioning guide](./versioning.html) explains in more detail what kinds of changes are safe to make, and what kinds could break existing clients.

Additionally, using the [Apollo CLI](https://www.npmjs.com/package/apollo) with Apollo Engine provides the tools needed to [validate schema changes](https://www.apollographql.com/docs/engine/features/schema-history.html) over time. This makes collaboration easier and more transparent.

<!-- -->
<!-- -->
<!-- -->
<!-- -->
<!-- -->
<!-- -->

## Standardizing GraphQL

> People in this phase are very experienced with using GraphQL and have made the decision to try to move to GraphQL as a company-wide standard. They are interested in setting up processes and guidelines to make sure other teams with less experience will be successful.

#### How should the transition to GraphQL happen?

As with any large change, the adoption of GraphQL should be incremental. GraphQL allows teams to leave existing services as they are and build convenient gateways on top of them. Read more about incremental adoption of GraphQL in the [tips for success](../fundamentals/tips.html#incremental-adoption).

#### Who owns the schema design?

GraphQL schemas work best when their design is heavily influenced by the needs of the product developers. It's tempting to design a schema to resemble the underlying sources or databases, but this can be hurtful to the usefulness of GraphQL. Read more about schema ownership in the [tips for success](../fundamentals/tips.html#schema).

#### Should we enforce Node.js for schema development?

There are GraphQL server tools available for most popular languages, but it is recommended to use [apollo-server](https://www.apollographql.com/server), which is built on top of Node.js. Using a node.js server is beneficial, because of its ability to be run nearly anywhere (including the edge), and the wide knowledge of Node.js across development teams. Read more about why Node.js is recommended [here](../fundamentals/tips.html#javascript).