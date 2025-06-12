
## Core Concepts

### Components

- **What are Components?**  
  Components are the building blocks of Angular applications. They control a portion of the UI and define the logic and behavior for that part.  
  Example:

  ```typescript
  @Component({
    selector: "app-hello",
    template: `<h1>Hello, {{ name }}!</h1>`,
    styles: [
      `
        h1 {
          color: blue;
        }
      `,
    ],
  })
  export class HelloComponent {
    name = "Angular";
  }
  ```

- **Creating a Component**  
  Components are created using the `@Component` decorator and consist of a TypeScript class, an HTML template, and optional CSS styles.  
  Example:

  ```typescript
  ng generate component my-component
  ```

- **Component Metadata**  
  Metadata is defined in the `@Component` decorator and includes properties like `selector`, `templateUrl`, and `styleUrls`.  
  Example:
  ```typescript
  @Component({
    selector: "app-example",
    templateUrl: "./example.component.html",
    styleUrls: ["./example.component.css"],
  })
  export class ExampleComponent {}
  ```

### Modules

- **What are Modules?**  
  Modules are containers for organizing an application into cohesive blocks of functionality. Every Angular app has at least one module, the root module.  
  Example:

  ```typescript
  @NgModule({
    declarations: [AppComponent],
    imports: [BrowserModule],
    bootstrap: [AppComponent],
  })
  export class AppModule {}
  ```

- **NgModule Basics**  
  The `@NgModule` decorator is used to define a module. It includes declarations, imports, providers, and bootstrap properties.

  - **Declarations**: Specifies the components, directives, and pipes that belong to this module.
  - **Imports**: Lists other modules whose exported classes are needed by components in this module.
  - **Providers**: Defines the services that should be available to the injector for this module.
  - **Bootstrap**: Identifies the root component that Angular should bootstrap when the application starts.

  Example:

  ```typescript
  @NgModule({
    declarations: [MyComponent],
    imports: [CommonModule],
    providers: [MyService],
    bootstrap: [MyComponent]
  })
  export MyModule {}
  ```

- **Feature Modules**  
  Feature modules encapsulate specific functionality, such as user management or routing, to keep the application modular and maintainable.

  - **Declarations**: Lists the components, directives, and pipes that are part of this feature module.
  - **Imports**: Specifies other modules that this feature module depends on, such as `CommonModule` or `RouterModule`.
  - **Exports**: Makes components, directives, or pipes from this module available to other modules that import this feature module.

  Example:

  ```typescript
  @NgModule({
    declarations: [UserComponent],
    imports: [RouterModule.forChild(routes)],
    exports: [UserComponent],
  })
  export class UserModule {}
  ```

### Services

- **What is a Service?**  
  Services are reusable classes that encapsulate business logic, data access, or shared functionality across components.  
  Example:

  ```typescript
  @Injectable({
    providedIn: "root",
  })
  export class DataService {
    getData() {
      return ["Item1", "Item2", "Item3"];
    }
  }
  ```

- **`@Injectable` Decorator**: Marks a class as available for dependency injection. It allows Angular to inject this service wherever it is needed.
- **`providedIn` Property**: Specifies the scope of the service. Setting it to `'root'` makes the service available application-wide and ensures a single instance is created (singleton).

  Other options for `providedIn` include:

  - **`providedIn: 'platform'`**: The service is shared across all Angular applications running on the same platform (e.g., a browser). This ensures that a single instance of the service is created and shared across multiple Angular applications within the same platform context.
  - **`providedIn: 'any'`**: A new instance of the service is created for each lazy-loaded module or component that injects it.
  - **Omitting `providedIn`**: The service must be explicitly added to the `providers` array of a module.

- **Creating and Using Services**  
  Services are created using the `@Injectable` decorator and are typically provided in modules or components for dependency injection.  
  Example:
  ```typescript
  constructor(private dataService: DataService) {
    this.items = dataService.getData();
  }
  ```

### Dependency Injection (DI)

- **What is Dependency Injection?**  
  DI is a design pattern used in Angular to provide dependencies to components, directives, and services automatically.  
  Example:

  ```typescript
  constructor(private myService: MyService) {}
  ```

- **Basic Configuration**  
  Dependencies are configured using the `providers` array in modules or components.  
  Example:

  ```typescript
  @NgModule({
    providers: [MyService],
  })
  export class AppModule {}
  ```

- **Using DI in Angular**  
   Angular injects dependencies into constructors, enabling components and services to use shared functionality.  
   Example:

  ```typescript
  @Component({...})
  export class MyComponent {
  @Inject(MyService) myService!: MyService;

  ngOnInit() {
    this.myService.doSomething();
  }
  }

  ```

  ```typescript
  import { Component, inject } from "@angular/core";
  @Component({
  ...
  })
  export class AddNewItem {
  fb = inject(FormBuilder);
  ```
