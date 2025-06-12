## Routing

### Basics

- **Router Module Imports**  
  Import `RouterModule` and configure routes in the module.  
  Example:

  ```typescript
  RouterModule.forRoot([{ path: "home", component: HomeComponent }]);
  ```

- **Standalone Component Routing**  
  Define routes directly in standalone components using the `imports` array.  
  Example:

  ```typescript
  import { Component } from '@angular/core';
  import { RouterModule } from '@angular/router';

  @Component({
    standalone: true,
    imports: [RouterModule],
    template: '<router-outlet></router-outlet>',
  })
  export class AppComponent {}
  ```

- **Route Configuration**  
  Define routes with `path` and `component` properties. Routes can be configured in two ways:  

  **Module-Based Approach:**  
  Example:

  ```typescript
  import { NgModule } from '@angular/core';
  import { RouterModule, Routes } from '@angular/router';
  import { AboutComponent } from './about.component';

  const routes: Routes = [
    { path: 'about', component: AboutComponent },
  ];

  @NgModule({
    imports: [RouterModule.forRoot(routes)],
    exports: [RouterModule],
  })
  export class AppRoutingModule {}
  ```

  **Standalone Component Approach:**  
  Example:

  ```typescript
  import { Component } from '@angular/core';
  import { provideRouter, RouterModule } from '@angular/router';
  import { AboutComponent } from './about.component';

  @Component({
    standalone: true,
    imports: [RouterModule],
    providers: [
      provideRouter([
        { path: 'about', component: AboutComponent },
      ]),
    ],
    template: '<router-outlet></router-outlet>',
  })
  export class AppComponent {}
  ```

  **Using a `routes.ts` File:**  
  Define routes in a separate `routes.ts` file to keep the routing logic modular.  
  Example:

  ```typescript
  // routes.ts
  import { Routes } from '@angular/router';
  import { AboutComponent } from './about.component';

  export const appRoutes: Routes = [
    { path: 'about', component: AboutComponent },
  ];
  ```

  **Module-Based Approach with `routes.ts`:**  
  Example:

  ```typescript
  import { NgModule } from '@angular/core';
  import { RouterModule } from '@angular/router';
  import { appRoutes } from './routes';

  @NgModule({
    imports: [RouterModule.forRoot(appRoutes)],
    exports: [RouterModule],
  })
  export class AppRoutingModule {}
  ```

  **Standalone Component Approach with `routes.ts`:**  
  Example:

  ```typescript
  import { Component } from '@angular/core';
  import { provideRouter, RouterModule } from '@angular/router';
  import { appRoutes } from './routes';

  @Component({
    standalone: true,
    imports: [RouterModule],
    providers: [provideRouter(appRoutes)],
    template: '<router-outlet></router-outlet>',
  })
  export class AppComponent {}
  ```

- **Router Outlet**  
  The `<router-outlet>` directive acts as a placeholder that Angular dynamically fills based on the current route. It is essential for rendering routed components in your application.  

  **Basic Usage:**  
  Place `<router-outlet>` in your main application template to display routed components.  
  Example:

  ```html
  <router-outlet></router-outlet>
  ```

  **Multiple Router Outlets:**  
  You can define multiple named outlets for advanced routing scenarios. Use the `outlet` property in route configuration to specify which outlet to target.  
  Example:

  ```html
  <router-outlet name="primary"></router-outlet>
  <router-outlet name="sidebar"></router-outlet>
  ```

  ```typescript
  const routes: Routes = [
    { path: 'main', component: MainComponent, outlet: 'primary' },
    { path: 'sidebar', component: SidebarComponent, outlet: 'sidebar' },
  ];
  ```

  **Conditional Rendering with Router Outlet:**  
  You can conditionally render `<router-outlet>` using structural directives like `*ngIf`.  
  Example:

  ```html
  <div *ngIf="isLoggedIn">
    <router-outlet></router-outlet>
  </div>
  ```

- **Router Links and Active Links**  
  Use `[routerLink]` for navigation and `routerLinkActive` for active link styling.  
  Example:

  ```html
  <a [routerLink]="['/home']" routerLinkActive="active">Home</a>
  ```

- **Router State**  
  The router state represents the current state of the router at any given time. It provides access to route parameters, query parameters, and static or dynamic data associated with the route. This is useful for building dynamic and data-driven applications.  

  **Accessing Route Parameters:**  
  Use `ActivatedRoute` to access route parameters.  
  Example:

  ```typescript
  import { Component, OnInit } from '@angular/core';
  import { ActivatedRoute } from '@angular/router';

  @Component({
    selector: 'app-detail',
    template: '<p>Detail works!</p>',
  })
  export class DetailComponent implements OnInit {
    constructor(private route: ActivatedRoute) {}

    ngOnInit() {
      const id = this.route.snapshot.params['id'];
      console.log('Route parameter ID:', id);
    }
  }
  ```

  **Accessing Query Parameters:**  
  Query parameters can be accessed using `ActivatedRoute`.  
  Example:

  ```typescript
  this.route.queryParams.subscribe((params) => {
    console.log('Query Params:', params);
  });
  ```

  **Accessing Route Data:**  
  Route data can be defined in the route configuration and accessed using `ActivatedRoute`.  
  Example:

  ```typescript
  const routes: Routes = [
    { path: 'detail', component: DetailComponent, data: { title: 'Detail Page' } },
  ];

  this.route.data.subscribe((data) => {
    console.log('Route Data:', data);
  });
  ```

- **Router Events**  
  Angular's `Router` emits various events during the navigation lifecycle. These events can be used to track navigation progress, handle errors, or perform custom logic.  

  **Common Events:**  
  - `NavigationStart`: Triggered when navigation starts.
  - `NavigationEnd`: Triggered when navigation ends successfully.
  - `NavigationCancel`: Triggered when navigation is canceled.
  - `NavigationError`: Triggered when navigation fails due to an error.
  - `RoutesRecognized`: Triggered when the router has parsed and recognized the route configuration.

  **Listening to Router Events:**  
  Use the `Router.events` observable to listen to these events.  
  Example:

  ```typescript
  import { Component, OnInit } from '@angular/core';
  import { Router, Event, NavigationStart, NavigationEnd } from '@angular/router';

  @Component({
    selector: 'app-root',
    template: '<router-outlet></router-outlet>',
  })
  export class AppComponent implements OnInit {
    constructor(private router: Router) {}

    ngOnInit() {
      this.router.events.subscribe((event: Event) => {
        if (event instanceof NavigationStart) {
          console.log('Navigation started:', event);
        } else if (event instanceof NavigationEnd) {
          console.log('Navigation ended:', event);
        }
      });
    }
  }
  ```

  **Use Cases for Router Events:**  
  - Displaying a loading spinner during navigation.
  - Logging navigation details for analytics.
  - Handling errors during navigation.

  **Example with Loading Spinner:**  
  ```typescript
  import { Component } from '@angular/core';
  import { Router, Event, NavigationStart, NavigationEnd } from '@angular/router';

  @Component({
    selector: 'app-root',
    template: `
      <div *ngIf="loading" class="loading-spinner">Loading...</div>
      <router-outlet></router-outlet>
    `,
  })
  export class AppComponent {
    loading = false;

    constructor(private router: Router) {
      this.router.events.subscribe((event: Event) => {
        if (event instanceof NavigationStart) {
          this.loading = true;
        } else if (event instanceof NavigationEnd) {
          this.loading = false;
        }
      });
    }
  }
  ```

- **Child Routes**  
  Child routes allow you to define nested routes within a parent route. This is useful for creating hierarchical navigation structures. Use the `children` property in the route configuration to define child routes.  

  **Defining Child Routes:**  
  Example:

  ```typescript
  const routes: Routes = [
    {
      path: 'parent',
      component: ParentComponent,
      children: [
        { path: 'child', component: ChildComponent },
      ],
    },
  ];
  ```

  **Using Router Outlet for Child Routes:**  
  Place a `<router-outlet>` in the parent component's template to render child components.  
  Example:

  ```html
  <h1>Parent Component</h1>
  <router-outlet></router-outlet>
  ```

- **Relative Navigation**  
  Relative navigation allows you to navigate to routes relative to the current route. This is particularly useful when working with child routes. Use the `relativeTo` property in the `navigate` method to specify the base route.  

  **Navigating to a Child Route:**  
  Example:

  ```typescript
  this.router.navigate(['child'], { relativeTo: this.route });
  ```

  **Navigating to a Parent Route:**  
  Example:

  ```typescript
  this.router.navigate(['../'], { relativeTo: this.route });
  ```

  **Navigating to a Sibling Route:**  
  Example:

  ```typescript
  this.router.navigate(['../sibling'], { relativeTo: this.route });
  ```

  **Other Options for `this.router.navigate`:**  
  The `navigate` method provides additional options to customize navigation behavior.  

  **Navigating with Query Parameters:**  
  Example:

  ```typescript
  this.router.navigate(['path'], { queryParams: { key: 'value' } });
  ```

  **Navigating with Fragment:**  
  Example:

  ```typescript
  this.router.navigate(['path'], { fragment: 'section' });
  ```

  **Using Navigation Extras:**  
  - `skipLocationChange`: Prevents the URL from being updated in the browser's address bar.
  - `replaceUrl`: Replaces the current entry in the browser's history stack instead of adding a new one.
  Example:

  ```typescript
  this.router.navigate(['path'], { skipLocationChange: true });
  this.router.navigate(['path'], { replaceUrl: true });
  ```

  **Combining Options:**  
  You can combine multiple options in a single navigation call.  
  Example:

  ```typescript
  this.router.navigate(['path'], {
    queryParams: { key: 'value' },
    fragment: 'section',
    skipLocationChange: true,
  });
  ```