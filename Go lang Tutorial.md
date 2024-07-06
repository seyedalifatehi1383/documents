## Installation
I pesonally installed using `pacman` package manager of arch Linux.
```shell
doas pacman -S go
```
(you can also use `sudo` instead of `doas` but `doas` is better in minimallity)
(
	for configure `doas` you can do these steps:
	1. install `doas` using `sudo pacman -S doas` command.
	2. make `doas.conf` file in `/etc` folder located in `root`.
	3. then write `permit <your-username-on-linux>` in this file and save it.
	Your `doas` is ready to use. 
)
## Dependencies, modules, packages and enable dependency tracking for our code
### Modules
A **module** is a collection of related Go packages that are versioned together as a single unit. Modules provide a way to manage and distribute Go code. They help in dependency management by tracking the versions of the libraries your project depends on.

- **Definition**: A module is defined by a `go.mod` file at its root. This file declares the module path (often corresponding to the URL where it’s hosted) and its dependencies.
- **Purpose**: Modules enable versioning and dependency management, ensuring that you can specify and control which versions of dependencies your project uses.

#### Module path
The **module path** is a unique identifier for a Go module. It typically represents the repository or location where the module's source code is hosted. The module path is specified when you initialize a module using the [`go mod init` command](https://go.dev/ref/mod#go-mod-init) and is stored in the `go.mod` file.

### Dependencies
A **dependency** is an external module or library that your code relies on.

- **Management**: Dependencies are listed in the `go.mod` file under the `require` directive.
- **Versioning**: The `go.mod` file specifies which versions of dependencies your module is compatible with.

### Packages
A **package** is a way to group functions, and it's made up of all the files in the same directory.

- **Definition**: A package is defined by a directory containing one or more Go source files, all with the same package name at the top of the file.
- **Usage**: Packages are imported and used in other Go files to reuse code.

### Enable dependency tracking for your code 
When your code imports packages contained in other modules, you manage those dependencies through your code's own module. That module is defined by a `go.mod` file that tracks the modules that provide those packages. That `go.mod` file stays with your code, including in your source code repository.

To enable dependency tracking for your code by creating a go.mod file, run the `go mod init` command, giving it the name of the module your code will be in. The name is the module's module path.
```shell
go mod init <mod-path>
```


In actual development, the module path will typically be the repository location where your source code will be kept. For example, the module path might be `github.com/mymodule`. If you plan to publish your module for others to use, the module path _must_ be a location from which Go tools can download your module.

## Call code in an external package
