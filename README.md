# go-template-flake

A minimal Go practice environment, provided by a Nix flake. Write a few
files, try things out, throw them away.

## Quick start

```sh
nix develop        # or: direnv allow (enters automatically on cd)
go run .           # prints "Hello, Go!"
```

## What each file is for

| File | Needed by | Purpose |
|---|---|---|
| `go.mod` | Go | Declares the module path and the minimum Go version. The only file Go itself requires. |
| `main.go` | Go | Hello world in `package main`, to check the environment works. |
| `flake.nix` | Nix | Declares the devShell: `go` (the whole toolchain) and `gopls` (language server for your editor). |
| `flake.lock` | Nix | Pins the nixpkgs revision, and with it the Go version. Update with `nix flake update`. |
| `.envrc` | Convenience | Lets [direnv](https://direnv.net/) load the devShell on `cd`, so editors find `gopls`. Does nothing without direnv. |
| `.gitignore` | Convenience | Ignores `.direnv/` and the binary `go build` writes (`practice`, named after the module). |

Go needs nothing beyond the `go` binary and `go.mod`. Formatting (`go fmt`),
static checks (`go vet`), tests (`go test`) and dependency management
(`go mod`) are all part of the toolchain.

## Reset and reuse

Discard practice work and go back to the template:

```sh
git restore .      # undo changes to tracked files
git clean -fd      # delete new untracked files (keeps ignored ones such as .direnv/)
```

A copy made with "Use this template" (or `gh repo create --template`) works
as it is: the module is named `practice`, not after this repository. If you
ever publish the code for others to import, rename it to the repository's
path with `go mod edit -module github.com/<owner>/<repo>`, and update the
binary name in `.gitignore`.

To learn Go itself, see <https://go.dev/doc/>.
