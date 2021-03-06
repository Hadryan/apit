# apit

[![Build Status](https://travis-ci.org/wschott/apit.svg?branch=master)](https://travis-ci.org/wschott/apit)

**RUN THIS AT YOUR OWN RISK! MAKE A BACKUP BEFORE RUNNING THIS!**

apit allows batch tagging .m4a (AAC and ALAC) file metadata tags using data from Apple Music/iTunes Store.


# Installation

## Requirements

1. at least [Python](https://www.python.org) 3.6
2. a [*new* version of AtomicParsley](https://bitbucket.org/wez/atomicparsley)

### AtomicParsley Installation

Execute the following in your terminal to compile the new version (tested using macOS Lion):

    $ cd atomicparsley-source-folder/
    $ make maintainer-clean
    $ ./configure --disable-universal
    $ make

Place the built `AtomicParsley` executable inside one of these folders:

- `$HOME/.bin/`
- `$HOME/bin/`
- `$HOME/Applications/`
- `/Applications/`

## apit Installation

    $ cd apit-source-code/  # i.e. this folder
    $ pip install .



# Usage

## Command line help

    $ apit -h

## Print the current metadata

    $ apit show ~/Music/Music/Media/Artist/Album/

## Tag the music files' metadata using data from Apple Music/iTunes Store

    $ apit tag ~/Music/Music/Media/Artist/Album/

### Filename format requirements

The filename of your files must have the following format in order to match them against the Apple Music/iTunes Store metadata:

1. **optional**: **disc number** (followed by "-")
2. **required**: **track number**
3. **required**: `.m4a` **extension**

Examples:
   - `14.m4a` (defaults to disc 1)
   - `14 song name.m4a`
   - `2-14 song name.m4a` (disc 2)

### Metadata source requirement

You must provide a source for the metadata to be used. Simply search for the album matching your files in the Apple Music/iTunes Store functionality or in a search engine and copy & paste the link to that album.
The format of that url **must match** the following form (as of 2020-05 using Apple Music on macOS 10.15 Catalina):

    https://music.apple.com/{COUNTRY_CODE}/album/album-name/{ID}
    e.g. https://music.apple.com/us/album/album-name/123456789

or the old style iTunes format:

    http://itunes.apple.com/{COUNTRY_CODE}/album/album-name/id{ID}
    e.g. http://itunes.apple.com/us/album/album-name/id123456789

Even this format will match:

    http://x/us/x/9/123456789?i=09876

This will lookup the metadata of the album with the ID _123456789_ in the _US_ store of Apple Music/iTunes (and optionally download that data to `~/.apit`).

### Attention: Beware of album variations (e.g. deluxe editions)

You should compare your files against the album's metadata you found via the iTunes Store or a search engine. Sometimes the songs' track order vary from album edition to album edition (e.g. deluxe edition) or the album published in another country has a different order. This means that your files will be tagged using the wrong metadata if you choose the wrong edition! You can overcome this by renaming your files before tagging to match against the appropriate metadata. Optionally, you can edit your files' track number metadata again after tagging to revert to your original track order.

## Provide a source using a command line option

You can provide the source using a command line option instead of entering/pasting a source while executing apit:

- either using an **url** (depending on your OS you maybe have to quote the url)
- or using an **already downloaded metadata file** (i.e. no further download will happen)

Examples using an url:

    $ apit tag ~/Music/Music/Media/Artist/Album/ https://music.apple.com/us/album/album-name/123456789

Examples using an existing metadata file:

    $ apit tag ~/Music/Music/Media/Artist/Album/ ~/.apit/Artist-Album-123456789.json

## Metadata cache

The downloaded metadata can be saved to your disk for later usage using `--cache` (short: `-c`).

    $ apit --cache tag ~/Music/Music/Media/Artist/Album/ https://music.apple.com/us/album/album-name/123456789

## Create temporary files instead of overwriting existing file

You can create temporary files with the updated metadata if you put `--temp` (short: `-t`) in your command. Without `--temp` your files will be overwritten!

    $ apit --temp tag ~/Music/Music/Media/Artist/Album/

## Verbose mode

To see more information what happens you can put `-v` into your command. Using `-vv` enables debug output.

    $ apit -v tag ~/Music/Music/Media/Artist/Album/


# apit Development

## Develop mode

Install apit in an editable mode:

    $ pip install -e ".[dev]"

This will install [tox](https://tox.readthedocs.io/) and other development tools.
Tox is used to run for example tests in an isolated environment. Show all possible actions using:

    $ tox -a

Running a specific tox command will install its necessary dependencies in separate virtualenvs (e.g. [coverage](https://coverage.readthedocs.io/), [isort](https://github.com/timothycrosley/isort), [mypy](http://mypy-lang.org), [flake8](https://flake8.pycqa.org/), [bandit](https://github.com/PyCQA/bandit), [black](https://github.com/psf/black))

## Building

    $ tox -e build
