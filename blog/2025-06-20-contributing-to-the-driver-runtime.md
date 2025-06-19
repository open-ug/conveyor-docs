---
slug: contributing-to-the-conveyor-ci-driver-runtime
title: A Guide to contributing to the Conveyor CI Driver Runtime
authors: jim-junior
tags: [conveyor-ci, runtime]
---

A detailed guide to contributing to the Conveyor CI Driver runtime.

<!-- truncate -->

## Introduction

A brief introduction on the Conveyor CI Driver runtime.

The Conveyor CI Driver runtime is a collection of SDKs(Software Devlopment Kits) used by developers to build Conveyor CI [Drivers](/docs/concepts/drivers). These SDKs are libraries that contain Utility functions that expose different Conveyor CI functionality. In this Guide we shall explore how one can contribute to the Conveyor CI Driver Runtime. This Guide is written to be language agnostic, meaning it can guide you to build or contribute to a driver runtime of your language of choice.

## How the Driver Runtime Works

Lets explore what the Runtime is

### What is the Driver Runtime

You can think of the Driver runtime as the wrapper of Driver applications and is the environment in which Drivers execute. You can define it as the software layer that provides necessary services, infrastructure, and behavior for executing the driver. Think of how language runtime work like the JVM, Go Runtime, V8, CPython etc. If you where to write a program in those languages, run in an enviroment that contains your code and the language runtime.

So even when building a Conveyor CI Driver, the final program is a combination of the Driver runtime and your custom Driver code.

![](/img/Driver-Runtime.png)

### What Features does it Provide

As mentioned earlier, the driver runtime provides necessary services and utilities for building CI Drivers. Some of these features are not just exclusive to the runtime but rather and extension of the base features provided by the Conveyor CI Ecosystem. These features include:

- A realtime Event System: The driver runtime is designed to ensure drivers recieve and publish events within the Conveyor System to ensure quick and instant execution of taks.
- Log Management: The Driver runtime provided a [Driver Logger](/docs/concepts/drivers#driver-logger), that provides drivers with functionality to export and save necessary logs in realtime. These can then be fetched or streamed in realtime or for future use.
- Horizontal Scaling: The Driver runtime provides out of the box Horizontal Scaling to Drivers, meaning once you build a driver. It comes with the ability to horizontaly scale into a distributed system effeciently without you writing any custom code. This feature can be really important in Cloud Native environments.
- Conveyor CI API Server Interaction: No driver runtime package is complete without utility functions to interact with the Conveyor API Server to for example create and manipulate Resources.
- Observability out of the Box: The Driver runtime is designed to provide a comprehensive observability platfor that follows all the three prillars that include Metrics, Tracing and Logging.

And many more...

### Driver runtime Compoments

The Driver runtime contains mainly two components. The Driver Manager and the Client Library.

### Driver Manager

The Driver manager, the component that encapsules the Driver and provides utilities that are used by the Driver. By design, it is usually a `Class` or a Class-like data type that has a `run()` method that is called to start the program. Its is resposible for the following functionality.

- Listening for events from Conveyor CI sent via NATS Jetstream: This is accomplished using JetStream [Streams](https://docs.nats.io/nats-concepts/jetstream/streams) and [Consumers](https://docs.nats.io/nats-concepts/jetstream/consumers). The Driver manager contains a Consumer that listens for events from the `messages` stream and filters out subjects depending on the resources that the Driver defines to listen to.
- Provides the Driver Logger to the Driver. The driver logger is a componnet that collects logs from the driver and send them to Grafana Loki for Storage. After storing them it also streams them via a NATS connection on the subject with the following semantics `driver:{DRIVER_NAME}:logs:{RUN_ID}` with the place holders `DRIVER_NAME` and `RUN_ID` refering to the name of the driver and the current run id respectively.
- Runs the `Reconcile` function of the Drivers upon each event from JetStream and passes in the approriate parameters.

![](/img/Driver-Manger-Class-Diagram.png)

### Client Library

The client Library is a component of the driver runtime that contains funtions that interact with the Conveyor CI API Server. these are normal HTTP Requests to the API Server. It also has the ability to fetch Conveyor CI API Server metadata like the API Host and Port.

### Architectural Diagram

## Contributing Guide

### Workflow

### Enviromnment Setup

## Resources

NATS AND JETSTREAM RESOURCES

- [NATS Documentation](https://docs.nats.io/)
- [JetStream Consumers Youtube Video](https://youtu.be/334XuMma1fk?si=-0K7lhFuKDwgj5jj)

CONVEYOR GO PACKAGE AND DRIVER RUNTIME

- [Repository](https://github.com/open-ug/conveyor/tree/main/pkg)

OTHER RESOURCES

- [Sending Logs to Loki Grafana](https://chatgpt.com/share/68549cda-2640-8005-ac59-03a1a6a348c9)
- [Event Driven Architecture Explainer](https://www.confluent.io/learn/event-driven-architecture/)