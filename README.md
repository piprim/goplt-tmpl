# goplt-tmpl

Official template collection for [goplt](https://github.com/piprim/goplt) — cookiecutter-style Go project scaffolding.

---

## Usage

Templates are fetched automatically by `goplt` via the Go module system. No manual cloning required.

```bash
# Install goplt
go install github.com/piprim/goplt/cmd/goplt@latest

# Generate a project from a template
goplt generate --template github.com/piprim/goplt-tmpl/cli-cobra

# Pin to a specific version
goplt generate --template github.com/piprim/goplt-tmpl/cli-cobra@v1.0.0
```

`goplt` will open an interactive form, collect your answers, render the template, and run post-generation hooks (`go mod tidy`, `git init`, `git add .`).

---

## Templates

### `cli-cobra` — Go CLI application

```bash
goplt generate --template github.com/piprim/goplt-tmpl/cli-cobra
```

Scaffolds a production-ready Go CLI using [Cobra](https://github.com/spf13/cobra) and [Viper](https://github.com/spf13/viper).

**Variables:**

| Name | Default | Description |
|---|---|---|
| `name` | _(required)_ | Binary name |
| `short` | _(required)_ | One-line description shown in help |
| `description` | _(required)_ | Longer description for README and `cobra.Long` |
| `org-prefix` | `github.com/acme` | Go module prefix |
| `author` | _(required)_ | LICENSE and README author |
| `version` | `0.1.0` | Initial semver |
| `with-config` | `true` | Viper config wiring + `configs/config.toml` |
| `with-docker` | `true` | `Dockerfile` + `.dockerignore` |
| `toolchain` | `make` | Task runner: `make` or `mise` |
| `license` | `MIT` | `MIT`, `Apache-2.0`, `GPL-3.0`, `BSD-3-Clause` |

**Generated structure** (a `<name>/` subdirectory is created automatically):

```
<name>/
  go.mod
  .gitignore
  README.md
  LICENSE
  Makefile              (or mise.toml)
  Dockerfile            (optional)
  .dockerignore         (optional)
  cmd/
    main.go
    cmd/
      root.go
      version/version.go
      hello/hello.go
      completion/completion.go
  internal/config/      (optional, with-config=true)
  configs/config.toml   (optional, with-config=true)
```

Pass `--output <dir>` to override the destination and bypass the automatic subdirectory.

**Features:**
- Subcommands in their own packages — no `init()`, explicit wiring in `NewRootCmd()`
- Structured logging via `log/slog` + [tint](https://github.com/lmittmann/tint) for development, JSON for production
- Environment-specific config overlay (`configs/config_<env>.toml`)
- `ldflags` version injection (`Version`, `Commit`, `BuildDate`)
- OS-aware build detection in Makefile

---

## Repository layout

Each template is an independent Go module in its own directory:

```
goplt-tmpl/
  cli-cobra/
    go.mod          ← module: github.com/piprim/goplt-tmpl/cli-cobra
    template.toml
    ...
```

This means each template is independently versioned. Pinning to `@v1.2.0` pins only that template, not the whole collection.

---

## Contributing

To add a new template, create a new directory with its own `go.mod` and a `template.toml` at its root.
See the [goplt documentation](https://github.com/piprim/goplt) for the `template.toml` reference.
