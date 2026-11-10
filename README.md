# go-template-flake

A minimal Go practice environment, provided by a Nix flake. The template
holds only the Nix side; the Go module is created with `go mod init` in each
copy.

## Quick start

### 1. Make a copy

With the GitHub CLI, as a new repository of your own:

```sh
gh repo create my-go --template hypatia-tile/go-template-flake --private --clone
cd my-go
```

Or with plain `git`, as a local directory with fresh history:

```sh
git clone --depth 1 https://github.com/hypatia-tile/go-template-flake.git my-go
cd my-go
rm -rf .git && git init
```

### 2. Enter the devShell and create the module

```sh
nix develop                # or: direnv allow (enters automatically on cd)
go mod init example/hello  # any name; use github.com/<owner>/<repo> if others will import it
```

### 3. Write some code and run it

```sh
cat > main.go <<'EOF'
package main

import "fmt"

func main() {
	fmt.Println("Hello, Go!")
}
EOF
go run .                   # prints "Hello, Go!"
```

## What each file is for

| File | Needed by | Purpose |
|---|---|---|
| `flake.nix` | Nix | Declares the devShell: `go` (the whole toolchain) and `gopls` (language server for your editor). |
| `flake.lock` | Nix | Pins the nixpkgs revision, and with it the Go version. Update with `nix flake update`. |
| `.envrc` | Convenience | Lets [direnv](https://direnv.net/) load the devShell on `cd`, so editors find `gopls`. Does nothing without direnv. |
| `.gitignore` | Convenience | Ignores `.direnv/`. Add the binary `go build` writes, named after the last element of the module path (`/hello` for `example/hello`). |

Go needs nothing beyond the `go` binary and the `go.mod` that `go mod init`
writes. Formatting (`go fmt`), static checks (`go vet`), tests (`go test`)
and dependency management (`go mod`) are all part of the toolchain.

## Reset and reuse

Discard practice work and go back to the last commit:

```sh
git restore .      # undo changes to tracked files
git clean -fd      # delete new untracked files (keeps ignored ones such as .direnv/)
```

To learn Go itself, see <https://go.dev/doc/>.
