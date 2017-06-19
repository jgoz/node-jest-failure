# jest test run exception

- Tested using [`node@d3c668ce`](https://github.com/nodejs/node/commit/d3c668cead3d5baff03d795755e2ae1906408580), compared against node 8.1.2
- Related issue: https://github.com/nodejs/node/issues/13804

## How to reproduce

1. Run `npm install`
2. Run the following:
    ```sh
    $ path/to/out/Debug/node ./node_modules/.bin/jest --runInBand
    ```
3. With node at the specified commit, note the `V8_Fatal` exception with the following stack trace:

    ```
    #
    # Fatal error in ../deps/v8/src/objects-inl.h, line 1955
    # Check failed: has_named_interceptor().
    #

    ==== C stack trace ===============================

        0   node                                0x0000000101c01c2e v8::base::debug::StackTrace::StackTrace() + 30
        1   node                                0x0000000101c01c65 v8::base::debug::StackTrace::StackTrace() + 21
        2   node                                0x0000000101a51627 v8::platform::(anonymous namespace)::PrintStackTrace() + 23
        3   node                                0x0000000101bfbdfd V8_Fatal + 445
        4   node                                0x00000001011c0a31 v8::internal::Map::GetNamedInterceptor() + 81
        5   node                                0x00000001011abfad v8::internal::JSObject::GetNamedInterceptor() + 29
        6   node                                0x00000001011be0dc v8::internal::__RT_impl_Runtime_LoadPropertyWithInterceptor(v8::internal::Arguments, v8::internal::Isolate*) + 716
        7   node                                0x00000001011bdcb0 v8::internal::Runtime_LoadPropertyWithInterceptor(int, v8::internal::Object**, v8::internal::Isolate*) + 272
        8   ???                                 0x00002c5ab4184264 0x0 + 48768080167524
    Received signal 4 <unknown> 000101c05971
    [1]    31361 illegal hardware instruction  ../node/out/Debug/node ./node_modules/.bin/jest
    ```

4. With node at 8.1.2, note the expected output:

    ```
    PASS  ./test.js
    test
        ✓ is a test (4ms)

    Test Suites: 1 passed, 1 total
    Tests:       1 passed, 1 total
    Snapshots:   0 total
    Time:        0.196s, estimated 1s
    Ran all test suites.
    ```
