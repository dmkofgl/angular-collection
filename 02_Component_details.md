
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