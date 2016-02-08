# shellstyle

A guide to write good, clean and compatible shell script code.

## Why?

Shell is the JavaScript of systems programming. Although in some cases it's better
to use a systems language like C or Go, Shell is an ideal systems language for
smaller POSIX-oriented or command line tasks. Here's three quick reasons why:

- It's everywhere. Like JavaScript for the web, every POSIX OS has a SH
interpreter and therefore is already there ready for systems programming.
- It's neutral. Unlike Ruby, Python, JavaScript, or PHP, Shell offends equally
across all communities. ;)
- It's made to be glue. Write complex parts in C or Go (or whatever!), and glue
them together with Shell.

This document is how I write shell and how I'd like collaborators to write shell
with me in my open source projects. It's based on a lot of experience and time
collecting best practices.

Keep in mind that these rules are made for based on the assumption you want
to write shell scripts that work ~everywhere~, not specifically Bash or ZSH.

## Big Rules

- Always double quote variables, including subshells. No naked `$` signs
This rule gets you pretty far.
- All code goes in a function. Even if it's one function, `main`.
Unless a library script, you can do global script settings and call `main`
or `main "$@"`.
- Avoid global variables. Though when defining constants use readonly.
// - Always use local when setting variables, unless there is reason to use declare
// Exception being rare cases when you are intentionally setting a variable in an outer scope.
- Variable names should be lowercase unless exported to environment.
- Always use `set -eo pipefail`. Fail fast and be aware of exit codes.
Use `|| true` on programs that you intentionally let exit non-zero.
- Never use some shell-specific style. Most notably:
  - `local`.
  - `[[`, use `test` or `[` instead.
  - Never use backticks, use `$( ... )`.
- Prefer absolute paths (leverage `$PWD`), always qualify relative paths with `./`.
- Always use declare and name variable arguments at the top of functions that are more than 2-lines
Example: `declare arg1="$1" arg2="$2"`
- Use `mktemp` for temporary files, always cleanup with a `trap`.
- Warnings and errors should go to `STDERR`, anything parsable should go to `STDOUT`.

