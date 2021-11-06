# Kuiochai, a maybe too simple automatic file updater

## Introduction

Kuiochai is an as sturdy and simple as possible automatic file updater written
entirely in POSIX shell, with a big emphasis on portability.

It isn't meant to be a complete package manager, instead aiming for something
that will update single files with the least amount of work possible. As such,
it has been designed with an extremely simple, yet relatively modular design.
Its only hard dependency is a POSIX compliant system and cURL.

It tries to be reasonably modular by separating its version fetching logic into
*file sources*, simple executables which, given some arguments, spit out the
latest version and the download link for the given file.

Which arguments to pass to which source are determined by *file definitions*,
one-line files with a very simple syntax.

The files' name contain their own version using a very strict scheme.
Their version also escapes any special character to an underscore to avoid
separator collisions and file systeem incompatibilities.

Both file definitions and file sources directories can be specified with the
`KUIOCHAI_FILEDEF_PATH` and `KUIOCHAI_REPO_PATH` enviroment variables.

Kuiochai's under the Unlicense, meaning that it's fully into the public domain.
That means that you can do whatever you like for any purpose without any
attribution required. For more information read the `UNLICENSE` file.

## Quick start for the impatient

1. Find a repository for sources and file definitions.

2. Set KUIOCHAI_SOURCE_PATH and KUIOCHAI_FILEDEF_PATH to the sources and the
file definition directories respectively.

3. Find an interesting filedef name to download/update in the filedef repo.

3. `touch <filedef name>-0[.anyextension]`

4. `kuiochai <filedef name>-0[.anyextension]`

That's it.


## Update system

Kuiochai uses a very, *very* simple update system. It's divided in files, file
definitions and file sources.

Glossary:

 - File: the actual file that you're updating (plugins, libraries, mods,
whatever). Its name must be formatted as
`filedef-version_whatever[.extension]`, where `filedef` is the name that will
be used to look up the file definition. Dashes and periods must only be used to
separate versions and extensions, respectively. Any non-alphanumeric character
in the remote version will be automatically escaped to `_` in the final file name.

 - File definition (aka filedef): it's a very simple file which describes the
source being used and the arguments provided to it. Its syntax is simply
`<source name>+<arguments>`. Refer to the specific source's documentation for
more details on the arguments available.

 - File source (aka source): it's the executable that's responsible for
finding the latest version of a file and its download link by returning them
as: `<version> [URL]`. The URL is technically optional, although only a
simple warning will be thrown when missing.


## Enviroment variables

KUIOCHAI_SOURCE_PATH: A list of paths separated by `:`. Each of them will be
checked for a source when asked by a file definition in their original order.
To override a source, put its directory before the one you want to override.

KUIOCHAI_FILEDEF_PATH: Same thing as KUIOCHAI_SOURCE_PATH, just with file
definitions.

KUIOCHAI_DEBUG: When set to anything, kuiochai will print more verbose
messages. This is useful to know what's going on when something goes wrong.
