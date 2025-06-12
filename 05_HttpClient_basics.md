## HttpClient Basics

- **Overview**  
  The `HttpClient` service in Angular is a powerful tool for making HTTP requests to interact with backend APIs. It simplifies communication with remote servers and supports features like request/response handling, error management, and data transformation.

- **Key Features**  
  - Provides methods for all HTTP verbs: `GET`, `POST`, `PUT`, `DELETE`, etc.  
  - Built-in support for RxJS, enabling reactive programming with streams.  
  - Simplifies error handling and response transformation.  
  - Supports interceptors for request/response modification.

- **Making HTTP Requests**  
  Use methods like `get`, `post`, `put`, and `delete` for CRUD operations.  
  Example:

  ```typescript
  this.http.post("/api/data", { name: "Angular" }).subscribe({
    next: (response) => console.log("Data saved successfully:", response),
    error: (err) => console.error("Error saving data:", err),
    complete: () => console.log("Request completed."),
  });
  ```

- **Handling Responses**  
  Leverage RxJS operators like `map`, `catchError`, and `tap` to process and transform responses.  
  Example:

  ```typescript
  this.http.get("/api/data").pipe(
    tap((response) => console.log("Raw response:", response)),
    map((response) => response.data),
    catchError((error) => {
      console.error("Error occurred while fetching data:", error);
      return throwError(() => new Error("Failed to fetch data"));
    })
  ).subscribe({
    next: (data) => console.log("Processed data:", data),
    error: (err) => console.error("Error in subscription:", err),
    complete: () => console.log("Data fetch completed."),
  });
  ```

- **Error Handling**  
  Handle errors gracefully using RxJS `catchError` operator.  
  Example:

  ```typescript
  this.http.get("/api/data").pipe(
    catchError((error) => {
      console.error("Error occurred:", error);
      return throwError(() => new Error("Something went wrong"));
    })
  ).subscribe();
  ```

- **Using Interceptors**  
  Interceptors allow you to modify requests or responses globally. For example, adding authentication tokens or logging requests.  
  Example:

  ```typescript
  import { Injectable } from '@angular/core';
  import { HttpEvent, HttpHandler, HttpInterceptor, HttpRequest, HttpResponse } from '@angular/common/http';
  import { Observable, throwError } from 'rxjs';
  import { catchError, tap } from 'rxjs/operators';

  @Injectable()
  export class AuthInterceptor implements HttpInterceptor {
    intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
      const clonedRequest = req.clone({
        setHeaders: { Authorization: `Bearer YOUR_DYNAMIC_TOKEN` },
      });
      console.log("Request intercepted and modified:", clonedRequest);

      return next.handle(clonedRequest).pipe(
        tap((event) => {
          if (event instanceof HttpResponse) {
            console.log("Response intercepted:", event);
          }
        }),
        catchError((error) => {
          console.error("Error intercepted:", error);
          return throwError(() => new Error("Request failed"));
        })
      );
    }
  }
  ```

  To use this interceptor, register it in your Angular module:

  ```typescript
  import { NgModule } from '@angular/core';
  import { HTTP_INTERCEPTORS } from '@angular/common/http';
  import { AuthInterceptor } from './auth.interceptor'; // Adjust the path as needed

  @NgModule({
    providers: [
      {
        provide: HTTP_INTERCEPTORS,
        useClass: AuthInterceptor,
        multi: true, // Allows multiple interceptors to be used
      },
    ],
  })
  export class AppModule {}
  ```

  Once registered, the `AuthInterceptor` will automatically modify all HTTP requests made using `HttpClient`. For example:

  ```typescript
  this.http.get('/api/data').subscribe({
    next: (data) => console.log('Data:', data),
    error: (err) => console.error('Error:', err),
  });
  ```

  To use this interceptor in a standalone component setup, you can provide it directly in the component's `providers` array:

  ```typescript
  import { Component } from '@angular/core';
  import { HTTP_INTERCEPTORS } from '@angular/common/http';
  import { AuthInterceptor } from './auth.interceptor'; // Adjust the path as needed

  @Component({
    selector: 'app-standalone',
    templateUrl: './standalone.component.html',
    styleUrls: ['./standalone.component.css'],
    providers: [
      {
        provide: HTTP_INTERCEPTORS,
        useClass: AuthInterceptor,
        multi: true, // Allows multiple interceptors to be used
      },
    ],
  })
  export class StandaloneComponent {
    constructor(private http: HttpClient) {
      this.http.get('/api/data').subscribe({
        next: (data) => console.log('Data:', data),
        error: (err) => console.error('Error:', err),
      });
    }
  }
  ```

  **Note:** When using standalone components, the interceptor will only apply to HTTP requests made within that specific component or its children.

  For a more modern and declarative approach in standalone applications, you can use `provideHttpClient` and `withInterceptors`:

  ```typescript
  import { bootstrapApplication } from '@angular/platform-browser';
  import { AppComponent } from './app.component';
  import { provideHttpClient, withInterceptors } from '@angular/common/http';

  bootstrapApplication(AppComponent, {
    providers: [
      provideHttpClient(
        withInterceptors([AuthInterceptor])
      ),
    ],
  }).catch((err) => console.error(err));
  ```

  **Note:** This approach is ideal for standalone applications or components, allowing you to declaratively register interceptors.
