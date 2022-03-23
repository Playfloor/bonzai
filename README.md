# Bonzai! Dominate the Command Line

🚧 *under construction* 🚧

[![Go Version](https://img.shields.io/github/go-mod/go-version/rwxrob/bonzai)](https://tip.golang.org/doc/go1.18)
[![GoDoc](https://godoc.org/github.com/rwxrob/bonzai?status.svg)](https://godoc.org/github.com/rwxrob/bonzai)
[![License](https://img.shields.io/badge/license-Apache2-brightgreen.svg)](LICENSE)
[![Go Report
Card](https://goreportcard.com/badge/github.com/rwxrob/bonzai)](https://goreportcard.com/report/github.com/rwxrob/bonzai)

## Installation

🎉 ***Bonzai shamelessly requires Go 1.18+*** 💋

1. Install Go 1.18 and the tooling your require for it
1. `go install github.com/rwxrob/bonzai@latest` 
1. `import "github.com/rwxrob/bonzai"`
1. Consider using the [template][] to get started

[template]: <https://github.com/rwxrob/bonzai-template>

😎 *Yes, we use the wonderful new generics all [over](fn).* 👍

## Welcome to Bonzai

Yes, "banzai" is something people yell going into battle. But isn't
that what making command line utilities in Go (instead of your favorite
shell script) actually is? 

And yes, "bonsai" trees are well-manicured, meticulously crafted,
miniature trees that rival their larger cousins, just like Bonzai
command and data node trees. They are unlike anything you've probably
encountered so far, no getopt dashes (we kind of hate them), no ugly
commander interface to learn, no 12637 lines of shell tab completion
bloat to source before your command will complete, just well manicured
nested-tab-complete-with-magical-aliases-enabled commands organized into
rooted node trees for your command-line enjoyment. Your right-pinky will
be particularly grateful.

But wait, there's more! What about all those other tasks you need to do
to make a command line application honorable in anyone's eyes? Tools are
needed. 

## Contributors/PRs Welcome

*... especially for "Completers", included popular commands, and Runtime
Detection.*

Speaking of sharing, why not send a PR for your addition to the ever
growing collection of `comp` subpackage `Completers` for everything from
days of the week, to tab-driven inline math calculations, to a list of
all the listening ports running on your current system.

[CONTRIBUTING](CONTRIBUTING)

## "It's spelled bonsai/banzai."

We know. The domains were taken. Plus, this makes it more unique and
easier to find once you know the strange spelling we chose to use. Sorry
if that triggers your OCD.

If you must know, the primary motivator was the similarity to a
well-manicured tree (since it is for creating trees of commands). And
Buckaroo Banzai was always a favorite. We like to think he would use
Bonzai today to make amazing things.

On a lighter note, it just so happens that "banzai" means 'a traditional
Japanese idiom meaning "ten thousand years" of long life,' a cheer used
in celebrations. So combining the notion of a happy, little,
well-manicured, beautiful tree and "ten thousand years of long life"
works out just fine for us.

It turns out that the "call to war" associated with Bonzai is not
entirely without merit as well. Bonzai makes short work of creating
offensive and defensive tool kits all wrapping into one nice Go binary,
popular for building single-binary Linux container distros (like BusyBox
and Alpine, watch for Bonzai Linux soon), as well as root kits, and
other security tools

## What People Are Saying

> "It's like a modular, multicall BusyBox builder for Go with built in
> completion and embedded documentation support."

> "The utility here is that Bonzai lets you maintain your own personal
> 'toolbox' with built in auto-complete that you can assemble from
> various Go modules. Individual commands are isolated and unaware of
> each other and possibly maintained by other people." (tadasv123)

## Example GitHub Template

<https://github.com/rwxrob/bonzai-template>

## Design Considerations

* **Promote high-level package library API calls over Cmd bloat.** Code
  belongs in package libraries, not Cmds. While Bonzai allows for rapid
  applications development by putting everything initially in Cmd
  Methods, Cmds are most designed for documentation and completion, not
  heavy Method implementations. Eventually, most Method implementations
  should be moved into their own package libraries, perhaps even in the
  same Go module. Cmds should *never* be communicating with each other
  directly. While the idea of adding a Channel attribute was intriguing,
  it quickly became clear that doing so would promote too much code and
  tight coupling --- even with channels --- between specific commands.
  Cmds should be *very* light. In fact, most Cmds should assign their
  Method directly from one matching the Method function signature in a
  callable, high-level library.

* **Only bash completion planned.** Zsh, Powershell, and Fish have no
  equivalent to `complete -C` (which allows any executable to provide
  its own completion) This forces inferior dependencies on overly verbose
  external "completer" scripts written in only those languages for
  those specific shells. This dependency completely negates any
  possibility of providing modular completers and composable commands
  that carry their own completion logic. This one objective fact alone
  should give every pause before opting to use one of these inferior
  shells for their command line interactions. Bonzai commands leverage
  this best-of-breed completion functionality of bash to provide an
  unlimited number of completion methods and combinations. The
  equivalent implementations, perhaps as an export collected from all
  composed commands providing their shell equivalent of completion
  scripts, would be preposterously large (even though `kubectl` requires
  12637 lines of bash just for its basic completion). Bonzai uses Go
  itself to manage that completion --- drawing on a rich collection of
  completers included in the standard Bonzai module --- and provides
  documented shortcut aliases when completion is not available (h|help,
  for example). 

* **Bonzai commands may default to `shell.Cmd` or `help.Cmd`** These
  provide help information and optional interactive assistance
  including tab completion in runtime environments that do not have
  `complete -C foo foo` enabled. 

* **One major use case is to replace shell scripts in "dot files"
  collections.** By creating a `cmd` subdirectory of a dot files repo a
  multi-call Bonzai command named `cmd` can be easily maintained and
  added to just as quickly as any shell script. This has the added bonus
  of allowing others to quickly add one of your little commands with
  just a simple import (for example, `import github.com/rwxrob/dot/cmd`
  and then `cmd.Isosec`) from their own `cmd` monoliths. This also
  enables the fastest possible prototyping of code that would otherwise
  require significant, problematic mocks. Developers can work out the
  details of a thing just as fast as with shell scripting --- but with
  the power of all the Go standard library --- and then factor out their
  favorites as they grow into their own Bonzai command repos. This
  approach keeps "Go on the brain" (instead of having to port a bunch of
  bash later) and promotes the massive benefits of rapid applications
  development the fullest extent.

* **Use either `foo.Cmd` or `cmd.Foo` convention.** People may decide to
  put all their Bonzai commands into a single `cmd` package or to put
  each command into its own package. Both are perfectly acceptable and
  allow the developer making the import to alias the packages as needed
  using Go's excellent package import design.

## Style Guidelines

* Everything through `go fmt` or equiv, no exceptions
* In Vim `set textwidth=72` (not 80 to line numbers fit)
* Use `/* */` for package documentation comment, `//` elsewhere
* Smallest possible names for given scope while still clear
* Favor additional packages (possibly in `internal`) over long names
* Package globals that will be used a lot can be single capital
* Must be good reason to use more than 4 character pkg name
* Avoid unnecessary comments

## Acknowledgements

The <https://twitch.tv/rwxrob> community has been constantly involved
with the development of this project, making suggestions about
everything from my use of init, to the name "bonzai". While all their
contributions are too numerous to name specifically, they 
more than deserve a huge thank you here.

## Legal

Copyright 2022 Robert S. Muhlestein (<mailto:rob@rwx.gg>)  
SPDX-License-Identifier: Apache-2.0

"Bonzai" and "bonzai" are legal trademarks of Robert S. Muhlestein but
can be used freely to refer to the Bonzai project
<https://github.com/rwxrob/bonzai> without limitation. To avoid
potential developer confusion, intentionally using these trademarks to
refer to other projects --- free or proprietary --- is prohibited.
