# bingo
Bingo - The missing package manager for "go get"

Bingo is a wrapper for the go toolchain to make installing and managing golang-compiled binaries a bit easier.

#### Relating A Golang Binary To Its Package

One pain point when using `go get` to install binaries is that, once the binary is installed, there's no means of relating that binary to its original source package.

Bingo tries to solve that problem by maintaining a link between an installed binary and its package.

#### A Toy Trying To Take Itself Seriously

Although designed as a set of toy scripts to play around with the idea, bingo is trying to take itself seriously and make a real run at being a useful tool.

So please give it a try and feel free to file issues or requests for feature enhancements !

#### TOC

- [Using](#using)
- [Installing](#installing)
- [Work Folders](#work-folders)a
- [Contributing](#contributing)
- [Contact](#contact)
- [License](#license)

--------
## Using

 - [Compiling + Installing Binaries](#compiling--installing-binaries)
   - [Compiling 'Complicated' Packages](#compiling-complicated-packages)
   - [Binary Naming](#binary-naming)
 - [Listing Installed Binaries](#listing-installed-binaries)
 - [Displaying A Binary's Associated Package](#displaying-a-binarys-associated-package)
 - [Uninstalling Binaries / Packages](#uninstalling-binaries--packages)
 - [Upgrading Binaries](#upgrading-binaries)

------------------------------------
#### Compiling + Installing Binaries

To install a binary with bingo, use the golang application's full package path, same as you would with `"go get"`.

_hello example_
```
$ bingo install github.com/golang/example/hello

Installing binary hello from package github.com/golang/example/hello
Downloading package (folder: '~/.bingo/pkg/hello')
Compiling package
Installing binary (file: '~/.bingo/bin/hello')
Done

$ ~/.bingo/bin/hello

Hello, Go examples!
```

##### Compiling 'Complicated' Packages
*TBD*

Currently, bingo only supports packages which can be directly compiled via `go get`.

Packages that require a more complex build process are not supported at this time.

##### Installation Folder

See the [Work Folders](#work-folders) section for details on configuring the various folders needed by bingo, including which folder to install binaries in.

##### Binary Naming

By default, the installed binary will be named after the last folder element in its package path.

As you saw above, installing the `github.com/golang/example/hello` package installed a binary named `hello`.

You can override this behavior and specify the binary name at the time of installation:

_install hello example as 'foo'_
```
$ bingo install -n foo -q github.com/golang/example/hello

$ ~/.bingo/bin/foo

Hello, Go examples!
```

#### Listing Installed Binaries

To see a list of installed binaries, use the `installed` command:

```
$ bingo installed -p

Bingo-managed binaries (folder: '~/.bingo/bin')

 - foo github.com/golang/example/hello
 - hello github.com/golang/example/hello
```

##### Per-Binary Isolation

As you see above, installing the hello example as `foo` did not interfere with the example installed as `hello`.

Each installed binary is managed in a separate `$GOROOT`, even if it is has the same package path as another binary.

#### Displaying A Binary's Associated Package

If you need a reminder of which package a binary was compiled/installed from, you can use the `package` command:

```
$ bingo package hello

github.com/golang/example/hello
```

#### Uninstalling Binaries / Packages

Use the `uninstall` command to uninstall binaries:

```
$ bingo uninstall foo

Uninstalling binary foo from package github.com/golang/example/hello
Removing binary (file: '~/.bingo/bin/foo')
Removing package (folder: '~/.bingo/pkg/foo')
Done

$ bingo installed -q

hello
```

*NOTE*: Uninstalling a binary also removes the associated package folder.

#### Upgrading Binaries
*TBD*

Bingo doesn't yet have an `upgrade` command.

For now, you can uninstall, then re-install, a binary to get the latest version.

## Requirements

#### Run

Bingo exists as a Runfile, and requires the Run tool to operate:

* https://github.com/TekWizely/run

#### Bash

Bingo (currently) uses `bash` for its command scripts.

#### Readlink

Bingo uses symbolic links to associate binaries to their packages.

The scripts use `readlink` to resolve symbolic links.

-------------
## Installing

### Releases

See the [Releases](https://github.com/TekWizely/bingo/releases) page for downloadable archives of versioned releases.

#### Brew Core
TBD

#### Brew Tap

In addition to working on brew core support, I have also created a tap to ensure the latest version is always available:

* https://github.com/TekWizely/homebrew-tap

_install bingo directly from tap_
```
$ brew install tekwizely/tap/bingo
```

_install tap to track updates_
```
$ brew tap tekwizely/tap

$ brew install bingo
```

---------------
## Work Folders
Bingo requires the following work folders:

| Folder         | Description
|----------------|------------
| Bin Folder     | Where compiled binaries are installed.  This folder should be in your `$PATH`.  This _can_ be a "shared" folder that also contains non-bingo binaries.
| Package Folder | Where packages are downloaded / compiled.  This should NOT be a "shared" folder.
| Cache Folder   | Where to store Go cache files.  This _might_ work as a shared folder but has not been tested.

### Configuring Work Folders via Shell Variables

#### Individual

Each of these folders can be individually configured via shell variables:

* `$BINGO_BIN`
* `$BINGO_PKG`
* `$BINGO_CACHE`

#### Fallback: `$BINGO_HOME`

If the `$BINGO_HOME` variable is defined, bingo will use it as a fall-back for any folder that is not individually configured, i.e:

* `$BINGO_HOME/bin`
* `$BINGO_HOME/pkg`
* `$BINGO_HOME/cache`

#### Default: `$HOME/.bingo`

As a final default, bingo will use $HOME/.bingo i.e:

* `$HOME/.bingo/bin`
* `$HOME/.bingo/pkg`
* `$HOME/.bingo/cache`

---------------
## Contributing

To contribute to Run, follow these steps:

1. Fork this repository.
2. Create a branch: `git checkout -b <branch_name>`.
3. Make your changes and commit them: `git commit -m '<commit_message>'`
4. Push to the original branch: `git push origin <project_name>/<location>`
5. Create the pull request.

Alternatively see the GitHub documentation on [creating a pull request](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request).

----------
## Contact

If you want to contact me you can reach me at TekWize.ly@gmail.com.

----------
## License

The `tekwizely/bingo` project is released under the [MIT](https://opensource.org/licenses/MIT) License.  See `LICENSE` file.
