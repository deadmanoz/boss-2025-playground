# Bitcoin Core "Test the Test"
There was a pre-screening exercise that was part of the application process for the ₿OSS program. I won't specify exactly what we were required to do, but I'll note that it required building Bitcoin Core from source and then running the functional tests. Below are some of my notes about doing these things.

## Building Bitcoin Core
Please note that I've skipped the dependency installation steps in the following, the full details are available in the Bitcoin Core build docs, e.g. see [Bitcoin Core (macOS)](https://github.com/bitcoin/bitcoin/blob/master/doc/build-osx.md) for macOS instructions.

Assuming cloned repo and dependencies installed, configure the build. There's lots of options, which can be viewed by running `cmake -B build -LH`, I used the following to ensure wallet support (both BDB for legacy wallets and SQLite for modern wallets) and not have any GUI:
```
cmake -B build -DWITH_BDB=ON
```

Onto compilation with 8 jobs (took a few minutes on my machine):
```
cmake --build build -j 8
```

You can run the primary unit tests with (less than 1 minute):
```
ctest --test-dir build -j 8
```

We've now got `bitcoind` and `bitcoin-cli` available in the `build/src` directory.

## Running the functional tests
There are two ways to run a functional test:
1. Using the test runner script - `test_runner.py`
2. Running the test directly

### Using the test runner script

### Running the test directly

### Test output
As per the [Bitcoin Core test README (test logging)](https://github.com/bitcoin/bitcoin/blob/master/test/README.md#test-logging):
> _The tests contain logging at five different levels (DEBUG, INFO, WARNING, ERROR and CRITICAL). From within your functional tests you can log to these different levels using the logger included in the test_framework, e.g. self.log.debug(object). By default:_
> - _when run through the test_runner harness, all logs are written to test_framework.log and no logs are output to the console._
> - _when run directly, all logs are written to test_framework.log and INFO level and above are output to the console._

### Adding a new functional test
If you want to add a new functional test to `test/functional`, `new_test.py`, and then have the test available for running you need to do a few things:
- Ensure that the file has executable permissions - `chmod +x new_test.py`. 
- Add the test to the `src/test/functional/test_runner.py` - find somewhere appropriate to insert `'new_test.py',` in the list of test files
- Run the CMake **configuration** phase as this is where the test directory structure under `build` is created and populated - e.g. `cmake -B build -DWITH_BDB=ON`

### Killing zombie bitcoind processes after test failure
Sometimes you'll see a warning when running a test:
```
WARNING! There is already a bitcoind process running on this system. Tests may fail unexpectedly due to resource contention!
```

You've likely got a zombie `bitcoind` process running (or a legitimate `bitcoind` process that you don't want to kill, be sure as the following will kill all running `bitcoind` processes..).
You can kill it with:
```
pkill -9 bitcoind
```
Further details: [Troubleshooting and debugging test failures - resource contention](https://github.com/bitcoin/bitcoin/blob/master/test/README.md#resource-contention)

