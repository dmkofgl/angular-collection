## Angular CLI

- **What is Angular CLI?**  
  Angular CLI (Command Line Interface) is a powerful tool designed to streamline the development process of Angular applications. It provides developers with a set of commands to create, build, test, and deploy Angular projects efficiently. By automating repetitive tasks, Angular CLI helps maintain consistency and reduces the chances of human error.

  Example:

  ```bash
  ng new my-app
  ```
  This command initializes a new Angular project named `my-app` with a pre-configured structure and default settings.

- **Why Use Angular CLI?**  
  - **Standardized Project Structure:** Ensures a consistent and scalable project layout.
  - **Productivity Boost:** Automates common tasks like scaffolding components, services, and modules.
  - **Built-in Best Practices:** Enforces Angular's recommended coding standards and configurations.
  - **Seamless Integration:** Works seamlessly with Angular's ecosystem, including testing and deployment tools.

- **Common CLI Commands**  
  Angular CLI offers a wide range of commands to simplify development. Below are some commonly used ones:

  - **Serve the Application:**
    ```bash
    ng serve
    ```
    This command starts a development server, watches for file changes, and reloads the application automatically in the browser.

  - **Build the Application:**
    ```bash
    ng build --prod
    ```
    Compiles the application into an optimized production-ready bundle.

  - **Generate Components, Services, and More:**
    ```bash
    ng generate component my-component
    ng generate service my-service
    ng generate module my-module
    ```
    These commands scaffold Angular building blocks with boilerplate code, saving time and ensuring consistency.

- **Generate Options**  
  The `ng generate` command is used to scaffold various Angular building blocks. Below is a comprehensive list of options:

  - **Component:**
    ```bash
    ng generate component <name>
    ```
    Creates a new Angular component.

  - **Directive:**
    ```bash
    ng generate directive <name>
    ```
    Creates a new Angular directive.

  - **Service:**
    ```bash
    ng generate service <name>
    ```
    Creates a new Angular service.

  - **Module:**
    ```bash
    ng generate module <name>
    ```
    Creates a new Angular module.

  - **Pipe:**
    ```bash
    ng generate pipe <name>
    ```
    Creates a new Angular pipe.

  - **Class:**
    ```bash
    ng generate class <name>
    ```
    Creates a new TypeScript class.

  - **Guard:**
    ```bash
    ng generate guard <name>
    ```
    Creates a new Angular route guard.

  - **Interface:**
    ```bash
    ng generate interface <name>
    ```
    Creates a new TypeScript interface.

  - **Enum:**
    ```bash
    ng generate enum <name>
    ```
    Creates a new TypeScript enum.

  - **Application:**
    ```bash
    ng generate application <name>
    ```
    Creates a new Angular application within a workspace.

  - **Library:**
    ```bash
    ng generate library <name>
    ```
    Creates a new Angular library within a workspace.

  - **Component Store:**
    ```bash
    ng generate component-store <name>
    ```
    Creates a new component store for state management (if using NgRx).

  Each of these options can be customized further with flags and parameters to suit specific project requirements.

  - **Run Unit Tests:**
    ```bash
    ng test
    ```
    Executes unit tests in a watch mode using the configured testing framework.

  - **Run End-to-End Tests:**
    ```bash
    ng e2e
    ```
    Launches end-to-end tests to validate the application's functionality in a browser environment.

  - **Add Features or Libraries:**
    ```bash
    ng add @angular/material
    ```
    Installs and configures third-party libraries or Angular features, such as Angular Material.

  - **Lint the Codebase:**
    ```bash
    ng lint
    ```
    Analyzes the code for potential errors and enforces coding standards.

- **Customizing Angular CLI**  
  Angular CLI can be customized using the `angular.json` configuration file. This file allows developers to modify build options, specify file paths, and define custom scripts to suit their project's needs.
