# ZSTD Wasm Bug example

Example for `zstd-rs` issue to show that building for WASM fails with released `0.11.0`, though it works for the same revision in Git.

## Works for Git release

Since I'm on Mac, I need to point to Clang built with wasm support (see https://github.com/gyscos/zstd-rs/wiki/Compile-for-WASM)

```bash
export PATH="/usr/local/opt/llvm/bin/:$PATH"
export CC=/usr/local/opt/llvm/bin/clang
export AR=/usr/local/opt/llvm/bin/llvm-ar
```

Note that this `Cargo.toml` is pointing to the same `0.11` release git SHA: https://github.com/gyscos/zstd-rs/commit/3a6c7fa76b49928bd4afc6a68ae82fce842df8dd

```
> cd git && cargo build --target wasm32-unknown-unknown
    Finished dev [unoptimized + debuginfo] target(s) in 0.14s
```

## Doesn't work for released `0.11`

```
> cd released && cargo build --target wasm32-unknown-unknown
   Compiling zstd-sys v2.0.0+zstd.1.5.2
The following warnings were emitted during compilation:

warning: In file included from zstd/lib/common/pool.c:13:
warning: zstd/lib/common/zstd_deps.h:29:10: fatal error: 'string.h' file not found
warning: #include <string.h>
warning:          ^~~~~~~~~~
warning: In file included from zstd/lib/common/zstd_common.c:17:
warning: zstd/lib/common/zstd_deps.h:29:10: fatal error: 'string.h' file not found
warning: #include <string.h>
warning:          ^~~~~~~~~~
warning: In file included from zstd/lib/common/fse_decompress.c:20:
warning: In file included from zstd/lib/common/bitstream.h:29:
warning: In file included from zstd/lib/common/mem.h:24:
warning: zstd/lib/common/zstd_deps.h:29:10: fatal error: 'string.h' file not found
warning: #include <string.h>
warning:          ^~~~~~~~~~
warning: In file included from zstd/lib/common/entropy_common.c:18:
warning: In file included from zstd/lib/common/mem.h:24:
warning: zstd/lib/common/zstd_deps.h:29:10: fatal error: 'string.h' file not found
warning: #include <string.h>
warning:          ^~~~~~~~~~
warning: In file included from zstd/lib/common/error_private.c:13:
warning: In file included from zstd/lib/common/error_private.h:27:
warning: zstd/lib/common/zstd_deps.h:29:10: fatal error: 'string.h' file not found
warning: #include <string.h>
warning:          ^~~~~~~~~~
warning: In file included from zstd/lib/compress/zstd_compress_superblock.c:16:
warning: In file included from zstd/lib/compress/../common/zstd_internal.h:23:
warning: In file included from zstd/lib/common/cpu.h:19:
warning: In file included from zstd/lib/common/mem.h:24:
warning: zstd/lib/common/zstd_deps.h:29:10: fatal error: 'string.h' file not found
warning: #include <string.h>
warning:          ^~~~~~~~~~
warning: 1 error generated.
warning: 1 error generated.
warning: 1 error generated.
warning: 1 error generated.
warning: 1 error generated.
warning: In file included from zstd/lib/compress/zstdmt_compress.c:23:
warning: zstd/lib/compress/../common/zstd_deps.h:29:10: fatal error: 'string.h' file not found
warning: #include <string.h>
warning:          ^~~~~~~~~~
warning: In file included from zstd/lib/compress/zstd_double_fast.c:11:
warning: In file included from zstd/lib/compress/zstd_compress_internal.h:21:
warning: In file included from zstd/lib/compress/../common/zstd_internal.h:23:
warning: In file included from zstd/lib/common/cpu.h:19:
warning: In file included from zstd/lib/common/mem.h:24:
warning: zstd/lib/common/zstd_deps.h:29:10: fatal error: 'string.h' file not found
warning: #include <string.h>
warning:          ^~~~~~~~~~
warning: In file included from zstd/lib/compress/zstd_fast.c:11:
warning: In file included from zstd/lib/compress/zstd_compress_internal.h:21:
warning: In file included from zstd/lib/compress/../common/zstd_internal.h:23:
warning: In file included from zstd/lib/common/cpu.h:19:
warning: In file included from zstd/lib/common/mem.h:24:
warning: zstd/lib/common/zstd_deps.h:29:10: fatal error: 'string.h' file not found
warning: #include <string.h>
warning:          ^~~~~~~~~~
warning: 1 error generated.
warning: 1 error generated.
warning: 1 error generated.
warning: 1 error generated.

error: failed to run custom build command for `zstd-sys v2.0.0+zstd.1.5.2`

Caused by:
  process didn't exit successfully: `/Users/kbarron/github/rust/zstd-rs-wasm-bug-example/released/target/debug/build/zstd-sys-d8fdf8d7f8ca86e4/build-script-build` (exit status: 1)
  --- stdout
  cargo:rustc-cfg=feature="std"
  cargo:rerun-if-changed=wasm-shim/stdlib.h
  cargo:rerun-if-changed=wasm-shim/string.h
  TARGET = Some("wasm32-unknown-unknown")
  HOST = Some("x86_64-apple-darwin")
  CC_wasm32-unknown-unknown = None
  CC_wasm32_unknown_unknown = None
  TARGET_CC = None
  CC = Some("/usr/local/opt/llvm/bin/clang")
  CFLAGS_wasm32-unknown-unknown = None
  CFLAGS_wasm32_unknown_unknown = None
  TARGET_CFLAGS = None
  CFLAGS = None
  CRATE_CC_NO_DEFAULTS = None
  DEBUG = Some("true")
  running: "/usr/local/opt/llvm/bin/clang" "-O3" "-ffunction-sections" "-fdata-sections" "-fPIC" "-g" "-fno-omit-frame-pointer" "--target=wasm32-unknown-unknown" "-I" "wasm-shim/" "-I" "zstd/lib/" "-I" "zstd/lib/common" "-fvisibility=hidden" "-DXXH_STATIC_ASSERT=0" "-DZSTD_LIB_DEPRECATED=0" "-DXXH_PRIVATE_API=" "-DZSTDLIB_VISIBILITY=" "-DZSTDERRORLIB_VISIBILITY=" "-o" "/Users/kbarron/github/rust/zstd-rs-wasm-bug-example/released/target/wasm32-unknown-unknown/debug/build/zstd-sys-fa1903bcd47c13dd/out/zstd/lib/common/pool.o" "-c" "zstd/lib/common/pool.c"
  running: "/usr/local/opt/llvm/bin/clang" "-O3" "-ffunction-sections" "-fdata-sections" "-fPIC" "-g" "-fno-omit-frame-pointer" "--target=wasm32-unknown-unknown" "-I" "wasm-shim/" "-I" "zstd/lib/" "-I" "zstd/lib/common" "-fvisibility=hidden" "-DXXH_STATIC_ASSERT=0" "-DZSTD_LIB_DEPRECATED=0" "-DXXH_PRIVATE_API=" "-DZSTDLIB_VISIBILITY=" "-DZSTDERRORLIB_VISIBILITY=" "-o" "/Users/kbarron/github/rust/zstd-rs-wasm-bug-example/released/target/wasm32-unknown-unknown/debug/build/zstd-sys-fa1903bcd47c13dd/out/zstd/lib/common/debug.o" "-c" "zstd/lib/common/debug.c"
  running: "/usr/local/opt/llvm/bin/clang" "-O3" "-ffunction-sections" "-fdata-sections" "-fPIC" "-g" "-fno-omit-frame-pointer" "--target=wasm32-unknown-unknown" "-I" "wasm-shim/" "-I" "zstd/lib/" "-I" "zstd/lib/common" "-fvisibility=hidden" "-DXXH_STATIC_ASSERT=0" "-DZSTD_LIB_DEPRECATED=0" "-DXXH_PRIVATE_API=" "-DZSTDLIB_VISIBILITY=" "-DZSTDERRORLIB_VISIBILITY=" "-o" "/Users/kbarron/github/rust/zstd-rs-wasm-bug-example/released/target/wasm32-unknown-unknown/debug/build/zstd-sys-fa1903bcd47c13dd/out/zstd/lib/common/error_private.o" "-c" "zstd/lib/common/error_private.c"
  running: "/usr/local/opt/llvm/bin/clang" "-O3" "-ffunction-sections" "-fdata-sections" "-fPIC" "-g" "-fno-omit-frame-pointer" "--target=wasm32-unknown-unknown" "-I" "wasm-shim/" "-I" "zstd/lib/" "-I" "zstd/lib/common" "-fvisibility=hidden" "-DXXH_STATIC_ASSERT=0" "-DZSTD_LIB_DEPRECATED=0" "-DXXH_PRIVATE_API=" "-DZSTDLIB_VISIBILITY=" "-DZSTDERRORLIB_VISIBILITY=" "-o" "/Users/kbarron/github/rust/zstd-rs-wasm-bug-example/released/target/wasm32-unknown-unknown/debug/build/zstd-sys-fa1903bcd47c13dd/out/zstd/lib/compress/zstd_compress_superblock.o" "-c" "zstd/lib/compress/zstd_compress_superblock.c"
  running: "/usr/local/opt/llvm/bin/clang" "-O3" "-ffunction-sections" "-fdata-sections" "-fPIC" "-g" "-fno-omit-frame-pointer" "--target=wasm32-unknown-unknown" "-I" "wasm-shim/" "-I" "zstd/lib/" "-I" "zstd/lib/common" "-fvisibility=hidden" "-DXXH_STATIC_ASSERT=0" "-DZSTD_LIB_DEPRECATED=0" "-DXXH_PRIVATE_API=" "-DZSTDLIB_VISIBILITY=" "-DZSTDERRORLIB_VISIBILITY=" "-o" "/Users/kbarron/github/rust/zstd-rs-wasm-bug-example/released/target/wasm32-unknown-unknown/debug/build/zstd-sys-fa1903bcd47c13dd/out/zstd/lib/common/zstd_common.o" "-c" "zstd/lib/common/zstd_common.c"
  running: "/usr/local/opt/llvm/bin/clang" "-O3" "-ffunction-sections" "-fdata-sections" "-fPIC" "-g" "-fno-omit-frame-pointer" "--target=wasm32-unknown-unknown" "-I" "wasm-shim/" "-I" "zstd/lib/" "-I" "zstd/lib/common" "-fvisibility=hidden" "-DXXH_STATIC_ASSERT=0" "-DZSTD_LIB_DEPRECATED=0" "-DXXH_PRIVATE_API=" "-DZSTDLIB_VISIBILITY=" "-DZSTDERRORLIB_VISIBILITY=" "-o" "/Users/kbarron/github/rust/zstd-rs-wasm-bug-example/released/target/wasm32-unknown-unknown/debug/build/zstd-sys-fa1903bcd47c13dd/out/zstd/lib/common/fse_decompress.o" "-c" "zstd/lib/common/fse_decompress.c"
  running: "/usr/local/opt/llvm/bin/clang" "-O3" "-ffunction-sections" "-fdata-sections" "-fPIC" "-g" "-fno-omit-frame-pointer" "--target=wasm32-unknown-unknown" "-I" "wasm-shim/" "-I" "zstd/lib/" "-I" "zstd/lib/common" "-fvisibility=hidden" "-DXXH_STATIC_ASSERT=0" "-DZSTD_LIB_DEPRECATED=0" "-DXXH_PRIVATE_API=" "-DZSTDLIB_VISIBILITY=" "-DZSTDERRORLIB_VISIBILITY=" "-o" "/Users/kbarron/github/rust/zstd-rs-wasm-bug-example/released/target/wasm32-unknown-unknown/debug/build/zstd-sys-fa1903bcd47c13dd/out/zstd/lib/common/entropy_common.o" "-c" "zstd/lib/common/entropy_common.c"
  running: "/usr/local/opt/llvm/bin/clang" "-O3" "-ffunction-sections" "-fdata-sections" "-fPIC" "-g" "-fno-omit-frame-pointer" "--target=wasm32-unknown-unknown" "-I" "wasm-shim/" "-I" "zstd/lib/" "-I" "zstd/lib/common" "-fvisibility=hidden" "-DXXH_STATIC_ASSERT=0" "-DZSTD_LIB_DEPRECATED=0" "-DXXH_PRIVATE_API=" "-DZSTDLIB_VISIBILITY=" "-DZSTDERRORLIB_VISIBILITY=" "-o" "/Users/kbarron/github/rust/zstd-rs-wasm-bug-example/released/target/wasm32-unknown-unknown/debug/build/zstd-sys-fa1903bcd47c13dd/out/zstd/lib/common/threading.o" "-c" "zstd/lib/common/threading.c"
  cargo:warning=In file included from zstd/lib/common/pool.c:13:
  cargo:warning=zstd/lib/common/zstd_deps.h:29:10: fatal error: 'string.h' file not found
  cargo:warning=#include <string.h>
  cargo:warning=         ^~~~~~~~~~
  cargo:warning=In file included from zstd/lib/common/zstd_common.c:17:
  cargo:warning=zstd/lib/common/zstd_deps.h:29:10: fatal error: 'string.h' file not found
  cargo:warning=#include <string.h>
  cargo:warning=         ^~~~~~~~~~
  cargo:warning=In file included from zstd/lib/common/fse_decompress.c:20:
  cargo:warning=In file included from zstd/lib/common/bitstream.h:29:
  cargo:warning=In file included from zstd/lib/common/mem.h:24:
  cargo:warning=zstd/lib/common/zstd_deps.h:29:10: fatal error: 'string.h' file not found
  cargo:warning=#include <string.h>
  cargo:warning=         ^~~~~~~~~~
  cargo:warning=In file included from zstd/lib/common/entropy_common.c:18:
  cargo:warning=In file included from zstd/lib/common/mem.h:24:
  cargo:warning=zstd/lib/common/zstd_deps.h:29:10: fatal error: 'string.h' file not found
  cargo:warning=#include <string.h>
  cargo:warning=         ^~~~~~~~~~
  cargo:warning=In file included from zstd/lib/common/error_private.c:13:
  cargo:warning=In file included from zstd/lib/common/error_private.h:27:
  cargo:warning=zstd/lib/common/zstd_deps.h:29:10: fatal error: 'string.h' file not found
  cargo:warning=#include <string.h>
  cargo:warning=         ^~~~~~~~~~
  cargo:warning=In file included from zstd/lib/compress/zstd_compress_superblock.c:16:
  cargo:warning=In file included from zstd/lib/compress/../common/zstd_internal.h:23:
  cargo:warning=In file included from zstd/lib/common/cpu.h:19:
  cargo:warning=In file included from zstd/lib/common/mem.h:24:
  cargo:warning=zstd/lib/common/zstd_deps.h:29:10: fatal error: 'string.h' file not found
  cargo:warning=#include <string.h>
  cargo:warning=         ^~~~~~~~~~
  cargo:warning=1 error generated.
  exit status: 0
  running: "/usr/local/opt/llvm/bin/clang" "-O3" "-ffunction-sections" "-fdata-sections" "-fPIC" "-g" "-fno-omit-frame-pointer" "--target=wasm32-unknown-unknown" "-I" "wasm-shim/" "-I" "zstd/lib/" "-I" "zstd/lib/common" "-fvisibility=hidden" "-DXXH_STATIC_ASSERT=0" "-DZSTD_LIB_DEPRECATED=0" "-DXXH_PRIVATE_API=" "-DZSTDLIB_VISIBILITY=" "-DZSTDERRORLIB_VISIBILITY=" "-o" "/Users/kbarron/github/rust/zstd-rs-wasm-bug-example/released/target/wasm32-unknown-unknown/debug/build/zstd-sys-fa1903bcd47c13dd/out/zstd/lib/compress/zstdmt_compress.o" "-c" "zstd/lib/compress/zstdmt_compress.c"
  exit status: 0
  running: "/usr/local/opt/llvm/bin/clang" "-O3" "-ffunction-sections" "-fdata-sections" "-fPIC" "-g" "-fno-omit-frame-pointer" "--target=wasm32-unknown-unknown" "-I" "wasm-shim/" "-I" "zstd/lib/" "-I" "zstd/lib/common" "-fvisibility=hidden" "-DXXH_STATIC_ASSERT=0" "-DZSTD_LIB_DEPRECATED=0" "-DXXH_PRIVATE_API=" "-DZSTDLIB_VISIBILITY=" "-DZSTDERRORLIB_VISIBILITY=" "-o" "/Users/kbarron/github/rust/zstd-rs-wasm-bug-example/released/target/wasm32-unknown-unknown/debug/build/zstd-sys-fa1903bcd47c13dd/out/zstd/lib/compress/zstd_double_fast.o" "-c" "zstd/lib/compress/zstd_double_fast.c"
  exit status: 1
  running: "/usr/local/opt/llvm/bin/clang" "-O3" "-ffunction-sections" "-fdata-sections" "-fPIC" "-g" "-fno-omit-frame-pointer" "--target=wasm32-unknown-unknown" "-I" "wasm-shim/" "-I" "zstd/lib/" "-I" "zstd/lib/common" "-fvisibility=hidden" "-DXXH_STATIC_ASSERT=0" "-DZSTD_LIB_DEPRECATED=0" "-DXXH_PRIVATE_API=" "-DZSTDLIB_VISIBILITY=" "-DZSTDERRORLIB_VISIBILITY=" "-o" "/Users/kbarron/github/rust/zstd-rs-wasm-bug-example/released/target/wasm32-unknown-unknown/debug/build/zstd-sys-fa1903bcd47c13dd/out/zstd/lib/compress/zstd_fast.o" "-c" "zstd/lib/compress/zstd_fast.c"
  cargo:warning=1 error generated.
  cargo:warning=1 error generated.
  exit status: 1
  exit status: 1
  cargo:warning=1 error generated.
  cargo:warning=1 error generated.
  exit status: 1
  exit status: 1
  cargo:warning=In file included from zstd/lib/compress/zstdmt_compress.c:23:
  cargo:warning=zstd/lib/compress/../common/zstd_deps.h:29:10: fatal error: 'string.h' file not found
  cargo:warning=#include <string.h>
  cargo:warning=         ^~~~~~~~~~
  cargo:warning=In file included from zstd/lib/compress/zstd_double_fast.c:11:
  cargo:warning=In file included from zstd/lib/compress/zstd_compress_internal.h:21:
  cargo:warning=In file included from zstd/lib/compress/../common/zstd_internal.h:23:
  cargo:warning=In file included from zstd/lib/common/cpu.h:19:
  cargo:warning=In file included from zstd/lib/common/mem.h:24:
  cargo:warning=zstd/lib/common/zstd_deps.h:29:10: fatal error: 'string.h' file not found
  cargo:warning=#include <string.h>
  cargo:warning=         ^~~~~~~~~~
  cargo:warning=In file included from zstd/lib/compress/zstd_fast.c:11:
  cargo:warning=In file included from zstd/lib/compress/zstd_compress_internal.h:21:
  cargo:warning=In file included from zstd/lib/compress/../common/zstd_internal.h:23:
  cargo:warning=In file included from zstd/lib/common/cpu.h:19:
  cargo:warning=In file included from zstd/lib/common/mem.h:24:
  cargo:warning=zstd/lib/common/zstd_deps.h:29:10: fatal error: 'string.h' file not found
  cargo:warning=#include <string.h>
  cargo:warning=         ^~~~~~~~~~
  cargo:warning=1 error generated.
  exit status: 1
  cargo:warning=1 error generated.
  cargo:warning=1 error generated.
  exit status: 1
  exit status: 1
  cargo:warning=1 error generated.
  exit status: 1

  --- stderr


  error occurred: Command "/usr/local/opt/llvm/bin/clang" "-O3" "-ffunction-sections" "-fdata-sections" "-fPIC" "-g" "-fno-omit-frame-pointer" "--target=wasm32-unknown-unknown" "-I" "wasm-shim/" "-I" "zstd/lib/" "-I" "zstd/lib/common" "-fvisibility=hidden" "-DXXH_STATIC_ASSERT=0" "-DZSTD_LIB_DEPRECATED=0" "-DXXH_PRIVATE_API=" "-DZSTDLIB_VISIBILITY=" "-DZSTDERRORLIB_VISIBILITY=" "-o" "/Users/kbarron/github/rust/zstd-rs-wasm-bug-example/released/target/wasm32-unknown-unknown/debug/build/zstd-sys-fa1903bcd47c13dd/out/zstd/lib/common/entropy_common.o" "-c" "zstd/lib/common/entropy_common.c" with args "clang" did not execute successfully (status code exit status: 1).
```
