# Angular Core Concepts Overview

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

## Component Details

### Lifecycle

- **What is Component Lifecycle?**  
  The component lifecycle in Angular refers to the sequence of events that occur from the creation of a component to its destruction. Angular provides lifecycle hooks that allow developers to execute custom logic at specific stages of this lifecycle.

- **Lifecycle Stages**

  1. **Creation**: The component is instantiated, and its dependencies are injected.
  2. **Change Detection**: Angular checks for changes in the component's data-bound properties.
  3. **Rendering**: The component's template is rendered or updated in the DOM.
  4. **Destruction**: The component is removed from the DOM, and resources are cleaned up.

- **All Lifecycle Hooks**  
  Angular provides lifecycle hooks that allow developers to execute custom logic at specific stages of a component's lifecycle. These hooks are methods that Angular calls at specific points in the lifecycle of a component or directive.

  - **`ngOnChanges`**: Called when an input property value changes. Receives a `SimpleChanges` object containing the current and previous values.
  - **`ngOnInit`**: Called once after the component's input properties are initialized. Ideal for initialization logic.
  - **`ngDoCheck`**: Called during every change detection cycle. Use this for custom change detection logic.
  - **`ngAfterContentInit`**: Called once after Angular projects external content into the component's view.
  - **`ngAfterContentChecked`**: Called after every check of the projected content.
  - **`ngAfterViewInit`**: Called once after the component's view and child views are initialized.
  - **`ngAfterViewChecked`**: Called after every check of the component's view and child views.
  - **`ngOnDestroy`**: Called just before the component is destroyed. Use this for cleanup logic, such as unsubscribing from observables or detaching event handlers.

  **Lifecycle Flow**:

  1. `ngOnChanges` (if applicable)
  2. `ngOnInit`
  3. `ngDoCheck`
  4. `ngAfterContentInit`
  5. `ngAfterContentChecked`
  6. `ngAfterViewInit`
  7. `ngAfterViewChecked`
  8. `ngOnDestroy`

  Example:

  ```typescript
  @Component({...})
  export class MyComponent implements OnInit, OnDestroy {
    @Input() data: string;
    name = input('');

    ngOnInit() {
      console.log('Component initialized');
    }

    ngOnDestroy() {
      console.log('Component destroyed');
    }

    ngOnChanges(changes: SimpleChanges) {
    for (const inputName in changes) {
      const inputValues = changes[inputName];
      console.log(`Previous ${inputName} == ${inputValues.previousValue}`);
      console.log(`Current ${inputName} == ${inputValues.currentValue}`);
      console.log(`Is first ${inputName} change == ${inputValues.firstChange}`);
    }
  }
  }
  ```

### Events

- **Event Binding**  
  Event binding allows you to listen to user actions like clicks and keypresses.  
  Example:

  ```html
  <button (click)="onClick()">Click Me</button>
  ```

- **Custom Events**  
  Custom events are created using `@Output` and `EventEmitter`.  
  Example:
  ```typescript
  @Output() clicked = new EventEmitter<string>();
  onClick() {
    this.clicked.emit('Button clicked!');
  }
  ```

### Templates

- **What are Templates?**  
  Templates in Angular define the structure and layout of a component's view. They use HTML along with Angular's template syntax to bind data, handle events, and control the DOM structure dynamically.

- **Template Syntax Features**  
  Angular templates provide powerful features for building dynamic and interactive views:
  - **Interpolation**: Bind component properties to the view using `{{ }}` syntax.
    Example:
    ```html
    <h1>{{ title }}</h1>
    ```
  - **Property Binding**: Bind DOM element properties to component properties using `[ ]`.
    Example:
    ```html
    <input [value]="name" />
    ```
  - **Event Binding**: Bind DOM events to component methods using `( )`.
    Example:
    ```html
    <button (click)="onClick()">Click Me</button>
    ```
  - **Two-Way Binding**: Combine property and event binding using `[( )]` for seamless data synchronization.  
    Two-way binding allows data to flow in both directions between the component and the template. It ensures that changes in the component's property are reflected in the template, and user input in the template updates the component's property automatically. This is achieved using Angular's `[(ngModel)]` directive.

    **How it Works**:  
    - The `[( )]` syntax is a shorthand for combining property binding `[ ]` and event binding `( )`.
    - `[ ]` binds the property value from the component to the template.
    - `( )` listens for changes in the template and updates the component property.

    **Example**:
    ```html
    <input [(ngModel)]="name" />
    <p>Hello, {{ name }}!</p>
    ```
    In this example:
    - The `name` property in the component is bound to the input field.
    - Any changes made to the input field are reflected in the `name` property.
    - The updated `name` property is displayed in the paragraph.

    **Requirements**:
    - To use `[(ngModel)]`, you must import the `FormsModule` in your Angular module.
      ```typescript
      import { FormsModule } from '@angular/forms';

      @NgModule({
        imports: [FormsModule],
        ...
      })
      export class AppModule {}
      ```

    **Best Practices**:
    - Use two-way binding only when necessary, as it can make debugging more complex.
    - For better control, consider using separate property and event bindings instead of `[(ngModel)]` when appropriate.
    
  - **Directives**: Use structural (`*ngIf`, `*ngFor`) and attribute (`[ngClass]`, `[ngStyle]`) directives to manipulate the DOM.
    Example:
    ```html
    <div *ngIf="isVisible">Visible</div>
    <div [ngClass]="{'active': isActive}"></div>
    ```

- **Template Expressions**  
  Template expressions are used within bindings to perform calculations or access properties. They should be simple and free of side effects.
  Example:
  ```html
  <p>{{ items.length > 0 ? 'Items available' : 'No items' }}</p>
  ```

- **Template Statements**  
  Template statements respond to user actions and are used in event bindings. They can call component methods or assign values.
  Example:
  ```html
  <button (click)="addItem()">Add Item</button>
  ```

- **Using Structural Directives**  
  Structural directives like `*ngIf` and `*ngFor` dynamically modify the DOM structure.
  Example:
  ```html
  <ul>
    <li *ngFor="let item of items">{{ item }}</li>
  </ul>
  ```

- **Using Attribute Directives**  
  Attribute directives like `[ngClass]` and `[ngStyle]` modify the appearance or behavior of elements.
  Example:
  ```html
  <div [ngStyle]="{'color': isActive ? 'green' : 'red'}">Status</div>
  ```

- **Template Reference Variables**  
  Use `#` to declare variables in templates and access DOM elements or component instances.
  Example:
  ```html
  <input #inputRef />
  <button (click)="logValue(inputRef.value)">Log Value</button>
  ```

- **Dynamic Components**  
  Templates can include dynamic components using Angular's `ng-container` or `ng-template`.
  Example:
  ```html
  <ng-container *ngIf="isLoaded">
    <app-child></app-child>
  </ng-container>
  ```

- **Best Practices**  
  - Keep templates simple and focused on presentation.
  - Avoid complex logic in template expressions; move it to the component.
  - Use Angular's built-in directives and pipes to simplify template code.

### Interaction

- **Parent to Child Communication**  
  Use `@Input` to pass data from a parent to a child component.  
  Example:

  **Parent Component**:
  ```typescript
  @Component({
    selector: 'app-parent',
    template: `<app-child [name]="parentName"></app-child>`,
  })
  export class ParentComponent {
    parentName = 'Angular';
  }
  ```

  **Child Component**:
  ```typescript
  @Component({
    selector: 'app-child',
    template: `<p>Hello, {{ name }}!</p>`,
  })
  export class ChildComponent {
    @Input() name!: string;
  }
  ```

- **Child to Parent Communication**  
  Use `@Output` to emit events from a child to a parent component.  
  Example:

  **Child Component**:
  ```typescript
  @Component({
    selector: 'app-child',
    template: `<button (click)="notifyParent()">Notify Parent</button>`,
  })
  export class ChildComponent {
    @Output() notify = new EventEmitter<string>();

    notifyParent() {
      this.notify.emit('Child says hello!');
    }
  }
  ```

  **Parent Component**:
  ```typescript
  @Component({
    selector: 'app-parent',
    template: `<app-child (notify)="onNotify($event)"></app-child>`,
  })
  export class ParentComponent {
    onNotify(message: string) {
      console.log(message);
    }
  }
  ```

- **Using `@Input` and `@Output`**  
  Combine `@Input` and `@Output` for two-way communication between parent and child components.  

  **How it Works**:  
  - The `@Input` decorator allows the parent component to pass data to the child component. The child component receives this data as an input property.
  - The `@Output` decorator, combined with `EventEmitter`, allows the child component to emit events to the parent component. The parent listens to these events and updates its state accordingly.
  - The `[( )]` syntax in the parent component is a shorthand for binding the input property (`[ ]`) and listening to the output event (`( )`) simultaneously.

  **Example**:

  **Child Component**:
  ```typescript
  @Component({
    selector: 'app-child',
    template: `
      <input [value]="value" (input)="onValueChange($event.target.value)" />
    `,
  })
  export class ChildComponent {
    @Input() value!: string;
    @Output() valueChange = new EventEmitter<string>();

    onValueChange(newValue: string) {
      this.valueChange.emit(newValue);
    }
  }
  ```

  **Parent Component**:
  ```typescript
  @Component({
    selector: 'app-parent',
    template: `
      <app-child [(value)]="parentValue"></app-child>
      <p>Parent Value: {{ parentValue }}</p>
    `,
  })
  export class ParentComponent {
    parentValue = 'Initial Value';
  }
  ```

  **Explanation**:
  - The `value` property in the parent component is bound to the `@Input` property in the child component.
  - When the user types in the input field in the child component, the `onValueChange` method emits the new value using the `valueChange` event.
  - The parent component listens to the `valueChange` event and updates its `parentValue` property, which is then passed back to the child component via the `@Input` binding.

  This creates a seamless two-way communication between the parent and child components.

### Styles

- **Adding Styles to Components**  
  Styles can be added inline, in external files, or using Angular's `styleUrls`.  
  Example:

  ```typescript
  @Component({
    styles: [`h1 { color: red; }`]
  })
  ```

- **Global vs Component Styles**  
  Global styles apply to the entire app, while component styles are scoped to the component.

### View Encapsulation

- **What is View Encapsulation?**  
  View Encapsulation in Angular determines how styles defined in a component are applied to its template and whether they affect other parts of the application. It ensures that styles are scoped to specific components, preventing unintended side effects on other components.

- **Encapsulation Modes**  
  Angular provides three modes of view encapsulation:

  1. **Emulated (Default)**:  
     - Styles are scoped to the component using a mechanism that emulates Shadow DOM behavior.  
     - Angular rewrites CSS selectors to include unique attributes, ensuring styles are applied only to the component.  
     - Example:  
       ```typescript
       @Component({
         selector: 'app-example',
         template: `<h1>Hello</h1>`,
         styles: [`h1 { color: red; }`],
         encapsulation: ViewEncapsulation.Emulated, // Default
       })
       export class ExampleComponent {}
       ```

  2. **None**:  
     - Styles are applied globally to the entire application.  
     - There is no encapsulation, and styles defined in the component can affect other components.  
     - Example:  
       ```typescript
       @Component({
         selector: 'app-example',
         template: `<h1>Hello</h1>`,
         styles: [`h1 { color: red; }`],
         encapsulation: ViewEncapsulation.None,
       })
       export class ExampleComponent {}
       ```

  3. **ShadowDom**:  
     - Uses the browser's native Shadow DOM to scope styles to the component.  
     - Styles are encapsulated within the Shadow DOM, ensuring they do not leak out or affect other components.  
     - Example:  
       ```typescript
       @Component({
         selector: 'app-example',
         template: `<h1>Hello</h1>`,
         styles: [`h1 { color: red; }`],
         encapsulation: ViewEncapsulation.ShadowDom,
       })
       export class ExampleComponent {}
       ```

- **Comparison of Modes**  
  | Mode         | Style Scope       | Affects Other Components | Browser Support |
  |--------------|-------------------|--------------------------|-----------------|
  | **Emulated** | Component-specific | No                       | All browsers    |
  | **None**     | Global             | Yes                      | All browsers    |
  | **ShadowDom**| Component-specific | No                       | Modern browsers |

- **Best Practices**  
  - Use **Emulated** (default) for most cases to ensure styles are scoped to components without affecting others.  
  - Use **None** sparingly, only when global styles are required.  
  - Use **ShadowDom** for strict encapsulation when working with modern browsers and when leveraging native Shadow DOM features.

### Built-in Directives

- **Overview of Structural Directives**  
  Structural directives in Angular are used to modify the structure of the DOM by adding or removing elements. Here is a list of commonly used structural directives:

  1. **`*ngIf`**: Conditionally includes or excludes an element in the DOM based on a boolean expression.  
     Example:  
     ```html
     <div *ngIf="isVisible">Visible</div>
     ```

  2. **`*ngFor`**: Iterates over a collection and renders an element for each item.  
     Example:  
     ```html
     <ul>
       <li *ngFor="let item of items">{{ item }}</li>
     </ul>
     ```

  3. **`*ngSwitch`**: A set of directives (`*ngSwitch`, `*ngSwitchCase`, `*ngSwitchDefault`) used to conditionally display elements based on a matching expression.  
     Example:  
     ```html
     <div [ngSwitch]="value">
       <p *ngSwitchCase="'A'">Case A</p>
       <p *ngSwitchCase="'B'">Case B</p>
       <p *ngSwitchDefault>Default Case</p>
     </div>
     ```

  4. **`*ngTemplateOutlet`**: Dynamically includes an Angular template in the DOM.  
     Example:  
     ```html
     <ng-container *ngTemplateOutlet="templateRef"></ng-container>
     ```

  - **Custom Structural Directives**: Developers can also create custom structural directives to implement specific DOM manipulation logic.

### Built-in Pipes

- **What are Pipes?**  
  Pipes transform data in templates.  
  Example:

  ```html
  <p>{{ date | date:'short' }}</p>
  ```

- **Commonly Used Pipes**  
  Examples include `date`, `uppercase`, `lowercase`, and `currency`.  
  Example:
  ```html
  <p>{{ price | currency:'USD' }}</p>
  ```

- **Built-in Pipes**  
  Angular provides several built-in pipes to transform data in templates. Here is a list of commonly used pipes:

  1. **`DatePipe`**: Formats a date value according to locale rules.  
     Example:  
     ```html
     <p>{{ today | date:'short' }}</p>
     ```

  2. **`UpperCasePipe`**: Transforms text to uppercase.  
     Example:  
     ```html
     <p>{{ 'hello' | uppercase }}</p>
     ```

  3. **`LowerCasePipe`**: Transforms text to lowercase.  
     Example:  
     ```html
     <p>{{ 'HELLO' | lowercase }}</p>
     ```

  4. **`CurrencyPipe`**: Formats a number as currency.  
     Example:  
     ```html
     <p>{{ price | currency:'USD' }}</p>
     ```

  5. **`DecimalPipe`**: Formats a number with decimal points.  
     Example:  
     ```html
     <p>{{ 1234.5678 | number:'1.2-2' }}</p>
     ```

  6. **`PercentPipe`**: Formats a number as a percentage.  
     Example:  
     ```html
     <p>{{ 0.25 | percent }}</p>
     ```

  7. **`JsonPipe`**: Converts a value into a JSON string.  
     Example:  
     ```html
     <pre>{{ object | json }}</pre>
     ```

  8. **`SlicePipe`**: Extracts a portion of a string or array.  
     Example:  
     ```html
     <p>{{ 'Angular' | slice:0:3 }}</p>
     ```

  9. **`AsyncPipe`**: Unwraps values from Promises or Observables.  
     Example:  
     ```html
     <p>{{ observableValue | async }}</p>
     ```

  - **Custom Pipes**: Developers can also create custom pipes to implement specific data transformation logic.

## Angular CLI

- **What is Angular CLI?**  
  Angular CLI is a command-line tool for creating, building, and maintaining Angular applications.  
  Example:

  ```bash
  ng new my-app
  ```

- **Common CLI Commands**  
  Examples include `ng serve`, `ng build`, and `ng generate`.  
  Example:
  ```bash
  ng generate component my-component
  ```

## Forms

### Basics

- **Template-driven Forms**  
  Template-driven forms use directives like `ngModel` for two-way data binding.  
  Example:

  ```html
  <form>
    <input [(ngModel)]="name" />
  </form>
  ```

- **Reactive Forms**  
  Reactive forms use `FormGroup` and `FormControl` for programmatic control.  
  Example:
  ```typescript
  this.form = new FormGroup({
    name: new FormControl(""),
  });
  ```

### Validation

- **Basic Form Validation**  
  Use Angular's built-in validators like `required` and `minLength`.  
  Example:

  ```html
  <input [formControl]="name" required />
  ```

- **Custom Validators**  
  Create custom validators for specific validation logic.  
  Example:
  ```typescript
  function forbiddenNameValidator(control: FormControl) {
    return control.value === "admin" ? { forbiddenName: true } : null;
  }
  ```

## HttpClient

- **What is HttpClient?**  
  HttpClient is a service for making HTTP requests and handling responses.  
  Example:

  ```typescript
  this.http.get("/api/data").subscribe((data) => console.log(data));
  ```

- **Making HTTP Requests**  
  Use methods like `get`, `post`, `put`, and `delete` for CRUD operations.  
  Example:

  ```typescript
  this.http.post("/api/data", { name: "Angular" }).subscribe();
  ```

- **Handling Responses**  
  Use RxJS operators like `map` and `catchError` to process responses.  
  Example:
  ```typescript
  this.http.get("/api/data").pipe(map((response) => response.data));
  ```

## Routing

### Basics

- **Router Module Imports**  
  Import `RouterModule` and configure routes in the module.  
  Example:

  ```typescript
  RouterModule.forRoot([{ path: "home", component: HomeComponent }]);
  ```

- **Route Configuration**  
  Define routes with `path` and `component` properties.  
  Example:

  ```typescript
  { path: 'about', component: AboutComponent }
  ```

- **Router Outlet**  
  Use `<router-outlet>` to display routed components.  
  Example:

  ```html
  <router-outlet></router-outlet>
  ```

- **Router Links and Active Links**  
  Use `[routerLink]` for navigation and `routerLinkActive` for active link styling.  
  Example:

  ```html
  <a [routerLink]="['/home']" routerLinkActive="active">Home</a>
  ```

- **Router State**  
  Access router state using `ActivatedRoute`.  
  Example:

  ```typescript
  this.route.snapshot.params["id"];
  ```

  ```typescript
  import { Component, OnInit } from '@angular/core';
  import { ActivatedRoute } from '@angular/router';

  @Component({...})
  export class MyComponent implements OnInit {
    constructor(private route: ActivatedRoute) {}

    ngOnInit() {
      this.route.params.subscribe(params => {
        console.log(params['id']);
      });
    }
  }
  ```

- **Activated Route**  
  Use `ActivatedRoute` to access route parameters and data.  
  Example:

  ```typescript
  this.route.params.subscribe((params) => console.log(params));
  ```

- **Router Events**  
  Listen to router events like `NavigationStart` and `NavigationEnd`.  
  Example:
  ```typescript
  this.router.events.subscribe((event) => console.log(event));
  ```

### Child Routes

- **Defining Child Routes**  
  Use `children` property to define nested routes.  
  Example:

  ```typescript
  { path: 'parent', children: [{ path: 'child', component: ChildComponent }] }
  ```

- **Relative Navigation**  
  Navigate relative to the current route using `../` or `./`.  
  Example:
  ```typescript
  this.router.navigate(["../child"], { relativeTo: this.route });
  ```
