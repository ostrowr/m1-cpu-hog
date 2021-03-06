## Simple Go binary that sometimes leaves a hanging process

[main.go](main.go) is a simple go binary that sometimes leaves a hanging process that takes 100% CPU on
MacOS M1 machines.

[forkexec.c](forkexec.c) is a C program that forks a process in a new thread and then exits.

This seems to be a bug in the macOS kernel, not Go after all.

Unsure whether this is limited to MacOS M1; I've replicated on the following platforms so far:

- Replicated: go version go1.18.1 darwin/arm64
- Could not replicate: amd64 running under rosetta

Run `./test_race_c` or `./test_race_go` to create a binary with a random name, run the binary a bunch of times, and check for a process
created by that binary. It often takes several attemtpts to get a reproduction.

Run `kill_repros` to kill all of the hanging processes, which each take 90-100% CPU.

Related github issue: https://github.com/golang/go/issues/53863
