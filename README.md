For a project with workspace, Cargo returns issue relative to the workspace root
directory. `cargo.el` instead find the directory with `Cargo.toml`, which can be
different. When these two project root directory differ, Emacs compilation mode
fails to find the correct file path. To a user, Emacs is unable to jump to the
error location and a window prompt is open.

In this demo project, the file in `add-one/src/lib.rs` has a typo `x1`. When one
does a `cargo build`, we see the following compilation log:

```
-*- mode: cargo-process; default-directory: "~/repos/cargo.el-workspace/add-one/" -*-
Cargo-Process started at Mon Dec 25 15:50:13

cargo build
   Compiling add-one v0.1.0 (file:///Users/benzh/repos/cargo.el-workspace/add-one)
error[E0425]: cannot find value `x1` in this scope
 --> add-one/src/lib.rs:2:5
  |
2 |     x1 + 1
  |     ^^ did you mean `x`?

error: aborting due to previous error

error: Could not compile `add-one`.

To learn more, run the command again with --verbose.

Cargo-Process exited abnormally with code 101 at Mon Dec 25 15:50:15
```

And Emacs wants to open a non-existant file
`~/repos/cargo.el-workspace/add-one/add-one/src/lib.rs`. `cargo.el` finds file
relative to `~/repos/cargo.el-workspace/add-one/` while Cargo
`~/repos/cargo.el-workspace/`.
