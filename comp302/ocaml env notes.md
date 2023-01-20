### Environment Setup
- [Install guide for ocaml](https://cs3110.github.io/textbook/chapters/preface/install.html)
	- You must create an *opam switch*, which is a named installation of OCaml with a particular compiler version and set of packages. View the your different switches by typing `opam switch list` in terminal.
- Launch the universal toplevel in terminal with `utop`. Remember to end your statements with `;;`. To exit toplevel, do `Ctrl-D`.
	- Load the code from a file to execute in the toplevel with the directive `#use "filename.ext" ;;`. [More info](https://cs3110.github.io/textbook/chapters/basics/toplevel.html#loading-code-in-the-toplevel) 
- The double semicolon is intended for interactive sessions in the toplevel, so that the toplevel knows you are done entering a piece of code. There’s usually no reason to write it in a .ml file.
- Compile an OCaml file: `$ ocamlc -o hello.byte hello.ml`
	- It will generate an executable `./hello.byte` and a couple other files that you don't need to worry about. 
- Instead of doing the manual compilation/build process, we can use a *build system* called Dune (equivalent to something like `make` for C). 
- If you're getting annoying warnings that show up as errors with Dune you can turn them off [as explained here](https://stackoverflow.com/questions/73157592/unused-let-expression-in-ocaml). 

### Dune Info
[AlexandreKrantz/dune-example (github.com)](https://github.com/AlexandreKrantz/dune-example)
- The `dune` file tells Dune how you want the code in that directory (and subdirectories) to be compiled.
- The `dune-project` file is needed in the root directory of every source tree that you want to compile with Dune. It tells Dune what version of Dune to use for this project.
- In general, you’ll have a dune file in every subdirectory of the source tree but only one dune-project file at the root.
- When you run `dune build test.exe` Dune will create a directory `_build` and compile our program inside it. That’s one benefit of the build system over directly running the compiler: instead of polluting your source directory with a bunch of generated files, they get cleanly created in a separate directory. Inside `_build` there are many files that get created by Dune. Our executable is buried a couple of levels down:
```bash
$ _build/default/test.exe
Hello world!
```

To build and execute the program in one step, we can simply run:

```bash
dune exec ./test.exe
Hello world!
```
Finally, to clean up all the compiled code we just run:
```bash
dune clean
```
That removes the _build directory, leaving just your source code.
