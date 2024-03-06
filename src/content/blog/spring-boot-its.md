---
title: 'Configuring Spring Integration Tests with composable TestContainers'
description: 'Use TestContainers to simulate external services'
pubDate: 'Jan 10 2023'
heroImage: '/blog-placeholder-4.jpg'
---

This post shows how to bring the concept of Composition over Inheritance to the world of Spring Boot integration tests.

Our example application will read messages off an input kafka topic, and based on that do a lookup in a PostgreSQL database while also downloading reports from S3, before finally publishing on an output kafka topic. This is the sort of application where there are a lot of external integrations, and fairly simple business logic, so integration tests are especially valuable.

What I've often seen done to set up these tests is a base class called ```TestContainersTestBase```, ```FullITTestBase``` or something similar. Then any test wishing to use the test containers to simulate external services can simply inherit from this class. This has a few potential problems though. A big one is the lack of flexibility. For various reasons, we may want to do a "partial" integration test rather than a full one with absolutely nothing mocked. For example, I've found using a test container for Kafka to be quite slow, bringing a lot of overhead compared to other test containers. So what if we want to run some tests where the kafka reading and publishing is mocked out with much quicker ```direct``` camel routes? Would we make another base class?

A better solution is to use the concept of initializers. To get a test configured to use a TestContainer, we need to basically do 2 things. Startup the container, before the application context is fully initialized, and initialize the application context, overriding certain configuration values, like the database connection url or S3 bucket name for example.