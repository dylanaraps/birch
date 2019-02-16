# birch

<a href="https://travis-ci.org/dylanaraps/birch"><img src="https://travis-ci.org/dylanaraps/birch.svg?branch=master"></a>

A [WIP] IRC client written in pure bash.

<img src="https://i.imgur.com/AbeKUg4.jpg" width="400px">

## Usage

```sh
usage: [birch -s host [-u nick -p pass -c channel]]
        -s server      Server to connect to (required).
        -u nick        Username to use.
        -p password    Password to use.
        -c channel     Channel to join.
```

## Implemented Commands

- `/join #channel`
- `/nick nick`
- `/msg [user,channel] etc etc etc`

## TODO

- [ ] Add more IRC commands.
- [x] Add TAB completion for channels.
- [ ] Handle a dropped connection.
- [ ] Add formatting
    - [x] `bold`
    - [ ] `italics`
    - [ ] `underline`
    - [ ] `colors`

## Dependencies

- `bash 4+`

## Installation

- Add `birch` to your path.

**Full Installation.**

1. Download `birch`.
    - Release: https://github.com/dylanaraps/birch/releases/latest
    - Git: `git clone https://github.com/dylanaraps/birch`
2. Change working directory to `birch`.
    - `cd birch`
3. Run `make install` inside the script directory to install the script.
    - **NOTE**: You may have to run this as root.

**NOTE:** `birch` can be uninstalled easily using `make uninstall`. This removes all of files from your system.

