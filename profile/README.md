# configuration-management-tool

[![License](https://img.shields.io/badge/license-BSD--3--Clause-blue.svg)](https://github.com/configuration-management-tool/cmt/blob/main/LICENSE)
[![Pure Go](https://img.shields.io/badge/pure%20Go-CGO%3D0-00ADD8?logo=go&logoColor=white)](https://github.com/configuration-management-tool/cmt)
[![Go Reference](https://pkg.go.dev/badge/github.com/configuration-management-tool/cmt.svg)](https://pkg.go.dev/github.com/configuration-management-tool/cmt)

**Run named commands, in parallel, against named groups of hosts.**

A deploy needs the same handful of moves every time: pick a group of hosts,
run a sequence of commands against them, upload a few files, become somebody
else first if you have to. Most teams either hand-roll that in shell or drag
in a full configuration-management system to do a job that doesn't need one.

## What is here

**[cmt](https://github.com/configuration-management-tool/cmt)** — a
super-simple deployment tool: it runs named shell commands, defined in a
manifest, against named groups of hosts in parallel. It is a from-scratch,
pure-Go (`CGO_ENABLED=0`) reimplementation of the idea behind
[pressly/sup](https://github.com/pressly/sup) ("Stack Up") — full credit to
that project for the design this borrows — but `cmt` is **not**
Supfile-compatible. The manifest language is
[HCL2](https://github.com/hashicorp/hcl) instead of YAML, a group of hosts is
a `hosts_group` instead of a `network`, and connections (SSH, WinRM, local
exec, privilege escalation) are handled by the shared library
[`go-remoteexec/transport`](https://github.com/go-remoteexec/transport)
instead of a bespoke implementation.

```hcl
hosts_group "production" {
  hosts = ["api1.example.com", "api2.example.com"]
}

command "restart" {
  run    = "sudo docker restart example"
  serial = 2
}

target "deploy" { commands = ["restart"] }
```

```sh
cmt production deploy
```

See the [cmt README](https://github.com/configuration-management-tool/cmt)
for the full manifest reference and CLI usage, and the
[docs site](https://configuration-management-tool.github.io/docs/) for the
organisation-level index.

Part of the **Configuration management** family of the
[pure-Go ecosystem](https://go-desktop.github.io/): no cgo, no shelling out to
a command-line tool in place of a library, built on six 64-bit architectures,
100% statement coverage as a CI gate, BSD-3-Clause.
