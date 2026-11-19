# Fly LLVM toolchains

Prebuilt LLVM 20 packages for the [Fly](https://github.com/fly-lang/fly) bootstrap:
`libLLVM` (shared), `lld`, `compiler-rt`, `lldb`/`lldb-dap` and the headers, built
with the options Fly needs (no libxml2, no Python, no curses — a package that runs on
any machine of its platform without extra libraries).

The **sources** are the Fly fork of LLVM,
[fly-lang/llvm-project](https://github.com/fly-lang/llvm-project), branch
`fly-llvm-<version>`. This repository holds only the workflows that build them and the
releases they publish. It exists because the fork, a fork of a 3 GB repository, is not
given macOS arm64 runners by GitHub Actions (jobs are cancelled before they start),
while a repository of its own is.

## Releases

| Release | Asset | Platform |
|---|---|---|
| `v20.1.8-linux-x86_64` | `llvm-20.1.8-x86_64-linux-gnu.tar.gz` | Linux x86_64 |
| `v20.1.8-win-x64` | `llvm-20.1.8-win-x64.zip` | Windows x64 |
| `v20.1.8-macos-aarch64` | `llvm-20.1.8-aarch64-apple-darwin.tar.gz` | macOS arm64 (Apple Silicon), macOS 13+ |

The Linux and Windows packages were built in the fork and copied here **byte for byte**:
their SHA-256 is the one already pinned by the Fly seed
(`fly_0.13.x/cmake/LLVM-Project.cmake`) and by the self-host stage 0 scripts.

## Rules

- **A release is written once.** Each package is pinned by SHA-256 downstream; the
  workflows refuse to run when their release already exists. A new build is a new
  version.
- **By hand only.** Every workflow is `workflow_dispatch`:
  `gh workflow run build-macos-arm64.yml -R fly-lang/llvm`.
- The macOS package is checked for dependencies outside the system (Homebrew,
  `/usr/local`) before it is published.
