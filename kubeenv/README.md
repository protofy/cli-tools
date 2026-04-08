# kubeenv

Interactive TUI for switching between Kubernetes contexts.

Built with [Bubble Tea](https://github.com/charmbracelet/bubbletea) and [Lip Gloss](https://github.com/charmbracelet/lipgloss).

## Features

- Lists all available `kubectl` contexts
- Highlights the currently active context
- Fuzzy filtering to quickly find contexts
- Switches context on selection via `kubectl config use-context`

## Requirements

- `kubectl` configured with at least one context
- golang, if you want to work on it, or build it from source

## Install

### Build it yourself
```sh
go install github.com/protofy/cli-tools/kubeenv@latest
```
This will install it in your `$GOPATH/bin`, which should hopefully be in your $PATH already.

### Use the prebuilt Release
 1. Download the current release from the [release page](https://github.com/protofy/cli-tools/releases)
 2. Run `chmod +x kubeenv2`
 3. Run `mv kubeenv2 ~/bin/`

## Usage

```sh
kubeenv
```

Use arrow keys to navigate, type to filter, press Enter to switch context.
