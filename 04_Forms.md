## Forms

### Basics

- **Template-driven Forms**  
  Template-driven forms rely on Angular directives and are defined in the HTML template. They are simple to use and suitable for basic use cases. These forms use `ngModel` for two-way data binding and are tightly coupled with the template.

  Example:

  ```html
  <form>
    <input [(ngModel)]="name" name="name" required />
  </form>
  ```

  **Key Features:**
  - Easy to set up and use for simple forms.
  - Automatically tracks form state and validity.
  - Best suited for smaller applications or forms with less complex logic.

- **Reactive Forms**  
  Reactive forms provide a more programmatic approach to form creation and management. They use `FormGroup` and `FormControl` to define the structure and behavior of the form in the component class.

  Example:

  **Component Class:**
  ```typescript
  import { Component } from '@angular/core';
  import { FormGroup, FormControl, Validators } from '@angular/forms';

  @Component({
    selector: 'app-reactive-form',
    templateUrl: './reactive-form.component.html',
  })
  export class ReactiveFormComponent {
    form: FormGroup;

    constructor() {
      this.form = new FormGroup({
        name: new FormControl('', [Validators.required, Validators.minLength(3)]),
        email: new FormControl('', [Validators.required, Validators.email]),
        password: new FormControl('', [Validators.required, Validators.minLength(6)]),
      });
    }

    onSubmit() {
      if (this.form.valid) {
        console.log('Form Submitted', this.form.value);
      } else {
        console.log('Form is invalid');
      }
    }
  }
  ```

  **Template:**
  ```html
  <form [formGroup]="form" (ngSubmit)="onSubmit()">
    <label for="name">Name:</label>
    <input id="name" formControlName="name" />
    <div *ngIf="form.get('name')?.invalid && form.get('name')?.touched">
      Name is required and must be at least 3 characters long.
    </div>

    <label for="email">Email:</label>
    <input id="email" formControlName="email" />
    <div *ngIf="form.get('email')?.invalid && form.get('email')?.touched">
      Enter a valid email address.
    </div>

    <label for="password">Password:</label>
    <input id="password" type="password" formControlName="password" />
    <div *ngIf="form.get('password')?.invalid && form.get('password')?.touched">
      Password is required and must be at least 6 characters long.
    </div>

    <button type="submit" [disabled]="form.invalid">Submit</button>
  </form>
  ```

  **Key Features:**
  - Greater control over form behavior and validation.
  - Supports dynamic form creation and complex validation logic.
  - Ideal for large-scale applications or forms with advanced requirements.

### Validation

- **Basic Form Validation**  
  Angular provides built-in validators like `required`, `minLength`, and `maxLength` to handle common validation scenarios. These validators can be applied to both template-driven and reactive forms.

  Example:

  ```html
  <input [formControl]="name" required minlength="3" />
  ```

  **Key Features:**
  - Simplifies validation for standard use cases.
  - Automatically updates the form's validity state.

- **Custom Validators**  
  For more specific validation logic, you can create custom validators. These are functions that return an error object if validation fails or `null` if it passes.

  Example:

  **Component Class:**
  ```typescript
  import { Component } from '@angular/core';
  import { FormGroup, FormControl, Validators, ValidationErrors } from '@angular/forms';

  @Component({
    selector: 'app-custom-validator',
    templateUrl: './custom-validator.component.html',
  })
  export class CustomValidatorComponent {
    form: FormGroup;

    constructor() {
      this.form = new FormGroup({
        username: new FormControl('', [Validators.required, this.forbiddenNameValidator]),
      });
    }

    forbiddenNameValidator(control: FormControl): ValidationErrors | null {
      const forbidden = control.value === 'admin';
      return forbidden ? { forbiddenName: { value: control.value } } : null;
    }

    onSubmit() {
      if (this.form.valid) {
        console.log('Form Submitted', this.form.value);
      } else {
        console.log('Form is invalid');
      }
    }
  }
  ```

  **Template:**
  ```html
  <form [formGroup]="form" (ngSubmit)="onSubmit()">
    <label for="username">Username:</label>
    <input id="username" formControlName="username" />
    <div *ngIf="form.get('username')?.hasError('forbiddenName') && form.get('username')?.touched">
      The username "admin" is not allowed.
    </div>

    <button type="submit" [disabled]="form.invalid">Submit</button>
  </form>
  ```

  **Key Features:**
  - Enables implementation of custom business rules.
  - Can be reused across multiple forms.

### Advanced Topics

- **Dynamic Forms**  
  Dynamic forms allow you to create form controls and groups at runtime based on user input or external data. This is particularly useful for scenarios like survey forms or forms with variable fields.

  Example:

  **Component Class:**
  ```typescript
  import { Component } from '@angular/core';
  import { FormGroup, FormControl, FormBuilder } from '@angular/forms';

  @Component({
    selector: 'app-dynamic-form',
    templateUrl: './dynamic-form.component.html',
  })
  export class DynamicFormComponent {
    form: FormGroup;
    fields = [
      { name: 'firstName', label: 'First Name', type: 'text' },
      { name: 'lastName', label: 'Last Name', type: 'text' },
      { name: 'email', label: 'Email', type: 'email' },
    ];

    constructor(private fb: FormBuilder) {
      this.form = this.fb.group({});
      this.fields.forEach(field => {
        this.form.addControl(field.name, new FormControl(''));
      });
    }

    onSubmit() {
      console.log('Form Submitted', this.form.value);
    }
  }
  ```

  **Template:**
  ```html
  <form [formGroup]="form" (ngSubmit)="onSubmit()">
    <div *ngFor="let field of fields">
      <label [for]="field.name">{{ field.label }}:</label>
      <input [id]="field.name" [type]="field.type" [formControlName]="field.name" />
    </div>
    <button type="submit">Submit</button>
  </form>
  ```

- **Form Arrays**  
  Form arrays are useful for managing a dynamic list of form controls, such as adding or removing items in a list.

  Example:

  **Component Class:**
  ```typescript
  import { Component } from '@angular/core';
  import { FormGroup, FormArray, FormControl } from '@angular/forms';

  @Component({
    selector: 'app-form-array',
    templateUrl: './form-array.component.html',
  })
  export class FormArrayComponent {
    form: FormGroup;

    constructor() {
      this.form = new FormGroup({
        items: new FormArray([]),
      });
    }

    get items() {
      return this.form.get('items') as FormArray;
    }

    addItem() {
      this.items.push(new FormControl(''));
    }

    removeItem(index: number) {
      this.items.removeAt(index);
    }

    onSubmit() {
      console.log('Form Submitted', this.form.value);
    }
  }
  ```

  **Template:**
  ```html
  <form [formGroup]="form" (ngSubmit)="onSubmit()">
    <div formArrayName="items">
      <div *ngFor="let item of items.controls; let i = index">
        <input [formControlName]="i" />
        <button type="button" (click)="removeItem(i)">Remove</button>
      </div>
    </div>
    <button type="button" (click)="addItem()">Add Item</button>
    <button type="submit">Submit</button>
  </form>
  ```

  **Key Features:**
  - Simplifies handling of dynamic lists of controls.
  - Provides full control over adding, removing, or updating items in the array.
