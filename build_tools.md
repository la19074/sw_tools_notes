# Build Tools

Building software is often complex, so let's try to automate it!

## Make

You could write a shell script, but Make does everything for you.

Two main versions:

* **BSD Make** (More old fashioned, POSIX)  
* **GNU Make** (More features, Linux default)  


## Makefiles

### Rules

```make
hello: hello.c library.o
  cc -o hello hello.c library.o
library.o: library.c
  cc -c -o library.o library.c
```

Make will figure out which files it needs to compile based on what has changed. 

```sh
$ make hello
cc -c -o library.o library.c
cc -o hello hello.c library.o
```

### Phony Targets

By default Make only runs a target's commands if the target files (prerequisites) have changed. Phony targets don't depend on any files and just tell Make what to do when they're run. 

```make
.PHONY: all clean

all: hello coursework.zip flowchart.pdf

clean: git clean -dfx

hello: hello.c library.o
  cc -o hello hello.c library.o

library.o: library.c
  cc -c -o library.o library.c

coursework.zip: coursework
  zip -r coursework.zip coursework

flowchart.pdf: flowchart.dot
  dot -Tpdf flowchart.dot -O flowchart.pdf
```

`all` and `clean` are phony targets. 

```sh
$ make
cc -c -o library.o library.c
cc -o hello.o hello.c library.o
zip -r coursework.zip coursework
dot -Tpdf flowchart.dot -O flowchart.pdf
```

`make` defaults to the first target, which in this case is `all`. 

### Pattern Rules

`%`: wildcard, any characters  
`$@`: target name  
`$<`: first prerequisite  

```make
CC=clang
CFLAGS=-Wall -O3

.PHONY: all clean

all: hello coursework.zip flowchart.pdf

clean: git clean -dfx

%.o: %.c
  $(CC) $(CFLAGS) -c -o $@ $<

%: %.c
  $(CC) $(CFLAGS) -o $@ $<

%.zip: %
  zip -r $@ $<

%.pdf: %.dot
  dot -Tpdf $< -O $@
```

### Implicit Pattern Rules

Make can cimpile C (and Fortran/Pascal...) code implicitly:

```make
hello: hello.c library.o extra-library.o
```

### Variables and Subshells

```make
.PHONY: all
figures=$(patsubst .dot,.pdf,$(wildcard *.dot))

all: ${figures}

%.pdf: %.dot
  dot -Tpdf $< -O $@
```

Make works with variables and subshells too!

## Language Specific Buildtools

Modern software development often makes use of external libraries. Make isn't very good at tracking dependencies, but most languages have their own library management tools.

**Commonlisp** ASDF and Quicklisp  
**Go** Gobuild  
**Haskell** Cabal  
**Java** Ant, Maven, Gradle…  
**JavaScript** NPM  
**Perl** CPAN  
**Python** Distutils and requirements.txt  
**R** CRAN  
**Ruby** Gem  
**Rust** Cargo  
**LATEX** CTAN and TeXlive  

### Maven

```sh
mvn archetype:generate
```

This will generate a new folder with a Maven file (and the correct file structure). Maven will ask you for a few settings: press enter to use the default ones. 

Other recommended settings:  
**groupId**: uk.ac.bristol.cs  
**artifactId**: use project name  
**version**: start with 0.1?  

Source files are found under `src/main` and test files are found under `src/test`.  

`pom.xml` contains information on settings and dependencies. 

#### Other Commands

`mvn test` run the test suite  
`mvn install` install the JAR into your local JAR packages  
`mvn clean` delete everything  