About `zig cc`
--------------

Golang can use `zig cc` as it's C compiler. Usage for simple modules is
straightforward:

    CC="zig cc" go build <...>

To cross-compile a CGo binary to, say, linux/amd64:

    GOOS=linux GOOS=amd64 \
    CC="zig cc -target x86_64-linux-gnu.2.28" \
    CXX="zig c++ -target x86_64-linux-gnu.2.28" \
        go build <...>

The `gnu.2.28` is the glibc version that Zig should link against.

Running Go unit tests
---------------------

Go's unit tests pass when using `zig cc` as the C compiler. Requirements:
- Go master (at least go1.19beta1-3538-g10ad6c91de). Also the upcoming 1.21.
- Zig 0.11.0-dev.3191+fd213accb and Go dev versions (upcoming Go 1.21),
- `linux/amd64`. `linux/arm64` should work, but was not tested.

Prerequisites: */usr/local/bin/zig-cc*

    #!/bin/sh
    export ZIG_GLOBAL_CACHE_DIR=/tmp/zig-global-cache
    exec /usr/local/bin/zig cc -fno-sanitize=undefined "$@"

Now run the tests:

    $ GO_BUILDER_NAME=linux-amd64-zig-cc CC=zig-cc ./run.bash
