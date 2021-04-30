# license-scanning
Source code license scanning

## Install scancode-toolkit

`scancode-toolkit` is not packaged in Fedora. The [latest upstream release (v21.3.31)](https://github.com/nexB/scancode-toolkit/releases/tag/v21.3.31) did not work for me (msrb) on F34, because there were some problems with Python dependencies.
Luckily, I was able to run the latest code from the `develop` branch without any problems.

```
$ git clone git@github.com:nexB/scancode-toolkit.git
$ cd scancode-toolkit/
$ # create virtual environment for scancode
$ virtualenv venv
$ # activate virtual environment in this terminal
$ source ./venv/bin/activate
$ # install scancode dependencies in the virtual environment
$ pip install -r requirements.txt
$ # run scancode in the virtual environment
$ ./scancode --help
```

## Basic Usage

There are files with various licenses inside [`samples/`](https://github.com/nexB/scancode-toolkit/tree/develop/samples) directory.
Let's try to scan the sample files there:

```
./scancode --license --package --html results.html --json-pp results.json samples/
```

Two files will be generated:

* [results.html](./data/results.html) (standard HTML)
* [results.json](./data/results.json) (pretty-printed JSON)

JSON contains more information than the HTML output, notably "score" (which I believe represents scancode's confidence in the result(?)).

`scancode` reports results in [SPDX](https://spdx.org/licenses/) format.
Fedora/RH (AFAIK) uses it's own format.

The results are reported per-file, but there are options to group results on codebase level (`--summary-with-details`).

`--package` option tells `scancode` to also process package manifest files, like Java/Maven's `pom.xml`.
Sometimes developers forget to add license files and license headers, and only specify license in the language-native package manifest file.
The full list of supported package types can be obtained by running:
```
./scancode --list-packages
```

`scancode` can also scan files for copyright information (`--copyright` option), but I haven't tested that.
