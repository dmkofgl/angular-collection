
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
