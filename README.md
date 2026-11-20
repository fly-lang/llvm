# Fly LLVM toolchains

Prebuilt LLVM 20 packages for the [Fly](https://github.com/fly-lang/fly) bootstrap:
`libLLVM` (shared), `lld`, `compiler-rt`, `lldb`/`lldb-dap` and the headers, built
with the options Fly needs (no libxml2, no Python, no curses — a package that runs on
any machine of its platform without extra libraries).

The **sources** are upstream LLVM,
[llvm/llvm-project](https://github.com/llvm/llvm-project), at its release tag
`llvmorg-<version>`, **unmodified**: Fly needs no patch, only the CMake options in the
workflows. This repository holds only those workflows and the releases they publish.

It replaces the former fork [fly-lang/llvm-project](https://github.com/fly-lang/llvm-project),
which GitHub Actions gave no macOS arm64 runners (jobs were cancelled before they
started). The fork never changed LLVM's code — its branches only added workflow files —
and it stays, **archived**, because Fly 0.14.8 and the seeds up to 0.13.18 download
their LLVM from its releases.

## Releases

| Release | Asset | Platform |
|---|---|---|
| `v20.1.8-linux-x86_64` | `llvm-20.1.8-x86_64-linux-gnu.tar.gz` | Linux x86_64 |
| `v20.1.8-win-x64` | `llvm-20.1.8-win-x64.zip` | Windows x64 |
| `v20.1.8-macos-aarch64` | `llvm-20.1.8-aarch64-apple-darwin.tar.gz` | macOS arm64 (Apple Silicon), macOS 13+ |

The 20.1.8 Linux and Windows packages were built in the fork (same sources as the
upstream tag) and copied here **byte for byte**:
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
