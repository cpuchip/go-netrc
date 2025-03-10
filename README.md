# go-netrc

A Golang package for reading and writing netrc files. This package can parse netrc
files, make changes to them, and then serialize them back to netrc format, while
preserving any whitespace that was present in the source file.

[![GoDoc](https://godoc.org/github.com/cpuchip/go-netrc?status.png)][godoc]

[godoc]: https://godoc.org/github.com/cpuchip/go-netrc "go-netrc on Godoc.org"

## Usage

Below is a brief example of how you can load and parse your `~/.netrc` file and
retrieve your `gitlab.com` machine’s password:

```go
package main

import (
	"fmt"
	"log"
	"os"
	"path/filepath"

	"github.com/cpuchip/go-netrc/netrc"
)

func main() {
	// Construct a path to ~/.netrc
	home, err := os.UserHomeDir()
	if err != nil {
		log.Fatal(err)
	}
	path := filepath.Join(home, ".netrc")

	// Parse the file
	n, err := netrc.ParseFile(path)
	if err != nil {
		log.Fatal(err)
	}

	// Find the 'gitlab.com' machine
	machine := n.FindMachine("gitlab.com")
	if machine == nil {
		fmt.Println("gitlab.com not found in netrc")
		return
	}

	fmt.Printf("Your GitLab password is: %s\n", machine.Password)
}
```
