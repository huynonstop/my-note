# Controllers

Controllers are responsible for handling incoming **requests** and returning **responses** to the client.

`@Controller()` decorator, which is **required** to define a basic controller

![img](assets/NestJS_Overview/Controllers_1.png)

## Routing

Using a path prefix in a `@Controller()` decorator allows us to easily group a set of related routes, and minimize repetitive code.

For example, we may choose to group a set of routes that manage interactions with a customer entity under the route `/customers`. In that case, we could specify the path prefix `customers` in the `@Controller()` decorator so that we don't have to repeat that portion of the path for each route in the file.

```typescript
import { Controller, Get } from '@nestjs/common';

@Controller('cats') // prefix for every route
export class CatsController {
  @Get() //  Nest will map GET /cats requests to this handler
  // The route path for a handler is determined by concatenating the (optional) prefix declared for the controller, and any path specified in the request decorator.
  // @Get('profile') => Nest will map GET /cats/profile requests to this handler
  findAll(): string {
    return 'This action returns all cats';
  }
}
```

To create a controller using the CLI, simply execute the `$ nest g controller cats` command.

The `@Get()` HTTP request method decorator before the `findAll()` method tells Nest to create a handler for a specific endpoint for HTTP requests. The endpoint corresponds to the HTTP request method (GET in this case) and the route path.

This method will return a 200 status code and the associated response, which in this case is just a string

[Response](##Response)

## Request object

[Express 4.x - API Reference (expressjs.com) request object](https://expressjs.com/en/api.html)

Handlers often need access to the client **request** details. Nest provides access to the [request object](https://expressjs.com/en/api.html#req) of the underlying platform (Express by default). We can access the request object by instructing Nest to inject it by adding the `@Req()` decorator to the handler's signature.

```typescript
import { Controller, Get, Req } from '@nestjs/common';
import { Request } from 'express';

@Controller('cats')
export class CatsController {
  @Get()
  findAll(@Req() request: Request): string {
    return 'This action returns all cats';
  }
}
```

The request object represents the HTTP request and has properties for the request query string, parameters, HTTP headers, and body (read more [here](https://expressjs.com/en/api.html#req)). In most cases, it's not necessary to grab these properties manually. We can use dedicated decorators instead, such as `@Body()` or `@Query()`, which are available out of the box.

Below is a list of the provided decorators and the plain platform-specific objects they represent.

| `@Request(), @Req()`      | `req`                               |
| ------------------------- | ----------------------------------- |
| `@Response(), @Res()`     | `res`                               |
| `@Next()`                 | `next`                              |
| `@Session()`              | `req.session`                       |
| `@Param(key?: string)`    | `req.params` / `req.params[key]`    |
| `@Body(key?: string)`     | `req.body` / `req.body[key]`        |
| `@Query(key?: string)`    | `req.query` / `req.query[key]`      |
| `@Headers(name?: string)` | `req.headers` / `req.headers[name]` |
| `@Ip()`                   | `req.ip`                            |
| `@HostParam()`            | `req.hosts`                         |

 When using them, you should also import the typings for the underlying library (e.g., `@types/express`) to take full advantage

Note that when you inject either `@Res()` or `@Response()` in a method handler, you put Nest into **Library-specific mode** for that handler, and you become responsible for managing the response. When doing so, you must issue some kind of response by making a call on the `response` object (e.g., `res.json(...)` or `res.send(...)`), or the HTTP server will hang.

## Resources

```typescript
import { Controller, Get, Post } from '@nestjs/common';

@Controller('cats')
export class CatsController {
  @Post()
  create(): string {
    return 'This action adds a new cat';
  }

  @Get()
  findAll(): string {
    return 'This action returns all cats';
  }
}
```

Nest provides decorators for all of the standard HTTP methods: `@Get()`, `@Post()`, `@Put()`, `@Delete()`, `@Patch()`, `@Options()`, and `@Head()`. In addition, `@All()` defines an endpoint that handles all of them.

## Route wildcards

Pattern based routes are supported as well. For instance, the asterisk is used as a wildcard, and will match any combination of characters.

```typescript
@Get('ab*cd')
findAll() {
  return 'This route uses a wildcard';
}
```

The `'ab*cd'` route path will match `abcd`, `ab_cd`, `abecd`, and so on. The characters `?`, `+`, `*`, and `()` may be used in a route path, and are subsets of their regular expression counterparts. The hyphen ( `-`) and the dot (`.`) are interpreted literally by string-based paths.

## Status code

As mentioned, the response **status code** is always **200** by default, except for POST requests which are **201**. We can easily change this behavior by adding the `@HttpCode(...)` decorator at a handler level.

```typescript
import HttpCode from @nestjs/common
@Post()
@HttpCode(204) // static
create() {
  return 'This action adds a new cat';
}
```

Often, your status code isn't static but depends on various factors. In that case, you can use a library-specific **response** (inject using `@Res()`) object (or, in case of an error, throw an exception).

## Headers

To specify a custom response header, you can either use a `@Header()` decorator or a library-specific response object (and call `res.header()` directly).

```typescript
import Header  from @nestjs/common
@Post()
@Header('Cache-Control', 'none')
create() {
  return 'This action adds a new cat';
}
```

## Redirection

To redirect a response to a specific URL, you can either use a `@Redirect()` decorator or a library-specific response object (and call `res.redirect()` directly).

`@Redirect()` takes a required `url` argument, and an optional `statusCode` argument. The `statusCode` defaults to `302` (`Found`) if omitted.

```typescript
@Get()
@Redirect('https://nestjs.com', 301)
```

Sometimes you may want to determine the HTTP status code or the redirect URL dynamically. Do this by returning an object from the route handler method with the shape:

```json
{
  "url": string,
  "statusCode": number
}
```

Returned values will override any arguments passed to the `@Redirect()` decorator. For example:

```typescript
@Get('docs')
@Redirect('https://docs.nestjs.com', 302)
getDocs(@Query('version') version) {
  if (version && version === '5') {
    return { url: 'https://docs.nestjs.com/v5/' };
  }
}
```

## Route parameters

In order to define routes with parameters, we can add route parameter **tokens** in the path of the route to capture the dynamic value at that position in the request URL.

```typescript
import Param  from @nestjs/common
@Get(':id')
findOne(@Param() params): string {
  console.log(params.id);
  return `This action returns a #${params.id} cat`; //  we can access the `id` parameter by referencing `params.id`
}
```

`@Param()` is used to decorate a method parameter (`params` in the example above), and makes the **route** parameters available as properties of that decorated method parameter inside the body of the method.

```typescript
@Get(':id')
findOne(@Param('id') id: string): string { // You can also pass in a particular parameter token to the decorator, and then reference the route parameter directly by name in the method body.
  return `This action returns a #${id} cat`;
}
```

## Request payloads

`@Body()` decorator - client params

 We could determine the DTO schema by using **TypeScript** interfaces, or by simple classes. Interestingly, we recommend using **classes** here. Why? Classes are part of the JavaScript ES6 standard, and therefore they are preserved as real entities in the compiled JavaScript. On the other hand, since TypeScript interfaces are removed during the transpilation, Nest can't refer to them at runtime. This is important because features such as **Pipes** enable additional possibilities when they have access to the metatype of the variable at runtime.

```typescript
// DTO (Data Transfer Object) schema /create-cat.dto.ts
export class CreateCatDto {
  name: string;
  age: number;
  breed: string;
}
```

```typescript
// cats.controller.ts
@Post()
async create(@Body() createCatDto: CreateCatDto) {
  return 'This action adds a new cat';
}
```

## Sample

```typescript
// cats.controller.ts
import { Controller, Get, Query, Post, Body, Put, Param, Delete } from '@nestjs/common';
import { CreateCatDto, UpdateCatDto, ListAllEntities } from './dto';

@Controller('cats')
export class CatsController {
  @Post()
  create(@Body() createCatDto: CreateCatDto) {
    return 'This action adds a new cat';
  }

  @Get()
  findAll(@Query() query: ListAllEntities) {
    return `This action returns all cats (limit: ${query.limit} items)`;
  }

  @Get(':id')
  findOne(@Param('id') id: string) {
    return `This action returns a #${id} cat`;
  }

  @Put(':id')
  update(@Param('id') id: string, @Body() updateCatDto: UpdateCatDto) {
    return `This action updates a #${id} cat`;
  }

  @Delete(':id')
  remove(@Param('id') id: string) {
    return `This action removes a #${id} cat`;
  }
}
// Nest CLI: nest g resource {name}
```

## Controllers registration

Controllers always belong to a module, which is why we include the `controllers` array within the `@Module()` decorator. Since we haven't yet defined any other modules except the root `AppModule`, we'll use that to introduce the `CatsController`

```typescript
// app.module.ts
import { Module } from '@nestjs/common';
import { CatsController } from './cats/cats.controller';

@Module({
  controllers: [CatsController],
})
export class AppModule {}
```

We attached the metadata to the module class using the `@Module()` decorator, and Nest can now easily reflect which controllers have to be mounted.

## Response

Nest employs two **different** options for manipulating responses:

| Standard (recommended) | Using this built-in method, when a request handler returns a JavaScript object or array, it will **automatically** be serialized to JSON. When it returns a JavaScript primitive type (e.g., `string`, `number`, `boolean`), however, Nest will send just the value without attempting to serialize it. This makes response handling simple: just return the value, and Nest takes care of the rest.  Furthermore, the response's **status code** is always 200 by default, except for POST requests which use 201. We can easily change this behavior by adding the `@HttpCode(...)` decorator at a handler-level (see [Status codes](https://docs.nestjs.com/controllers#status-code)). |
| ---------------------- | ------------------------------------------------------------ |
| Library-specific       | We can use the library-specific (e.g., Express) [response object](https://expressjs.com/en/api.html#res), which can be injected using the `@Res()` decorator in the method handler signature (e.g., `findAll(@Res() response)`). With this approach, you have the ability to use the native response handling methods exposed by that object. For example, with Express, you can construct responses using code like `response.status(200).send()`. |

Nest detects when the handler is using either `@Res()` or `@Next()`, indicating you have chosen the **library-specific option**.

If both approaches are used at the same time, the Standard approach is **automatically disabled** for this single route and will no longer work as expected.

To use both approaches at the same time (for example, by injecting the response object to only set cookies/headers but still leave the rest to the framework), you must set the `passthrough` option to `true` in the `@Res({ passthrough: true })` decorator.

### Library-specific approach

[response object](https://expressjs.com/en/api.html#res)

The second way of manipulating the response is to use a library-specific response object. In order to inject a particular response object, we need to use the `@Res()` decorator.

```typescript
import { Controller, Get, Post, Res, HttpStatus } from '@nestjs/common';
import { Response } from 'express';

@Controller('cats')
export class CatsController {
  @Post()
  create(@Res() res: Response) {
    res.status(HttpStatus.CREATED).send();
  }

  @Get()
  findAll(@Res() res: Response) {
     res.status(HttpStatus.OK).json([]);
  }
}
```

> Though this approach works, and does in fact allow for more flexibility in some ways by providing full control of the response object (headers manipulation, library-specific features, and so on), it should be used with care. In general, the approach is much less clear and does have some disadvantages. The main disadvantage is that your code become platform-dependent (as underlying libraries may have different APIs on the response object), and harder to test (you'll have to mock the response object, etc.).

Also, in the example above, you lose compatibility with Nest features that depend on Nest standard response handling, such as Interceptors and `@HttpCode()` / `@Header()` decorators. To fix this, you can set the `passthrough` option to `true`

```typescript
@Get()
findAll(@Res({ passthrough: true }) res: Response) {
  res.status(HttpStatus.OK);
  return [];
}
// Now you can interact with the native response object (for example, set cookies or headers depending on certain conditions), but leave the rest to the framework.
```

## Sub-Domain Routing

The `@Controller` decorator can take a `host` option to require that the HTTP host of the incoming requests matches some specific value.

```typescript
@Controller({ host: 'admin.example.com' })
export class AdminController {
  @Get()
  index(): string {
    return 'Admin page';
  }
}
```

Since **Fastify** lacks support for nested routers, when using sub-domain routing, the (default) Express adapter should be used instead.

Similar to a route `path`, the `hosts` option can use tokens to capture the dynamic value at that position in the host name. The host parameter token in the `@Controller()` decorator example below demonstrates this usage. Host parameters declared in this way can be accessed using the `@HostParam()` decorator, which should be added to the method signature.

```typescript
@Controller({ host: ':account.example.com' })
export class AccountController {
  @Get()
  getInfo(@HostParam('account') account: string) {
    return account;
  }
}
```

## Scopes

In Nest, almost everything is shared across incoming requests. We have a connection pool to the database, singleton services with global state, etc. Remember that Node.js doesn't follow the request/response Multi-Threaded Stateless Model in which every request is processed by a separate thread. Hence, using singleton instances is fully **safe** for our applications.

However, there are edge-cases when request-based lifetime of the controller may be the desired behavior, for instance per-request caching in GraphQL applications, request tracking or multi-tenancy.

## Asynchronicity

https://kamilmysliwiec.com/typescript-2-1-introduction-async-await

Every async function has to return a `Promise`. This means that you can return a deferred value that Nest will be able to resolve by itself. Let's see an example of this:

```typescript
// cats.controller.ts
@Get()
async findAll(): Promise<any[]> {
  return [];
}
```

 Furthermore, Nest route handlers are even more powerful by being able to return RxJS observable streams. Nest will automatically subscribe to the source underneath and take the last emitted value (once the stream is completed).

```typescript
@Get()
findAll(): Observable<any[]> {
  return of([]);
}
```

# Providers (Services)

Providers are a fundamental concept in Nest. Many of the basic Nest classes may be treated as a provider – **services**, **repositories**, **factories**, **helpers**, and so on. 

The main idea of a provider is that it can **inject** dependencies; this means objects can create various relationships with each other, and the function of "wiring up" instances of objects can largely be delegated to the Nest runtime system.

Controllers should handle HTTP requests and delegate more complex tasks to **providers**. Providers are plain JavaScript classes with an `@Injectable()` decorator preceding their class declaration.

![img](assets/NestJS_Overview/Components_1.png)

```typescript
// interfaces/cat.interface.ts
export interface Cat {
  name: string;
  age: number;
  breed: string;
}
// cats.service.ts
import { Injectable } from '@nestjs/common';
import { Cat } from './interfaces/cat.interface';

@Injectable()
export class CatsService {
  private readonly cats: Cat[] = [];

  create(cat: Cat) {
    this.cats.push(cat);
  }

  findAll(): Cat[] {
    return this.cats;
  }
}
// Nest CLI: nest g service {name}
// Our CatsService is a basic class with one property and two methods. The only new feature is that it uses the @Injectable() decorator. The @Injectable() decorator attaches metadata, which tells Nest that this class is a Nest provider.

// cats.controller.ts
import { Controller, Get, Post, Body } from '@nestjs/common';
import { CreateCatDto } from './dto/create-cat.dto';
import { CatsService } from './cats.service';
import { Cat } from './interfaces/cat.interface';

@Controller('cats')
export class CatsController {
  constructor(private catsService: CatsService) {
      // The CatsService is injected through the class constructor. Notice the use of the private syntax. This shorthand allows us to both declare and initialize the catsService member immediately in the same location.
  }

  @Post()
  async create(@Body() createCatDto: CreateCatDto) {
    this.catsService.create(createCatDto);
  }

  @Get()
  async findAll(): Promise<Cat[]> {
    return this.catsService.findAll();
  }
}
```

## Dependency injection

[Why does one use dependency injection? - Stack Overflow](https://stackoverflow.com/questions/14301389/why-does-one-use-dependency-injection)

[Angular - Dependency injection in Angular](https://angular.io/guide/dependency-injection)

```typescript
constructor(private catsService: CatsService) {}
```

In Nest, thanks to TypeScript capabilities, it's extremely easy to manage dependencies because they are resolved just by type. In the example below, Nest will resolve the `catsService` by creating and returning an instance of `CatsService` (or, in the normal case of a singleton, returning the existing instance if it has already been requested elsewhere). This dependency is resolved and passed to your controller's constructor (or assigned to the indicated property)

## Scopes

Providers normally have a lifetime ("scope") synchronized with the application lifecycle. When the application is bootstrapped, every dependency must be resolved, and therefore every provider has to be instantiated. Similarly, when the application shuts down, each provider will be destroyed. However, there are ways to make your provider lifetime **request-scoped** as well.

## Custom providers

Nest has a built-in inversion of control ("IoC") container that resolves relationships between providers. This feature underlies the dependency injection feature described above, but is in fact far more powerful than what we've described so far. The `@Injectable()` decorator is only the tip of the iceberg, and is not the only way to define providers. In fact, you can use plain values, classes, and either asynchronous or synchronous factories. 

## Optional providers

Occasionally, you might have dependencies which do not necessarily have to be resolved. For instance, your class may depend on a **configuration object**, but if none is passed, the default values should be used. In such a case, the dependency becomes optional, because lack of the configuration provider wouldn't lead to errors.

```typescript
import { Injectable, Optional, Inject } from '@nestjs/common';

@Injectable()
export class HttpService<T> {
    // To indicate a provider is optional, use the @Optional() decorator in the constructor's signature.
  constructor(@Optional() @Inject('HTTP_OPTIONS') private httpClient: T) {}
}
```

Note that in the example above we are using a custom provider, which is the reason we include the `HTTP_OPTIONS` custom **token**. Previous examples showed constructor-based injection indicating a dependency through a class in the constructor.

## Property-based injection

The technique we've used so far is called constructor-based injection, as providers are injected via the constructor method. In some very specific cases, **property-based injection** might be useful.

For instance, if your top-level class depends on either one or multiple providers, passing them all the way up by calling `super()` in sub-classes from the constructor can be very tedious. In order to avoid this, you can use the `@Inject()` decorator at the property level.

```typescript
import { Injectable, Inject } from '@nestjs/common';

@Injectable()
export class HttpService<T> {
  @Inject('HTTP_OPTIONS')
  private readonly httpClient: T;
}
```

If your class doesn't extend another provider, you should always prefer using **constructor-based** injection.

## Provider registration

```typescript
// app.module.ts
import { Module } from '@nestjs/common';
import { CatsController } from './cats/cats.controller';
import { CatsService } from './cats/cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
})
export class AppModule {}
```

Nest will now be able to resolve the dependencies of the `CatsController` class.

![image-20201213230547403](assets/NestJS_Overview/image-20201213230547403.png)

## Manual instantiation

Thus far, we've discussed how Nest automatically handles most of the details of resolving dependencies. In certain circumstances, you may need to step outside of the built-in Dependency Injection system and manually retrieve or instantiate providers. We briefly discuss two such topics below.

To get existing instances, or instantiate providers dynamically, you can use Module reference.

To get providers within the `bootstrap()` function (for example for standalone applications without controllers, or to utilize a configuration service during bootstrapping) see Standalone applications.

# Modules

A module is a class annotated with a `@Module()` decorator. The `@Module()` decorator provides metadata that **Nest** makes use of to organize the application structure.

![img](assets/NestJS_Overview/Modules_1.png)

> Each application has at least one module, a **root module**. The root module is the starting point Nest uses to build the **application graph** - the internal data structure Nest uses to resolve module and provider relationships and dependencies. While very small applications may theoretically have just the root module, this is not the typical case. We want to emphasize that modules are **strongly** recommended as an effective way to organize your components. Thus, for most applications, the resulting architecture will employ multiple modules, each encapsulating a closely related set of **capabilities**.

The `@Module()` decorator takes a single object whose properties describe the module:

| `providers`   | the providers that will be instantiated by the Nest injector and that may be shared at least across this module |
| ------------- | ------------------------------------------------------------ |
| `controllers` | the set of controllers defined in this module which have to be instantiated |
| `imports`     | the list of imported modules that export the providers which are required in this module |
| `exports`     | the subset of `providers` that are provided by this module and should be available in other modules which import this module |

The module **encapsulates** providers by default. This means that it's impossible to inject providers that are neither directly part of the current module nor exported from the imported modules. Thus, you may consider the exported providers from a module as the module's public interface, or API.

## Feature modules

The `CatsController` and `CatsService` belong to the same application domain. As they are closely related, it makes sense to move them into a feature module. A feature module simply organizes code relevant for a specific feature, keeping code organized and establishing clear boundaries. (SOLID priciples)

```typescript
// cats/cats.module.ts
import { Module } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
})
export class CatsModule {}
// Nest CLI: $ nest g module {}
```

```typescript
// app.module.ts
import { Module } from '@nestjs/common';
import { CatsModule } from './cats/cats.module';

@Module({
  imports: [CatsModule],
})
export class AppModule {}
```

## Shared modules

In Nest, modules are **singletons** by default, and thus you can share the same instance of any provider between multiple modules effortlessly.

![img](assets/NestJS_Overview/Shared_Module_1.png)

Every module is automatically a **shared module**. Once created it can be reused by any module.

Let's imagine that we want to share an instance of the `CatsService` between several other modules. In order to do that, we first need to **export** the `CatsService` provider by adding it to the module's `exports` array

```typescript
// cats.module.ts
import { Module } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
  exports: [CatsService]
    // Now any module that imports the CatsModule has access to the CatsService and will share the same instance with all other modules that import it as well.
})
export class CatsModule {}
```

## Module re-exporting

As seen above, Modules can export their internal providers. In addition, they can re-export modules that they import. 

In the example below, the `CommonModule` is both imported into **and** exported from the `CoreModule`, making it available for other modules which import this one.

```typescript
@Module({
  imports: [CommonModule],
  exports: [CommonModule],
})
export class CoreModule {}
```

## Dependency injection

```typescript
import { Module } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
})
export class CatsModule {
  constructor(private catsService: CatsService) {
      // A module class can inject providers as well (e.g., for configuration purposes)
      //However, module classes themselves cannot be injected as providers due to circular dependency .
  }
}
```

## Global modules

If you have to import the same set of modules everywhere, it can get tedious. 

Nest, however, encapsulates providers inside the module scope. You aren't able to use a module's providers elsewhere without first importing the encapsulating module.

When you want to provide a set of providers which should be available everywhere out-of-the-box (e.g., helpers, database connections, etc.), make the module **global** with the `@Global()` decorator.

```typescript
import { Module, Global } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Global() // The @Global() decorator makes the module global-scoped. Global modules should be registered only once, generally by the root or core module.
@Module({
  controllers: [CatsController],
  providers: [CatsService],
  exports: [CatsService],
})
export class CatsModule {}
```

In the above example, the `CatsService` provider will be ubiquitous, and modules that wish to inject the service will not need to import the `CatsModule` in their imports array.

Making everything global is not a good design decision. Global modules are available to reduce the amount of necessary boilerplate. The `imports` array is generally the preferred way to make the module's API available to consumers.

## Dynamic modules

This feature enables you to easily create customizable modules that can register and configure providers dynamically.

```typescript
import { Module, DynamicModule } from '@nestjs/common';
import { createDatabaseProviders } from './database.providers';
import { Connection } from './connection.provider';

@Module({
  providers: [Connection],
})
export class DatabaseModule {
  static forRoot(entities = [], options?): DynamicModule {
      // The forRoot() method may return a dynamic module either synchronously or asynchronously (i.e., via a Promise).
    const providers = createDatabaseProviders(options, entities);
    return {
      // global: true, // If you want to register a dynamic module in the global scope, set the global property to true.
      module: DatabaseModule,
      providers: providers,
      exports: providers,
    };
  }
}
/*
This module defines the `Connection` provider by default (in the `@Module()` decorator metadata), but additionally - depending on the `entities` and `options` objects passed into the `forRoot()` method - exposes a collection of providers, for example, repositories. Note that the properties returned by the dynamic module **extend** (rather than override) the base module metadata defined in the `@Module()` decorator. That's how both the statically declared `Connection` provider **and** the dynamically generated repository providers are exported from the module.
*/

// some-where-else.ts
import { Module } from '@nestjs/common';
import { DatabaseModule } from './database/database.module';
import { User } from './users/entities/user.entity';

@Module({
  imports: [DatabaseModule.forRoot([User])],
  exports: [DatabaseModule], // If you want to in turn re-export a dynamic module
})
export class AppModule {}
```

# Middleware

[Using Express middleware (expressjs.com)](https://expressjs.com/en/guide/using-middleware.html)

Middleware is a function which is called **before** the route handler. Middleware functions have access to the [request](https://expressjs.com/en/4x/api.html#req) and [response](https://expressjs.com/en/4x/api.html#res) objects, and the `next()` middleware function in the application’s request-response cycle. The **next** middleware function is commonly denoted by a variable named `next`

![img](assets/NestJS_Overview/Middlewares_1.png)

> Middleware functions can perform the following tasks:
>
> - execute any code.
> - make changes to the request and the response objects.
> - end the request-response cycle.
> - call the next middleware function in the stack.
> - if the current middleware function does not end the request-response cycle, it must call `next()` to pass control to the next middleware function. Otherwise, the request will be left hanging.

You implement custom Nest middleware in either a function, or in a class with an `@Injectable()` decorator. The class should implement the `NestMiddleware` interface, while the function does not have any special requirements

```typescript
// logger.middleware.ts
import { Injectable, NestMiddleware } from '@nestjs/common';
import { Request, Response } from 'express';

@Injectable()
export class LoggerMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: Function) {
    console.log('Request...');
    next();
  }
}
```

## Dependency injection

Nest middleware fully supports Dependency Injection. Just as with providers and controllers, they are able to **inject dependencies** that are available within the same module. As usual, this is done through the `constructor`

## Middleware consumer

The `MiddlewareConsumer` is a helper class. It provides several built-in methods to manage middleware. All of them can be simply chained in the fluent style

## Applying middleware

There is no place for middleware in the `@Module()` decorator. Instead, we set them up using the `configure()` method of the module class. Modules that include middleware have to implement the `NestModule` interface. Let's set up the `LoggerMiddleware` at the `AppModule` level.

```typescript
// app.module.ts
import { Module, NestModule, MiddlewareConsumer } from '@nestjs/common';
import { LoggerMiddleware } from './common/middleware/logger.middleware';
import { CatsModule } from './cats/cats.module';
import { CatsController } from './cats/cats.controller.ts';

@Module({
  imports: [CatsModule],
})
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      // In order to bind multiple middleware that are executed sequentially, simply provide a comma separated list inside the apply() method
      .apply(LoggerMiddleware) // consumer.apply(cors(), helmet(), logger)
      // The forRoutes() method can take a single string, multiple strings, a RouteInfo object, a controller class and even multiple controller classes. In most cases you'll probably just pass a list of controllers separated by commas.
      .forRoutes('cats');
      //.forRoutes({ path: 'cats', method: RequestMethod.GET });
      //.forRoutes(CatsController);
  }
}
// The configure() method can be made asynchronous using async/await (e.g., you can await completion of an asynchronous operation inside the configure() method body).
```

## Route wildcards

Pattern based routes are supported as well. For instance, the asterisk is used as a **wildcard**, and will match any combination of characters:

```typescript
forRoutes({ path: 'ab*cd', method: RequestMethod.ALL });
```

The `'ab*cd'` route path will match `abcd`, `ab_cd`, `abecd`, and so on. The characters `?`, `+`, `*`, and `()` may be used in a route path, and are subsets of their regular expression counterparts. The hyphen ( `-`) and the dot (`.`) are interpreted literally by string-based paths.

The `fastify` package uses the latest version of the `path-to-regexp` package, which no longer supports wildcard asterisks `*`. Instead, you must use parameters (e.g., `(.*)`, `:splat*`).

## Excluding routes

At times we want to **exclude** certain routes from having the middleware applied. We can easily exclude certain routes with the `exclude()` method. This method can take a single string, multiple strings, or a `RouteInfo` object identifying routes to be excluded

```typescript
consumer
  .apply(LoggerMiddleware)
  .exclude( // supports wildcard parameters
    { path: 'cats', method: RequestMethod.GET },
    { path: 'cats', method: RequestMethod.POST },
    'cats/(.*)',
  )
  .forRoutes(CatsController);
```

## Functional middleware

The `LoggerMiddleware` class we've been using is quite simple. It has no members, no additional methods, and no dependencies. Why can't we just define it in a simple function instead of a class?

Consider using the simpler **functional middleware** alternative any time your middleware doesn't need any dependencies

```typescript
// logger.middleware.ts
import { Request, Response } from 'express';

export function logger(req: Request, res: Response, next: Function) {
  console.log(`Request...`);
  next();
};
// app.module.ts
consumer
  .apply(logger)
  .forRoutes(CatsController);
```

## Global middleware

```typescript
const app = await NestFactory.create(AppModule);
app.use(logger);
await app.listen(3000);
```

# Exception filters

Nest comes with a built-in **exceptions layer** which is responsible for processing all unhandled exceptions across an application. When an exception is not handled by your application code, it is caught by this layer, which then automatically sends an appropriate user-friendly response.

![img](assets/NestJS_Overview/Filter_1.png)

Out of the box, this action is performed by a built-in **global exception filter**, which handles exceptions of type `HttpException` (and subclasses of it). When an exception is **unrecognized** (is neither `HttpException` nor a class that inherits from `HttpException`), the built-in exception filter generates the following default JSON response:

```json
{
  "statusCode": 500,
  "message": "Internal server error"
}
```

## Throwing standard exceptions

[HTTP response status codes - HTTP | MDN (mozilla.org)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)

Nest provides a built-in `HttpException` class, exposed from the `@nestjs/common` package. For typical HTTP REST/GraphQL API based applications, it's best practice to send standard HTTP response objects when certain error conditions occur.

```typescript
// cats.controller.ts
@Get()
async findAll() {
  throw new HttpException('Forbidden', HttpStatus.FORBIDDEN);
}
// When the client calls this endpoint, the response looks like this:
{
  "statusCode": 403,
  "message": "Forbidden"
}
//  overriding the entire response body
@Get()
async findAll() {
  throw new HttpException({
    status: HttpStatus.FORBIDDEN,
    error: 'This is a custom message',
  }, HttpStatus.FORBIDDEN);
}
// When the client calls this endpoint, the response looks like this:
{
  "status": 403,
  "error": "This is a custom message"
}
```

To override just the message portion of the JSON response body, supply a string in the `response` argument. To override the entire JSON response body, pass an object in the `response` argument. Nest will serialize the object and return it as the JSON response body.

## Custom exceptions

```typescript
// forbidden.exception.ts
export class ForbiddenException extends HttpException {
  constructor() {
    super('Forbidden', HttpStatus.FORBIDDEN);
  }
}
// cats.controller.ts
@Get()
async findAll() {
  throw new ForbiddenException();
}
```

## Built-in HTTP exceptions

Nest provides a set of standard exceptions that inherit from the base `HttpException`. These are exposed from the `@nestjs/common` package, and represent many of the most common HTTP exceptions:

- `BadRequestException`
- `UnauthorizedException`
- `NotFoundException`
- `ForbiddenException`
- `NotAcceptableException`
- `RequestTimeoutException`
- `ConflictException`
- `GoneException`
- `HttpVersionNotSupportedException`
- `PayloadTooLargeException`
- `UnsupportedMediaTypeException`
- `UnprocessableEntityException`
- `InternalServerErrorException`
- `NotImplementedException`
- `ImATeapotException`
- `MethodNotAllowedException`
- `BadGatewayException`
- `ServiceUnavailableException`
- `GatewayTimeoutException`
- `PreconditionFailedException`

## Exception filters

While the base (built-in) exception filter can automatically handle many cases for you, you may want **full control** over the exceptions layer. For example, you may want to add logging or use a different JSON schema based on some dynamic factors. **Exception filters** are designed for exactly this purpose. They let you control the exact flow of control and the content of the response sent back to the client.

Let's create an exception filter that is responsible for catching exceptions which are an instance of the `HttpException` class, and implementing custom response logic for them.

```typescript
// http-exception.filter.ts
import { ExceptionFilter, Catch, ArgumentsHost, HttpException } from '@nestjs/common';
import { Request, Response } from 'express';
// The @Catch(HttpException) decorator binds the required metadata to the exception filter, telling Nest that this particular filter is looking for exceptions of type HttpException and nothing else. The @Catch() decorator may take a single parameter, or a comma-separated list. This lets you set up the filter for several types of exceptions at once.
@Catch(HttpException)
export class HttpExceptionFilter implements ExceptionFilter {
  catch(exception: HttpException, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const request = ctx.getRequest<Request>();
    const status = exception.getStatus();

    response
      .status(status)
      .json({
        statusCode: status,
        timestamp: new Date().toISOString(),
        path: request.url,
      });
  }
}
```

> All exception filters should implement the generic `ExceptionFilter<T>` interface. This requires you to provide the `catch(exception: T, host: ArgumentsHost)` method with its indicated signature. `T` indicates the type of the exception.

### Arguments host

[Execution context](https://docs.nestjs.com/fundamentals/execution-context)

`ArgumentsHost` is a powerful utility object.

In this code sample, we use it to obtain a reference to the `Request` and `Response` objects that are being passed to the original request handler (in the controller where the exception originates). In this code sample, we've used some helper methods on `ArgumentsHost` to get the desired `Request` and `Response` objects. 

The reason for this level of abstraction is that `ArgumentsHost` functions in all contexts (e.g., the HTTP server context we're working with now, but also Microservices and WebSockets). This will allow us to write generic exception filters that operate across all contexts.

## Binding filters

```typescript
// cats.controller.ts
@Post()
@UseFilters(new HttpExceptionFilter()) // from @nestjs/common
// @UseFilters(HttpExceptionFilter)  // or using like this
async create(@Body() createCatDto: CreateCatDto) {
  throw new ForbiddenException();
}
```

We have used the `@UseFilters()` decorator here. Similar to the `@Catch()` decorator, it can take a single filter instance, or a comma-separated list of filter instances. Here, we created the instance of `HttpExceptionFilter` in place. Alternatively, you may pass the class (instead of an instance), leaving responsibility for instantiation to the framework, and enabling **dependency injection**.

Prefer applying filters by using classes instead of instances when possible. It reduces **memory usage** since Nest can easily reuse instances of the same class across your entire module.

Exception filters can be scoped at different levels: method-scoped, controller-scoped, or global-scoped.

```typescript
// cats.controller.ts

@UseFilters(new HttpExceptionFilter()) // controller-scoped
export class CatsController {
    @Post()
    @UseFilters(new HttpExceptionFilter()) // method-scoped
    async create(@Body() createCatDto: CreateCatDto) {
      throw new ForbiddenException();
    }  
}
// main.ts
async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalFilters(new HttpExceptionFilter()); // global-scoped filter
  await app.listen(3000);
}
bootstrap();
```

The `useGlobalFilters()` method does not set up filters for gateways or hybrid applications.

Global-scoped filters are used across the whole application, for every controller and every route handler. 

In terms of dependency injection, global filters registered from outside of any module (with `useGlobalFilters()` as in the example above) cannot inject dependencies since this is done outside the context of any module. In order to solve this issue, you can register a global-scoped filter **directly from any module** using the following construction:

```typescript
import { Module } from '@nestjs/common';
import { APP_FILTER } from '@nestjs/core';

@Module({
  providers: [
    {
      provide: APP_FILTER,
      useClass: HttpExceptionFilter,
    },
  ],
})
export class AppModule {}
```

You can add as many filters with this technique as needed; simply add each to the providers array.

## Catch everything

In order to catch **every** unhandled exception (regardless of the exception type), leave the `@Catch()` decorator's parameter list empty, e.g., `@Catch()`.

```typescript
import {
  ExceptionFilter,
  Catch,
  ArgumentsHost,
  HttpException,
  HttpStatus,
} from '@nestjs/common';

@Catch()
export class AllExceptionsFilter implements ExceptionFilter {
  catch(exception: unknown, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse();
    const request = ctx.getRequest();

    const status =
      exception instanceof HttpException
        ? exception.getStatus()
        : HttpStatus.INTERNAL_SERVER_ERROR;

    response.status(status).json({
      statusCode: status,
      timestamp: new Date().toISOString(),
      path: request.url,
    });
  }
}
```

## Inheritance

 However, there might be use-cases when you would like to simply extend the built-in default **global exception filter**, and override the behavior based on certain factors.

In order to delegate exception processing to the base filter, you need to extend `BaseExceptionFilter` and call the inherited `catch()` method.

```typescript
// all-exceptions.filter.ts
import { Catch, ArgumentsHost } from '@nestjs/common';
import { BaseExceptionFilter } from '@nestjs/core';

@Catch()
export class AllExceptionsFilter extends BaseExceptionFilter {
  catch(exception: unknown, host: ArgumentsHost) {
    super.catch(exception, host);
  }
}
// Method-scoped and Controller-scoped filters that extend the BaseExceptionFilter should not be instantiated with new. Instead, let the framework instantiate them automatically.
```

Global filters **can** extend the base filter. This can be done in either of two ways.

The first method is to inject the `HttpServer` reference when instantiating the custom global filter:

```typescript
async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  const { httpAdapter } = app.get(HttpAdapterHost);
  app.useGlobalFilters(new AllExceptionsFilter(httpAdapter));

  await app.listen(3000);
}
bootstrap();
```

The second method is to use the `APP_FILTER` token

# Pipes

A pipe is a class annotated with the `@Injectable()` decorator. Pipes should implement the `PipeTransform` interface.

![img](assets/NestJS_Overview/Pipe_1.png)

Pipes have two typical use cases:

- **transformation**: transform input data to the desired form (e.g., from string to integer)
- **validation**: evaluate input data and if valid, simply pass it through unchanged; otherwise, throw an exception when the data is incorrect

In both cases, pipes operate on the `arguments` being processed by a controller route handler. Nest interposes a pipe just before a method is invoked, and the pipe receives the arguments destined for the method and operates on them. Any transformation or validation operation takes place at that time, after which the route handler is invoked with any (potentially) transformed arguments.

Nest comes with a number of built-in pipes that you can use out-of-the-box. You can also build your own custom pipes.

Pipes run inside the exceptions zone. This means that when a Pipe throws an exception it is handled by the exceptions layer (global exceptions filter and any exceptions filters that are applied to the current context).Given the above, it should be clear that when an exception is thrown in a Pipe, no controller method is subsequently executed. This gives you a best-practice technique for validating data coming into the application from external sources at the system boundary.

## Built-in pipes

Nest comes with six pipes available out-of-the-box:

- `ValidationPipe`
- `ParseIntPipe`
- `ParseBoolPipe`
- `ParseArrayPipe`
- `ParseUUIDPipe`
- `DefaultValuePipe`

They're exported from the `@nestjs/common` package.

## Binding pipes

```typescript
@Get(':id')
async findOne(@Param('id', ParseIntPipe) id: number) {
  return this.catsService.findOne(id);
}
// This ensures that one of the following two conditions is true: either the parameter we receive in the findOne() method is a number (as expected in our call to this.catsService.findOne()), or an exception is thrown before the route handler is called.

// GET localhost:3000/abc
{
  "statusCode": 400,
  "message": "Validation failed (numeric string is expected)",
  "error": "Bad Request"
}

// The exception will prevent the body of the findOne() method from executing.
```

In the example above, we pass a class (`ParseIntPipe`), not an instance, leaving responsibility for instantiation to the framework and enabling dependency injection. As with pipes and guards, we can instead pass an in-place instance. Passing an in-place instance is useful if we want to customize the built-in pipe's behavior by passing options

```typescript
@Get(':id')
async findOne(
  @Param('id', new ParseIntPipe({ errorHttpStatusCode: HttpStatus.NOT_ACCEPTABLE }))
  id: number,
) {
  return this.catsService.findOne(id);
}
```

Binding the other transformation pipes (all of the **Parse\*** pipes) works similarly. For example with a query string parameter:

```typescript
@Get()
async findOne(@Query('id', ParseIntPipe) id: number) { // When using ParseUUIDPipe() you are parsing UUID in version 3, 4 or 5, if you only require a specific version of UUID you can pass a version in the pipe options.
  return this.catsService.findOne(id);
}
```

## Custom pipes

```typescript
import { PipeTransform, Injectable, ArgumentMetadata } from '@nestjs/common';

@Injectable()
export class ValidationPipe implements PipeTransform {
  transform(value: any, metadata: ArgumentMetadata) {
    return value;
  }
}
// PipeTransform<T, R> is a generic interface that must be implemented by any pipe. The generic interface uses T to indicate the type of the input value, and R to indicate the return type of the transform() method.
```

Every pipe must implement the `transform()` method to fulfill the `PipeTransform` interface contract. This method has two parameters:

- `value`
- `metadata`

The `value` parameter is the currently processed method argument (before it is received by the route handling method), and `metadata` is the currently processed method argument's metadata. The metadata object has these properties:

```typescript
export interface ArgumentMetadata {
  type: 'body' | 'query' | 'param' | 'custom'; // Indicates whether the argument is a body @Body(), query @Query(), param @Param(), or a custom parameter decorator
  metatype?: Type<unknown>; // Provides the metatype of the argument, for example, String. Note: the value is undefined if you either omit a type declaration in the route handler method signature, or use vanilla JavaScript.
  data?: string; // The string passed to the decorator, for example @Body('string'). It's undefined if you leave the decorator parenthesis empty.
}
```

These properties describe the currently processed argument.

TypeScript interfaces disappear during transpilation. Thus, if a method parameter's type is declared as an interface instead of a class, the `metatype` value will be `Object`.

## Schema based validation

```typescript
// create-cat.dto.ts
export class CreateCatDto {
  name: string;
  age: number;
  breed: string;
}

@Post()
async create(@Body() createCatDto: CreateCatDto) { // we probably would like to ensure that the post body object is valid before attempting to run our service method.
  this.catsService.create(createCatDto);
}
```

We want to ensure that any incoming request to the create method contains a valid body. So we have to validate the three members of the `createCatDto` object. We could do this inside the route handler method, but doing so is not ideal as it would break the **single responsibility rule** (SRP).

Another approach could be to create a **validator class** and delegate the task there. This has the disadvantage that we would have to remember to call this validator at the beginning of each method.

How about creating validation middleware? This could work, but unfortunately it's not possible to create **generic middleware** which can be used across all contexts across the whole application. This is because middleware is unaware of the **execution context**, including the handler that will be called and any of its parameters.

## Object schema validation

There are several approaches available for doing object validation in a clean, [DRY](https://en.wikipedia.org/wiki/Don't_repeat_yourself) way. One common approach is to use **schema-based** validation. Let's go ahead and try that approach.

The [Joi](https://github.com/hapijs/joi) library allows you to create schemas in a straightforward way, with a readable API. Let's build a validation pipe that makes use of Joi-based schemas.

```bash
$ npm install --save @hapi/joi
$ npm install --save-dev @types/hapi__joi
```

```typescript
import { PipeTransform, Injectable, ArgumentMetadata, BadRequestException } from '@nestjs/common';
import { ObjectSchema } from '@hapi/joi';

@Injectable()
export class JoiValidationPipe implements PipeTransform {
  constructor(private schema: ObjectSchema) {}

  transform(value: any, metadata: ArgumentMetadata) {
    const { error } = this.schema.validate(value);
    if (error) {
      throw new BadRequestException('Validation failed');
    }
    return value;
  }
}
```

## Binding validation pipes

Binding validation pipes is also very straightforward.

Pipes can be parameter-scoped, method-scoped, controller-scoped, or global-scoped

Using the `@UsePipes()` decorator. Doing so makes our validation pipe re-usable across contexts, just as we set out to do.

In this case, we want to bind the pipe at the method call level. In our current example, we need to do the following to use the `JoiValidationPipe`:

1. Create an instance of the `JoiValidationPipe`
2. Pass the context-specific Joi schema in the class constructor of the pipe
3. Bind the pipe to the method

```typescript
@Post()
@UsePipes(new JoiValidationPipe(createCatSchema)) // @nestjs/common
async create(@Body() createCatDto: CreateCatDto) {
  this.catsService.create(createCatDto);
}
```

## Class validator

[typestack/class-validator: Decorator-based property validation for classes. (github.com)](https://github.com/typestack/class-validator)

require TypeScript

Nest works well with the [class-validator](https://github.com/typestack/class-validator) library. This powerful library allows you to use decorator-based validation. Decorator-based validation is extremely powerful, especially when combined with Nest's **Pipe** capabilities since we have access to the `metatype` of the processed property.

```bash
$ npm i --save class-validator class-transformer
```

```typescript
// create-cat.dto.ts
import { IsString, IsInt } from 'class-validator';

export class CreateCatDto {
  @IsString()
  name: string;

  @IsInt()
  age: number;

  @IsString()
  breed: string;
}

// validation.pipe.ts
import { PipeTransform, Injectable, ArgumentMetadata, BadRequestException } from '@nestjs/common';
import { validate } from 'class-validator';
import { plainToClass } from 'class-transformer'; // https://github.com/typestack/class-transformer

@Injectable()
export class ValidationPipe implements PipeTransform<any> {
  async transform(value: any, { metatype }: ArgumentMetadata) { // Nest supports both synchronous and asynchronous pipes.
    if (!metatype || !this.toValidate(metatype)) {
      // bypassing the validation step when the current argument being processed is a native JavaScript type (these can't have validation decorators attached, so there's no reason to run them through the validation step).
      return value;
    }
    const object = plainToClass(metatype, value); //  transform our plain JavaScript argument object into a typed object so that we can apply validation.
    const errors = await validate(object);
    if (errors.length > 0) {
      throw new BadRequestException('Validation failed');
    }
    return value;
  }

  private toValidate(metatype: Function): boolean {
    const types: Function[] = [String, Boolean, Number, Array, Object];
    return !types.includes(metatype);
  }
}

// cats.controller.ts
@Post()
async create(
  @Body(new ValidationPipe()) createCatDto: CreateCatDto, // we'll bind the pipe instance to the route handler @Body() decorator so that our pipe is called to validate the post body.
) {
  this.catsService.create(createCatDto);
}
// Parameter-scoped pipes are useful when the validation logic concerns only one specified parameter.
```

> The reason we must transform our plain JavaScript argument object into a typed object is that the incoming post body object, when deserialized from the network request, does **not have any type information** (this is the way the underlying platform, such as Express, works). Class-validator needs to use the validation decorators we defined for our DTO earlier, so we need to perform this transformation to treat the incoming body as an appropriately decorated object, not just a plain vanilla object.

## Global scoped pipes

```typescript
// main.ts
async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalPipes(new ValidationPipe());
  await app.listen(3000);
}
bootstrap();
// In the case of hybrid apps the useGlobalPipes() method doesn't set up pipes for gateways and micro services. For "standard" (non-hybrid) microservice apps, useGlobalPipes() does mount pipes globally.
// Global pipes are used across the whole application, for every controller and every route handler.
```

Note that in terms of dependency injection, global pipes registered from outside of any module (with `useGlobalPipes()` as in the example above) cannot inject dependencies since the binding has been done outside the context of any module. In order to solve this issue, you can set up a global pipe **directly from any module** using the following construction:

```typescript
// app.module.ts
import { Module } from '@nestjs/common';
import { APP_PIPE } from '@nestjs/core';

@Module({
  providers: [
    {
      provide: APP_PIPE,
      useClass: ValidationPipe,
    },
  ],
})
export class AppModule {}
// When using this approach to perform dependency injection for the pipe, note that regardless of the module where this construction is employed, the pipe is, in fact, global. Where should this be done? Choose the module where the pipe (ValidationPipe in the example above) is defined. Also, useClass is not the only way of dealing with custom provider registration.
```

## Transformation use case

A pipe can also **transform** the input data to the desired format. This is possible because the value returned from the `transform` function completely overrides the previous value of the argument.

Consider that sometimes the data passed from the client needs to undergo some change - for example converting a string to an integer - before it can be properly handled by the route handler method. Furthermore, some required data fields may be missing, and we would like to apply default values. **Transformation pipes** can perform these functions by interposing a processing function between the client request and the request handler.

```typescript
// parse-int.pipe.ts
import { PipeTransform, Injectable, ArgumentMetadata, BadRequestException } from '@nestjs/common';

@Injectable()
export class ParseIntPipe implements PipeTransform<string, number> {
  transform(value: string, metadata: ArgumentMetadata): number {
    const val = parseInt(value, 10);
    if (isNaN(val)) {
      throw new BadRequestException('Validation failed');
    }
    return val;
  }
}

// Params-scope
@Get(':id')
async findOne(@Param('id', new ParseIntPipe()) id) {
  return this.catsService.findOne(id);
}
```

```typescript
// select an existing user entity from the database using an id supplied in the request
@Get(':id')
findOne(@Param('id', UserByIdPipe) userEntity: UserEntity) {
  return userEntity;
}
```

## Providing defaults

`Parse*` pipes expect a parameter's value to be defined. They throw an exception upon receiving `null` or `undefined` values.

The `DefaultValuePipe` allow an endpoint to handle missing query-string parameter values by providing a default value to be injected before the `Parse*` pipes operate on these values

```typescript
@Get()
async findAll(
  @Query('activeOnly', new DefaultValuePipe(false), ParseBoolPipe) activeOnly: boolean,
  @Query('page', new DefaultValuePipe(0), ParseIntPipe) page: number,
) {
  return this.catsService.findAll({ activeOnly, page });
}
```

# Guards

A guard is a class annotated with the `@Injectable()` decorator. Guards should implement the `CanActivate` interface.

![img](assets/NestJS_Overview/Guards_1.png)

> Guards have a **single responsibility**. They determine whether a given request will be handled by the route handler or not, depending on certain conditions (like permissions, roles, ACLs, etc.) present at run-time. This is often referred to as **authorization**. Authorization (and its cousin, **authentication**, with which it usually collaborates) has typically been handled by **middleware** in traditional **Express** applications.
>
> Middleware is a fine choice for authentication, since things like token validation and attaching properties to the `request` object are not strongly connected with a particular route context (and its metadata).
>
> But middleware, by its nature, is dumb. It doesn't know which handler will be executed after calling the `next()` function. On the other hand, **Guards** have access to the `ExecutionContext` instance, and thus know exactly what's going to be executed next. They're designed, much like **exception filters**, **pipes**, and **interceptors**, to let you interpose processing logic at exactly the right point in the request/response cycle, and to do so declaratively. This helps keep your code DRY and declarative.

Guards are executed **after** each middleware, but **before** any interceptor or pipe.

Every guard must implement a `canActivate()` function. This function should return a boolean, indicating whether the current request is allowed or not. It can return the response either synchronously or asynchronously (via a `Promise` or `Observable`). Nest uses the return value to control the next action:

- if it returns `true`, the request will be processed.
- if it returns `false`, Nest will deny the request.

## Execution context

The `canActivate()` function takes a single argument, the `ExecutionContext` instance. The `ExecutionContext` inherits from `ArgumentsHost` (already in [Arguments host](#Arguments host))

By extending `ArgumentsHost`, `ExecutionContext` also adds several new helper methods that provide additional details about the current execution process. These details can be helpful in building more generic guards that can work across a broad set of controllers, methods, and execution contexts.

## Authorization guard

**Authorization** is a great use case for Guards because specific routes should be available only when the caller (usually a specific authenticated user) has sufficient permissions.

```typescript
// auth.guard.ts
import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common';
import { Observable } from 'rxjs';

@Injectable()
export class AuthGuard implements CanActivate {
  canActivate(
    context: ExecutionContext,
  ): boolean | Promise<boolean> | Observable<boolean> {
    const request = context.switchToHttp().getRequest();
    return validateRequest(request);
  }
}
```

## Let's build a custom role-based authentication guard

```typescript
// roles.guard.ts
import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common';
import { Observable } from 'rxjs';

@Injectable()
export class RolesGuard implements CanActivate {
  canActivate(
    context: ExecutionContext,
  ): boolean | Promise<boolean> | Observable<boolean> {
    return true;
  }
}
```

### Binding guards

Like pipes and exception filters, guards can be **controller-scoped**, method-scoped, or global-scoped.

`@UseGuards()` decorator may take a single argument, or a comma-separated list of arguments.

```typescript
@Controller('cats')
@UseGuards(RolesGuard) // @nestjs/common
// we passed the RolesGuard type (instead of an instance), leaving responsibility for instantiation to the framework and enabling dependency injection.
export class CatsController {
    //  controller-scoped
}

// or pass an instance
@Controller('cats')
@UseGuards(new RolesGuard())
export class CatsController {}
```

If we wish the guard to apply only to a single method, we apply the `@UseGuards()` decorator at the **method level**.

```typescript
// global guard
const app = await NestFactory.create(AppModule);
app.useGlobalGuards(new RolesGuard());
// In the case of hybrid apps the useGlobalGuards() method doesn't set up guards for gateways and micro services by default (see Hybrid application for information on how to change this behavior). For "standard" (non-hybrid) microservice apps, useGlobalGuards() does mount the guards globally.
```

Global guards are used across the whole application, for every controller and every route handler. In terms of dependency injection, global guards registered from outside of any module (with `useGlobalGuards()` as in the example above) cannot inject dependencies since this is done outside the context of any module. In order to solve this issue, you can set up a guard directly from any module using the following construction:

```typescript
// app.module.ts
import { Module } from '@nestjs/common';
import { APP_GUARD } from '@nestjs/core';

@Module({
  providers: [
    {
      provide: APP_GUARD,
      useClass: RolesGuard,
    },
  ],
})
export class AppModule {}
// When using this approach to perform dependency injection for the guard, note that regardless of the module where this construction is employed, the guard is, in fact, global. Where should this be done? Choose the module where the guard (RolesGuard in the example above) is defined. Also, useClass is not the only way of dealing with custom provider registration
```

### Setting roles per handler

Our guard doesn't yet know about roles, or which roles are allowed for each handler. (execution context)

Nest provides the ability to attach custom **metadata** to route handlers through the `@SetMetadata()` decorator. This metadata supplies our missing `role` data, which a smart guard needs to make decisions.

```typescript
// cats.controller.ts
@Post()
@SetMetadata('roles', ['admin']) // @nestjs/common
async create(@Body() createCatDto: CreateCatDto) {
  this.catsService.create(createCatDto);
}

// or

// roles.decorator.ts
import { SetMetadata } from '@nestjs/common';
export const Roles = (...roles: string[]) => SetMetadata('roles', roles);

// Now that we have a custom @Roles() decorator, we can use it to decorate the create() method.
// cats.controller.ts
@Post()
@Roles('admin')
async create(@Body() createCatDto: CreateCatDto) {
  this.catsService.create(createCatDto);
}
```

### Putting it all together

[Reflect - JavaScript | MDN (mozilla.org)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect)

We want `RolesGuard` to make the return value conditional based on the comparing the **roles assigned to the current user** to the actual roles required by the current route being processed. In order to access the route's role(s) (custom metadata)

```typescript
// roles.guard.ts
import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common';
import { Reflector } from '@nestjs/core';

@Injectable()
export class RolesGuard implements CanActivate {
  constructor(private reflector: Reflector) {}

  canActivate(context: ExecutionContext): boolean {
    const roles = this.reflector.get<string[]>('roles', context.getHandler());
    if (!roles) {
      return true;
    }
    const request = context.switchToHttp().getRequest();
    const user = request.user;
    return matchRoles(roles, user.roles);
  }
}
```

In the node.js world, it's common practice to attach the authorized user to the `request` object. Thus, in our sample code above, we are assuming that `request.user` contains the user instance and allowed roles.

In your app, you will probably make that association in your custom **authentication guard** (or middleware)

When a user with insufficient privileges requests an endpoint, Nest automatically returns the following response:

```typescript
{
  "statusCode": 403,
  "message": "Forbidden resource",
  "error": "Forbidden"
}
```

Note that behind the scenes, when a guard returns `false`, the framework throws a `ForbiddenException`. If you want to return a different error response, you should throw your own specific exception. For example:

```typescript
throw new UnauthorizedException();
```

Any exception thrown by a guard will be handled by the exceptions layer (global exceptions filter and any exceptions filters that are applied to the current context).

# Interceptors

[Aspect-oriented programming - Wikipedia](https://en.wikipedia.org/wiki/Aspect-oriented_programming)

[ReactiveX/rxjs: A reactive programming library for JavaScript (github.com)](https://github.com/ReactiveX/rxjs)

An interceptor is a class annotated with the `@Injectable()` decorator. Interceptors should implement the `NestInterceptor` interface.

![img](assets/NestJS_Overview/Interceptors_1.png)

Interceptors have a set of useful capabilities which are inspired by the Aspect Oriented Programming (AOP) technique. They make it possible to:

- bind **extra logic** before / after method execution
- **transform** the result returned from a function
- **transform** the exception thrown from a function
- **extend** the basic function behavior
- completely **override** a function depending on specific conditions (e.g., for caching purposes)

Each interceptor implements the `intercept()` method, which takes two arguments. The first one is the `ExecutionContext` instance which inherits from `ArgumentsHost`. It's a wrapper around arguments that have been passed to the original handler, and contains different arguments arrays based on the type of the application. The second argument is a `CallHandler`

By extending `ArgumentsHost`, `ExecutionContext` also adds several new helper methods that provide additional details about the current execution process. These details can be helpful in building more generic interceptors that can work across a broad set of controllers, methods, and execution contexts.

## Call handler

[Pointcut - Wikipedia](https://en.wikipedia.org/wiki/Pointcut)

The `CallHandler` interface implements the `handle()` method, which you can use to invoke the route handler method at some point in your interceptor. If you don't call the `handle()` method in your implementation of the `intercept()` method, the route handler method won't be executed at all.

> This approach means that the `intercept()` method effectively **wraps** the request/response stream. As a result, you may implement custom logic **both before and after** the execution of the final route handler. It's clear that you can write code in your `intercept()` method that executes **before** calling `handle()`, but how do you affect what happens afterward? Because the `handle()` method returns an `Observable`, we can use powerful RxJS operators to further manipulate the response. Using Aspect Oriented Programming terminology, the invocation of the route handler (i.e., calling `handle()`) is called a Pointcut, indicating that it's the point at which our additional logic is inserted.

Consider, for example, an incoming `POST /cats` request. This request is destined for the `create()` handler defined inside the `CatsController`. If an interceptor which does not call the `handle()` method is called anywhere along the way, the `create()` method won't be executed. Once `handle()` is called (and its `Observable` has been returned), the `create()` handler will be triggered. And once the response stream is received via the `Observable`, additional operations can be performed on the stream, and a final result returned to the caller.

## Aspect interception

The first use case we'll look at is to use an interceptor to log user interaction (e.g., storing user calls, asynchronously dispatching events or calculating a timestamp)

```typescript
// logging.interceptor.ts
import { Injectable, NestInterceptor, ExecutionContext, CallHandler } from '@nestjs/common';
import { Observable } from 'rxjs';
import { tap } from 'rxjs/operators';

@Injectable() // Interceptors, like controllers, providers, guards, and so on, can inject dependencies through their constructor.
export class LoggingInterceptor implements NestInterceptor { // The NestInterceptor<T, R> is a generic interface in which T indicates the type of an Observable<T> (supporting the response stream), and R is the type of the value wrapped by Observable<R>.
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    console.log('Before...');

    const now = Date.now();
    // Since handle() returns an RxJS Observable, we have a wide choice of operators we can use to manipulate the stream.
    return next
      .handle()
      .pipe(
        tap(() => console.log(`After... ${Date.now() - now}ms`)),
      );
  }
}
```

## Binding interceptors

Interceptors can be controller-scoped, method-scoped, or global-scoped.

```typescript
// cats.controller.ts
@UseInterceptors(LoggingInterceptor) // @nestjs/common
export class CatsController {}
```

Using the above construction, each route handler defined in `CatsController` will use `LoggingInterceptor`. When someone calls the `GET /cats` endpoint, you'll see the following output in your standard output:

```typescript
Before...
After... 1ms
```

Note that we passed the `LoggingInterceptor` type (instead of an instance), leaving responsibility for instantiation to the framework and enabling dependency injection. As with pipes, guards, and exception filters, we can also pass an in-place instance:

```typescript
// cats.controller.ts
@UseInterceptors(new LoggingInterceptor())
export class CatsController {}
```

If we want to restrict the interceptor's scope to a single method, we simply apply the decorator at the **method level**.

In order to set up a global interceptor, we use the `useGlobalInterceptors()` method of the Nest application instance:

```typescript
const app = await NestFactory.create(AppModule);
app.useGlobalInterceptors(new LoggingInterceptor());
```

Global interceptors are used across the whole application, for every controller and every route handler. In terms of dependency injection, global interceptors registered from outside of any module (with `useGlobalInterceptors()`, as in the example above) cannot inject dependencies since this is done outside the context of any module. In order to solve this issue, you can set up an interceptor **directly from any module** using the following construction:

```typescript
// app.module.tsJS
import { Module } from '@nestjs/common';
import { APP_INTERCEPTOR } from '@nestjs/core';

@Module({
  providers: [
    {
      provide: APP_INTERCEPTOR,
      useClass: LoggingInterceptor,
    },
  ],
})
export class AppModule {}
// When using this approach to perform dependency injection for the interceptor, note that regardless of the module where this construction is employed, the interceptor is, in fact, global. Where should this be done? Choose the module where the interceptor (LoggingInterceptor in the example above) is defined. Also, useClass is not the only way of dealing with custom provider registration.
```

## Response mapping

`handle()` returns an `Observable`. The stream contains the value **returned** from the route handler, and thus we can easily mutate it using RxJS's `map()` operator.

The response mapping feature doesn't work with the library-specific response strategy (using the `@Res()` object directly is forbidden).

```typescript
// transform.interceptor.ts
import { Injectable, NestInterceptor, ExecutionContext, CallHandler } from '@nestjs/common';
import { Observable } from 'rxjs';
import { map } from 'rxjs/operators';

export interface Response<T> {
  data: T;
}

@Injectable()
export class TransformInterceptor<T> implements NestInterceptor<T, Response<T>>  // Nest interceptors work with both synchronous and asynchronous intercept() methods. You can simply switch the method to async if necessary.
  intercept(context: ExecutionContext, next: CallHandler): Observable<Response<T>> {
    return next.handle().pipe(map(data => ({ data })));
  }
}

// when someone calls the GET /cats endpoint, the response would look like the following (assuming that route handler returns an empty array []):
{
  "data": []
}

// Another one
import { Injectable, NestInterceptor, ExecutionContext, CallHandler } from '@nestjs/common';
import { Observable } from 'rxjs';
import { map } from 'rxjs/operators';

@Injectable()
export class ExcludeNullInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    return next
      .handle()
      .pipe(map(value => value === null ? '' : value ));
  }
}
```

## Exception mapping

```typescript
// errors.interceptor.ts
import {
  Injectable,
  NestInterceptor,
  ExecutionContext,
  BadGatewayException,
  CallHandler,
} from '@nestjs/common';
import { Observable, throwError } from 'rxjs';
import { catchError } from 'rxjs/operators';

@Injectable()
export class ErrorsInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    return next
      .handle()
      .pipe(
        catchError(err => throwError(new BadGatewayException())),
      );
  }
}
```

## Stream overriding

There are several reasons why we may sometimes want to completely prevent calling the handler and return a different value instead. An obvious example is to implement a cache to improve response time.

```typescript
// cache.interceptor.ts
import { Injectable, NestInterceptor, ExecutionContext, CallHandler } from '@nestjs/common';
import { Observable, of } from 'rxjs';

@Injectable()
export class CacheInterceptor implements NestInterceptor {
  // In a realistic for caching, we'd want to consider other factors like TTL, cache invalidation, cache size, etc., but that's beyond the scope of this discussion. Here we'll provide a basic example that demonstrates the main concept.
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    const isCached = true;
    if (isCached) {
      return of([]);
    }
    return next.handle();
  }
}
```

The key point to note is that we return a new stream here, created by the RxJS `of()` operator, therefore the route handler **won't be called** at all. When someone calls an endpoint that makes use of `CacheInterceptor`, the response (a hardcoded, empty array) will be returned immediately. In order to create a generic solution, you can take advantage of `Reflector` and create a custom decorator.

## More operators

```typescript
// timeout.interceptor.ts
import { Injectable, NestInterceptor, ExecutionContext, CallHandler, RequestTimeoutException } from '@nestjs/common';
import { Observable, throwError, TimeoutError } from 'rxjs';
import { catchError, timeout } from 'rxjs/operators';

@Injectable()
export class TimeoutInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    return next.handle().pipe(
      timeout(5000),
      catchError(err => {
        if (err instanceof TimeoutError) {
          return throwError(new RequestTimeoutException());
        }
        return throwError(err);
      }),
    );
  };
};
```

# Custom route decorators

An ES2016 decorator is an expression which returns a function and can take a target, name and property descriptor as arguments. You apply it by prefixing the decorator with an `@` character and placing this at the very top of what you are trying to decorate. Decorators can be defined for either a class, a method or a property.

## Param decorators

Nest provides a set of useful **param decorators** that you can use together with the HTTP route handlers.

| Provided decorators        | Plain objects                        |
| -------------------------- | ------------------------------------ |
| `@Request(), @Req()`       | `req`                                |
| `@Response(), @Res()`      | `res`                                |
| `@Next()`                  | `next`                               |
| `@Session()`               | `req.session`                        |
| `@Param(param?: string)`   | `req.params` / `req.params[param]`   |
| `@Body(param?: string)`    | `req.body` / `req.body[param]`       |
| `@Query(param?: string)`   | `req.query` / `req.query[param]`     |
| `@Headers(param?: string)` | `req.headers` / `req.headers[param]` |
| `@Ip()`                    | `req.ip`                             |
| `@HostParam()`             | `req.hosts`                          |

``` typescript
const user = req.user; // node.js things

// In order to make your code more readable and transparent, you can create a @User() decorator and reuse it across all of your controllers.

// user.decorator.ts
import { createParamDecorator, ExecutionContext } from '@nestjs/common';

export const User = createParamDecorator(
  (data: unknown, ctx: ExecutionContext) => {
    const request = ctx.switchToHttp().getRequest();
    return request.user;
  },
);

// then use
@Get()
async findOne(@User() user: UserEntity) {
  console.log(user);
}
```

`createParamDecorator<T>()` is a generic. This means you can explicitly enforce type safety, for example `createParamDecorator<string>((data, ctx) => ...)`. Alternatively, specify a parameter type in the factory function, for example `createParamDecorator((data: string, ctx) => ...)`. If you omit both, the type for `data` will be `any`.

## Passing data

When the behavior of your decorator depends on some conditions, you can use the `data` parameter to pass an argument to the decorator's factory function. One use case for this is a custom decorator that extracts properties from the request object by key.

```json
// user entity for an authenticated request
{
  "id": 101,
  "firstName": "Alan",
  "lastName": "Turing",
  "email": "alan@email.com",
  "roles": ["admin"]
}

// user.decorator.ts
import { createParamDecorator, ExecutionContext } from '@nestjs/common';

export const User = createParamDecorator(
  (data: string, ctx: ExecutionContext) => {
    const request = ctx.switchToHttp().getRequest();
    const user = request.user;

    return data ? user?.[data] : user;
  },
);
// then
@Get()
async findOne(@User('firstName') firstName: string) {
  console.log(`Hello ${firstName}``);
}
```

## Working with pipes

Nest treats custom param decorators in the same fashion as the built-in ones (`@Body()`, `@Param()` and `@Query()`). 

This means that pipes are executed for the custom annotated parameters as well (in our examples, the `user` argument). Moreover, you can apply the pipe directly to the custom decorator:

```typescript
@Get()
async findOne(
  @User(new ValidationPipe({ validateCustomDecorators: true })) // validateCustomDecorators option must be set to true. ValidationPipe does not validate arguments annotated with the custom decorators by default.
  user: UserEntity,
) {
  console.log(user);
}
```

## Decorator composition

```typescript
// auth.decorator.ts
import { applyDecorators } from '@nestjs/common';
// The @ApiHideProperty() decorator from the @nestjs/swagger package is not composable and won't work properly with the applyDecorators function.
export function Auth(...roles: Role[]) {
  return applyDecorators( // This has the effect of applying all four decorators with a single declaration.
    SetMetadata('roles', roles),
    UseGuards(AuthGuard, RolesGuard),
    ApiBearerAuth(),
    ApiUnauthorizedResponse({ description: 'Unauthorized' }),
  );
}

// then
@Get('users')
@Auth('admin')
findAllUsers() {}
```