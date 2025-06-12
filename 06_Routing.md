
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