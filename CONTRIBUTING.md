Contributing to Flat-XML
------------------------

Bug reports and patches gladly accepted at
<https://github.com/danfuzz/flat-xml>.

Please be maximally specific with your bug reports. In particular,
please note what OS and version you are running. In addition, the
specific versions of the `bash` and `awk` tools may come in handy.

The gold standard for a bug report includes a pull request which
adds a test that demonstrates the problem. The platinum standard
includes a PR with both a test and a fix.

See below about testing. Please make sure that your fixes don't
break existing tests. If an existing test is *wrong*, please fix
the test!

Thanks!


Testing
-------

To run all the tests, issue the following command from your console
prompt:

```shell
./tests/run-all-tests
```

If you are working on a single test, use:

```shell
./tests/run-test <test-number>
```

For ease of debugging and new test development, the `run-test` command
accepts the options `--print` to print the actual output of the
test to the console, and `--update` to rewrite the expected output to
match the actual output.
