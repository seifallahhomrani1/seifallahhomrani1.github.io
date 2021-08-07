---
title: "Go - Introduction"
author : Seif-Allah
layout: post
category : go
tags : [go,programming]
description : Introduction to the Go programming language
image : /assets/images/go/intro.png
toc : true
---

## Introduction

The Go programming language is an *open source* project to make programmers more productive.

Go is expressive, concise, clean, and efficient. Its concurrency mechanisms make it easy to write programs that get the most out of multicore and networked machines, while its novel type system enables flexible and modular program construction. Go compiles quickly to machine code yet has the convenience of *garbage collection* and the power of *run-time reflection*. It's a *fast*, *statically typed*, compiled language that feels like a dynamically typed, *interpreted* language.



## Why GO

While I'm documenting for this learning journey, I read about how big companies like 'Facebook', 'Cloudflare', 'Dropbox' and many others have implemented go language in their products and services or even [*graceful upgrading*](https://blog.cloudflare.com/graceful-upgrades-in-go/) their infrastructure and the major benefit they are all happy about is the speed increase: "Finally, we sped up our application from more than 2.5 seconds to less than 250 milliseconds for longest request." ([Allegro](https://blog.allegro.tech/2016/03/writing-fast-cache-service-in-go.html)).

Link for the other case studies : [https://go.dev/solutions/#case-studies](https://go.dev/solutions/#case-studies)

## Install Go

You can actually find a comprehensive guide about how to install go in your working environment and get started coding here: [https://golang.org/doc/tutorial/getting-started](https://golang.org/doc/tutorial/getting-started)
In addition, you can run go inside a [Docker container](https://hub.docker.com/_/golang)
## Hello, World!

```go
package main

import "fmt"

func main() {
	fmt.Println("Hello, World!")
}
```
## Effective Go

Go is a new language. Although it borrows ideas from existing languages, it has unusual properties that make effective Go programs different in character from programs written in its relatives. A straightforward translation of a C++ or Java program into Go is unlikely to produce a satisfactory result—Java programs are written in Java, not Go. On the other hand, thinking about the problem from a Go perspective could produce a successful but quite different program. In other words, to write Go well, it's important to understand its properties and idioms. It's also important to know the established conventions for programming in Go, such as naming, formatting, program construction, and so on, so that programs you write will be easy for other Go programmers to understand.

### Packages

Go programs are organized into packages. A package is a collection of source files in the same directory that are compiled together. Functions, types, variables, and constants defined in one source file are visible to all other source files within the same package.

The standard library that ships with Go is a set of packages. These packages contain many of the fundamental building blocks to write modern software. For instance, the fmt package contains basic functions for formatting and printing strings. The net/http package contains functions that allow a developer to create web services, send and retrieve data over the http protocol, and more.

To make use of the functions in a package, you need to access the package with an import statement. An import statement is made up of the import keyword along with the name of the package.


### Formatting
What's been impressive for me was the formatting approach; Go let the machine take care of most formatting issues, the *gofmt* program reads a Go program and emits the source in a standard style of indentation and vertical alignment,retaining and if necessary reformatting comments.

### Semicolons

Like C, Go's formal grammar uses semicolons to terminate statements, but unlike in C, those semicolons do not appear in the source. Instead the lexer uses a simple rule to insert semicolons automatically as it scans, so the input text is mostly free of them.

The rule is this. If the last token before a newline is an identifier (which includes words like int and float64), a basic literal such as a number or string constant, or one of the tokens: **break contine fallthrough return ++ -- ) }**

**For more : [https://golang.org/doc/effective_go](https://golang.org/doc/effective_go)**  

## First Program (QR Code Generator)

The purpose of our first program is to generate a QR code from either a file containing a hash where its location is specified as a cli argument or either passed directly. It's going to act as an introduction for our next project. (Hash supposed valid, no tests included)
```go
package main

import (
	"flag"
	"fmt"
	qrcode "github.com/skip2/go-qrcode"
	"log"
	"math/rand"
	"os"
	"path/filepath"
	"strconv"
	"time"
)

func encode(hash string) {
	var filename string
	rand.Seed(time.Now().UnixNano())
	filename = "qr" + strconv.Itoa(rand.Intn(100)) + ".png"
	err := qrcode.WriteFile(hash, qrcode.High, 256, filename)
	if err != nil {
		log.Fatal(err)
	} else {
		absolute, err := filepath.Abs(filename)
		if err != nil {
			log.Fatal(err)
		} else {
			fmt.Println("Encoding Succeeded!\nAbsolute filepath:", absolute)
		}
	}
}

func main() {
	var location string
	var hash string
	flag.StringVar(&location, "l", "", "Specify file location")
	flag.StringVar(&hash, "t", "", "Specify hash to encode")
	flag.Parse()

	if location != "" {
		file, err := os.ReadFile(location)
		if err != nil {
			log.Fatal(err)
		} else {
			hash = string(file)
			encode(hash)
		}
	} else if hash != "" {
		encode(hash)
	} else {
		fmt.Println("Use -h flag to understand the usage.")
	}

}

```






## Resources

* Flag package: [https://www.digitalocean.com/community/tutorials/how-to-use-the-flag-package-in-go](https://www.digitalocean.com/community/tutorials/how-to-use-the-flag-package-in-go)
* Effective Go: [https://golang.org/doc/effective_go](https://golang.org/doc/effective_go)
* Official documentaion: [https://golang.org/doc/](https://golang.org/doc/)
