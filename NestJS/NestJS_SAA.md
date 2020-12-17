

```typescript
// To create a Nest standalone application, use the following construction:
async function bootstrap() {
  const app = await NestFactory.createApplicationContext(ApplicationModule);
  // application logic...
}
bootstrap();

// The standalone application object allows you to obtain a reference to any instance registered within the Nest application. Let's imagine that we have a TasksService in the TasksModule. This class provides a set of methods that we want to call from within a CRON job.

const app = await NestFactory.create(ApplicationModule);
const tasksService = app.get(TasksService);
```

To access the `TasksService` instance we use the `get()` method. The `get()` method acts like a **query** that searches for an instance in each registered module. Alternatively, for strict context checking, pass an options object with the `strict: true` property. With this option in effect, you have to navigate through specific modules to obtain a particular instance from the selected context.

```typescript
const app = await NestFactory.create(AppModule);
const tasksService = app.select(TasksModule).get(TasksService, { strict: true });
```

|            |                                                              |
| ---------- | ------------------------------------------------------------ |
| `get()`    | Retrieves an instance of a controller or provider (including guards, filters, and so on) available in the application context. |
| `select()` | Navigates through the modules graph to pull out a specific instance from the selected module (used together with strict mode as described above). |

In non-strict mode, the root module is selected by default. To select any other module, you need to navigate the modules graph manually, step by step.

If you want the node application to close after the script finishes (e.g., for a script running CRON jobs), add `await app.close()` to the end of your `bootstrap` function:

```typescript
async function bootstrap() {
  const app = await NestFactory.createApplicationContext(ApplicationModule);
  // application logic...
  await app.close();
}
bootstrap();
```