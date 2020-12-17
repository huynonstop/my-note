https://docs.nestjs.com/

https://academind.com/learn/mongodb/nestjs-mongodb/

https://academind.com/learn/node-js/nestjs-introduction/

[juliandavidmr/awesome-nestjs: 😏 Curated list of NestJS (github.com)](https://github.com/juliandavidmr/awesome-nestjs)

[design patterns - What is dependency injection? - Stack Overflow](https://stackoverflow.com/questions/130794/what-is-dependency-injection)

[Understanding dependency injection - Software Engineering Stack Exchange](https://softwareengineering.stackexchange.com/questions/232229/understanding-dependency-injection)

[Understanding the ES2016 decorator](https://medium.com/google-developers/exploring-es7-decorators-76ecb65fb841)

[Behind the Scenes: How Typescript Decorators Operate | by Netanel Basal | Netanel Basal (netbasal.com)](https://netbasal.com/behind-the-scenes-how-typescript-decorators-operate-28f8dcacb224)

[When use a interface or class in Typescript - Stack Overflow](https://stackoverflow.com/questions/51716808/when-use-a-interface-or-class-in-typescript)

[angular - Difference between interfaces and classes in Typescript - Stack Overflow](https://stackoverflow.com/questions/40973074/difference-between-interfaces-and-classes-in-typescript)

[TypeScript: Handbook - Generics (typescriptlang.org)](https://www.typescriptlang.org/docs/handbook/generics.html)

[TypeScript: Handbook - Interfaces (typescriptlang.org)](https://www.typescriptlang.org/docs/handbook/interfaces.html)

[angular-vietnam/100-days-of-angular: Day 11, 12, 18 -> 26 for TS and RxJS](https://github.com/angular-vietnam/100-days-of-angular)

[Fluent interface - Wikipedia](https://en.wikipedia.org/wiki/Fluent_interface)

Nest (NestJS) is a framework for building efficient, scalable **Node.js** server-side applications. It uses progressive JavaScript, is built with and fully supports **TypeScript** (yet still enables developers to code in pure JavaScript) and combines elements of OOP (Object Oriented Programming), FP (Functional Programming), and FRP (Functional Reactive Programming).

```bash
$ npm i -g @nestjs/cli
$ nest new project-name
```

```txt
src
 |_app.controller.ts				A basic controller with a single route.
 |_app.controller.spec.ts			The unit tests for the controller.
 |_app.module.ts					The root module of the application.
 |_app.service.ts					A basic service with a single method.
 |_main.ts							The entry file of the application which uses the core function NestFactory to create a Nest application instance.
```

```typescript
// main.ts
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
    // const app = await NestFactory.create<NestExpressApplication>(AppModule);
    // const app = await NestFactory.create<NestFastifyApplication>(AppModule);
  await app.listen(3000);
}
bootstrap();
```

# Overview

Controllers, Providers, Modules, Middleware, Exception filters, Pipes, Guards, Interceptors, Custom decorators 

 [NestJS_Overview.md](NestJS_Overview.md) 

# Platform agnosticism

Nest is a platform-agnostic framework. This means you can develop **reusable logical parts** that can be used across different types of applications. For example, most components can be re-used without change across different underlying HTTP server frameworks (e.g., Express and Fastify), and even across different *types* of applications (e.g., HTTP server frameworks, Microservices with different transport layers, and Web Sockets).

## Build once, use everywhere

The **Overview** section of the documentation primarily shows coding techniques using HTTP server frameworks (e.g., apps providing a REST API or providing an MVC-style server-side rendered app). However, all those building blocks can be used on top of different transport layers ([microservices](https://docs.nestjs.com/microservices/basics) or [websockets](https://docs.nestjs.com/websockets/gateways)).

Furthermore, Nest comes with a dedicated [GraphQL](https://docs.nestjs.com/graphql/quick-start) module. You can use GraphQL as your API layer interchangeably with providing a REST API.

In addition, the [application context](https://docs.nestjs.com/application-context) feature helps to create any kind of Node.js application - including things like CRON jobs and CLI apps - on top of Nest.

Nest aspires to be a full-fledged platform for Node.js apps that brings a higher-level of modularity and reusability to your applications. Build once, use everywhere!

# Fundamental

 [NestJS_Fundamental.md](NestJS_Fundamental.md) 

# Technique

[Health checks (Terminus)](https://docs.nestjs.com/recipes/terminus)

[Hot reload](https://docs.nestjs.com/recipes/hot-reload)

[NestJS_Technique.md](NestJS_Technique.md)

# Security

 [NestJS_Security.md](NestJS_Security.md) 

# Testing

 [NestJS_Testing.md](NestJS_Testing.md) 

# CLI

The [Nest CLI](https://github.com/nestjs/nest-cli) is a command-line interface tool that helps you to initialize, develop, and maintain your Nest applications. It assists in multiple ways, including scaffolding the project, serving it in development mode, and building and bundling the application for production distribution. It embodies best-practice architectural patterns to encourage well-structured apps. 

```bash
$ npm install -g @nestjs/cli
```

[NestJS_CLI.md](NestJS_CLI.md) 

# Create Documentation

[Documentation (Compodoc) ](https://docs.nestjs.com/recipes/documentation)

# Standalone applications

There are several ways of mounting a Nest application. You can create a web app, a microservice or just a bare Nest **standalone application** (without any network listeners). The Nest standalone application is a wrapper around the Nest **IoC container**, which holds all instantiated classes. We can obtain a reference to any existing instance from within any imported module directly using the standalone application object. Thus, you can take advantage of the Nest framework anywhere, including, for example, scripted **CRON** jobs. You can even build a **CLI** on top of it.

 [NestJS_SAA.md](NestJS_SAA.md) 

# GRAPHQL

[GraphQL + TypeScript | NestJS - A progressive Node.js framework](https://docs.nestjs.com/graphql/quick-start)

# WEBSOCKETS

[Gateways | NestJS - A progressive Node.js framework](https://docs.nestjs.com/websockets/gateways)

# MICROSERVICES

[Microservices | NestJS - A progressive Node.js framework](https://docs.nestjs.com/microservices/basics)

# Open API

[OpenAPI Specification - Version 3.0.3 | Swagger](https://swagger.io/specification/)

[OpenAPI (Swagger) | NestJS - A progressive Node.js framework](https://docs.nestjs.com/openapi/introduction)