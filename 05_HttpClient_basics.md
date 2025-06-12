
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
