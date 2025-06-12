# Angular Core Concepts Overview

## Core Concepts
### Components
- **What are Components?**  
  Components are the building blocks of Angular applications. They control a portion of the UI and define the logic and behavior for that part.  
  Example:  
  ```typescript
  @Component({
    selector: 'app-hello',
    template: `<h1>Hello, {{name}}!</h1>`,
    styles: [`h1 { color: blue; }`]
  })
  export class HelloComponent {
    name = 'Angular';
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
    selector: 'app-example',
    templateUrl: './example.component.html',
    styleUrls: ['./example.component.css']
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
    bootstrap: [AppComponent]
  })
  export class AppModule {}
  ```

- **NgModule Basics**  
  The `@NgModule` decorator is used to define a module. It includes declarations, imports, providers, and bootstrap properties.  
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
  Example:  
  ```typescript
  @NgModule({
    declarations: [UserComponent],
    imports: [RouterModule.forChild(routes)],
    exports: [UserComponent]
  })
  export class UserModule {}
  ```

### Services
- **What is a Service?**  
  Services are reusable classes that encapsulate business logic, data access, or shared functionality across components.  
  Example:  
  ```typescript
  @Injectable({
    providedIn: 'root'
  })
  export class DataService {
    getData() {
      return ['Item1', 'Item2', 'Item3'];
    }
  }
  ```

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
    providers: [MyService]
  })
  export class AppModule {}
  ```

- **Using DI in Angular**  
  Angular injects dependencies into constructors, enabling components and services to use shared functionality.  
  Example:  
  ```typescript
  constructor(private service: MyService) {
    this.data = service.getData();
  }
  ```

## Component Details
### Lifecycle
- **Component Lifecycle Hooks**  
  Lifecycle hooks like `ngOnInit`, `ngOnChanges`, and `ngOnDestroy` allow developers to tap into specific moments in a component's lifecycle.  
  Example:  
  ```typescript
  ngOnInit() {
    console.log('Component initialized');
  }
  ```

- **Common Lifecycle Hook Use Cases**  
  Examples include initializing data in `ngOnInit`, cleaning up resources in `ngOnDestroy`, and responding to input changes in `ngOnChanges`.  
  Example:  
  ```typescript
  ngOnDestroy() {
    console.log('Component destroyed');
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
- **Template Syntax**  
  Angular templates use interpolation, property binding, and structural directives.  
  Example:  
  ```html
  <h1>{{ title }}</h1>
  ```

- **Using Interpolation and Bindings**  
  Interpolation binds data from the component to the template.  
  Example:  
  ```html
  <input [value]="name" />
  ```

### Interaction
- **Parent to Child Communication**  
  Use `@Input` to pass data from a parent to a child component.  
  Example:  
  ```typescript
  @Input() name: string;
  ```

- **Child to Parent Communication**  
  Use `@Output` to emit events from a child to a parent component.  
  Example:  
  ```typescript
  @Output() notify = new EventEmitter<string>();
  ```

- **Using `@Input` and `@Output`**  
  Combine `@Input` and `@Output` for two-way communication.  
  Example:  
  ```typescript
  @Input() value: string;
  @Output() valueChange = new EventEmitter<string>();
  ```

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
  View encapsulation determines how styles are applied to components.  
  Example:  
  ```typescript
  encapsulation: ViewEncapsulation.None
  ```

- **Encapsulation Modes**  
  Modes include `Emulated`, `None`, and `ShadowDom`.

### Built-in Directives
- **Overview of Structural Directives**  
  Structural directives like `*ngIf` and `*ngFor` modify the DOM structure.  
  Example:  
  ```html
  <div *ngIf="isVisible">Visible</div>
  ```

- **Overview of Attribute Directives**  
  Attribute directives like `ngClass` and `ngStyle` modify the appearance or behavior of elements.  
  Example:  
  ```html
  <div [ngClass]="{'active': isActive}"></div>
  ```

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
    name: new FormControl('')
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
    return control.value === 'admin' ? { forbiddenName: true } : null;
  }
  ```

## HttpClient
- **What is HttpClient?**  
  HttpClient is a service for making HTTP requests and handling responses.  
  Example:  
  ```typescript
  this.http.get('/api/data').subscribe(data => console.log(data));
  ```

- **Making HTTP Requests**  
  Use methods like `get`, `post`, `put`, and `delete` for CRUD operations.  
  Example:  
  ```typescript
  this.http.post('/api/data', { name: 'Angular' }).subscribe();
  ```

- **Handling Responses**  
  Use RxJS operators like `map` and `catchError` to process responses.  
  Example:  
  ```typescript
  this.http.get('/api/data').pipe(map(response => response.data));
  ```

## Routing
### Basics
- **Router Module Imports**  
  Import `RouterModule` and configure routes in the module.  
  Example:  
  ```typescript
  RouterModule.forRoot([{ path: 'home', component: HomeComponent }])
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
  this.route.snapshot.params['id'];
  ```

- **Activated Route**  
  Use `ActivatedRoute` to access route parameters and data.  
  Example:  
  ```typescript
  this.route.params.subscribe(params => console.log(params));
  ```

- **Router Events**  
  Listen to router events like `NavigationStart` and `NavigationEnd`.  
  Example:  
  ```typescript
  this.router.events.subscribe(event => console.log(event));
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
  this.router.navigate(['../child'], { relativeTo: this.route });
  ```

## RxJS
- **What is RxJS?**  
  RxJS is a library for reactive programming using observables.  
  Example:  
  ```typescript
  of(1, 2, 3).subscribe(value => console.log(value));
  ```

- **Mapping Observables to Promises**  
  Use `toPromise` to convert observables to promises.  
  Example:  
  ```typescript
  this.http.get('/api/data').toPromise().then(data => console.log(data));
  ```

- **Common RxJS Operators**  
  Examples include `map`, `filter`, `mergeMap`, and `switchMap`.  
  Example:  
  ```typescript
  this.http.get('/api/data').pipe(map(data => data.items));
  ```