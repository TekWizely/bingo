# Bingo: The missing package manager for golang binaries<br/>(its *homebrew* for "go install")
![GitHub repo size](https://img.shields.io/github/repo-size/TekWizely/bingo)<!-- ALL-CONTRIBUTORS-BADGE:START - Do not remove or modify this section -->
[![All Contributors](https://img.shields.io/badge/all_contributors-0-orange.svg?style=flat-square)](#contributors-)<!-- ALL-CONTRIBUTORS-BADGE:END -->
![GitHub stars](https://img.shields.io/github/stars/TekWizely/bingo?style=social)
![GitHub forks](https://img.shields.io/github/forks/TekWizely/bingo?style=social)
![Twitter Follow](https://img.shields.io/twitter/follow/TekWizely?style=social)

Do you love the simplicity of being able to download & compile golang applications with `go install`, but wish it were easier to manage the compiled binaries?

Bingo makes installing and managing `go install`\-based packages a lot easier.

#### Features

* Keeps a link between the installed binary and the source package
* Can update / uninstall binaries using the name of the binary (i.e `bingo update goimports`)
* Can install binaries to a location of your choice
* Can control the name of the installed binary
* Can install multiple versions of the same package (using different names for the binaries)
* Each binary's source packages are isolated and managed in their own separate `$GOROOT`

#### TOC

- [Using](#using)
- [Installing](#installing)
- [Work Folders](#work-folders)
- [Contributing](#contributing)
- [Contact](#contact)
- [License](#license)

--------
## Using

 - [Compiling + Installing Binaries](#compiling--installing-binaries)
   - [Compiling 'Complicated' Packages](#compiling-complicated-packages)
   - [Specifying Package Version](#specifying-package-version)
   - [Binary Naming](#binary-naming)
   - [Using Go Get](#using-go-get)
 - [Listing Installed Binaries](#listing-installed-binaries)
 - [Displaying A Binary's Associated Package](#displaying-a-binarys-associated-package)
 - [Updating Binaries](#updating-binaries)
 - [Uninstalling Binaries / Packages](#uninstalling-binaries--packages)

------------------------------------
#### Compiling + Installing Binaries

To install a binary with bingo, use the golang application's full package path, same as you would with `"go install"`.

_hello example_
```
$ bingo install github.com/golang/example/hello

Installing binary hello from package github.com/golang/example/hello
Downloading & compiling package (folder: '~/.bingo/pkg/hello')
go: downloading github.com/golang/example v0.0.0-20170904185048-46695d81d1fa
go: found github.com/golang/example/hello in github.com/golang/example v0.0.0-20170904185048-46695d81d1fa
Installing binary (file: '~/.bingo/bin/hello')
Done

$ ~/.bingo/bin/hello

Hello, Go examples!
```

##### Compiling 'Complicated' Packages
*TBD*

Currently, bingo only supports packages which can be directly downloaded + compiled + installed via `go install` (or [go get](#using-go-get) for pre v1.16).

Packages that require a more complex build process are not supported at this time.

##### Installation Folder

See the [Work Folders](#work-folders) section for details on configuring the various folders needed by bingo, including which folder to install binaries in.

##### Specifying Package Version

You can specify which version of a package to install.

###### '@<!-- -->version' Syntax

_install example using @version syntax_
```
$ bingo install github.com/golang/example/hello@v1.2.3
```

###### '--version' Option

_install example using --version option_
```
$ bingo install --version v1.2.3 github.com/golang/example/hello
```

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

##### Using Go Get

By default, bingo uses `go install` to download + compile + install packages.

If you're using a version of go prior to v1.16, you can instruct bingo to use `go get`:

_install example using --useget option_
```
$ bingo install --useget github.com/golang/example/hello
```

_install example using BINGO_USE_GET variable_
```
$ BINGO_USE_GET=1 bingo install github.com/golang/example/hello
```

**NOTE:** Both of these work for the `bingo update` command as well.

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

#### Updating Binaries

To update an installed binary, use the `update` command:

```
$ bingo update hello

Updating hello package github.com/golang/example/hello
go: found github.com/golang/example/hello in github.com/golang/example v0.0.0-20170904185048-46695d81d1fa
Done
```

*NOTE*: By default, the resulting package version will be determined by Go's version resolution rules.

##### Specifying Package Version

You can specify which version of a package to update to.

*NOTE*: The target version does not have to be newer than the existing version.  i.e you can update to an older version of the package.

###### '@<!-- -->version' Syntax

_update example using @version syntax_
```
$ bingo update hello@v1.2.3
```

###### '--version' Option

_update example using --version option_
```
$ bingo update --version v1.2.3 hello
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

---------------
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

To contribute to Bingo, follow these steps:

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
