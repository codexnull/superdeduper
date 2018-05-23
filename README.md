# superdeduper

superdeduper is a utility to find duplicate or potentially duplicate files.

The program is implemented in a single file with (AFAIK) no dependencies other than python 2.7. There is no need to install or configure anything to use it.

### Usage

    superdeduper [opts] [directory]

### Options

The options below are supported by the program.

* `-h` `--help`

  Output a summary of usage and options and exit.

* `-v` `--version`

  Output the version number, currently v1.0.0, and exit.    

* `-sN` `--min-size=N`

  Specify minimum size of files (in MiBs) to check. The default min size is given by the `--help` option.

  The amount of space saved by finding duplicate smaller files may not be worth the additional work required to find them. Although, it *may* make sense to find and remove small duplicate files if looking to save inodes as well.

### Description

The program works by checking the directory given and recursing into its subdirectories. The duplicate detection algorithm is simple:

* Check the file size. Files with different sizes cannot be identical.
* If file size is the same, check the start of the file. Multiple files may have some standard size but radically different contents. Checking just the start is often enough to determine that files are NOT identical.
* If the size and start of the file is the same, check the entire file. This is a slow operation for large files so it is only done as a last resort.

File contents are compared using MD5 digests. The MD5 algorithm is no longer cryptographically secure but it is faster and more compact than those in the SHA family. If collision attacks are a concern, modify the program to use SHA-256 instead.

Duplicate files are reported as:

```
DUP: <path to duplicate file> (<size> MiB)
 --> <path to original file>
```

The program also looks for files that *may* contain duplicate data. For example, files that may just be compressed or encrypted versions of other files. These are identified by having file names that differ only by some known file extension.

Potentially duplicate files are reported as:

```
EXT: <path to file with known extension> (<size> MiB)
 --> <path to file without extension> (<size> MiB)
```

In no case are any files deleted. Cleanup of duplicate data must be done by the user, although an option to do this automatically may be added in the future.

### Disclaimer

Copyright (C) 2017 by Javier Alvarado.

This program was written for personal use by the author. Permission is hereby granted to study, use, copy, and/or modify for non-commercial use.

***THE SOFTWARE IS PROVIDED "AS IS" AND WITHOUT WARRANTY OF ANY KIND. THE AUTHOR IS IN NO WAY LIABLE FOR ANY DAMAGES OR LOSS OF DATA.***
