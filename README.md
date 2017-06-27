ps_mem
======

A utility to accurately report the core memory usage for a program

This is a fork of [pixelb/ps_mem](https://github.com/pixelb/ps_mem).
Functionality currently remains largely the same.

In this fork, `argparse` has been used instead of `getopt` for easier
maintenance and expansion of command line arguments.

Usage:

```
ps_mem [-h|--help] [-p PID|--pid PID] [-s|--split-args] [-t|--total] [-w N]
       [-d|--discriminate-by-pid] [-S|--swap]

```

Optional arguments:
'''
-h, --help                      Show this help message and exit
-p <pid>, --pid <pid>           Show memory usage of specified PIDs
-s, --split-args                Show and separate by, all command line arguments
-t, --total                     Show only the total value
-d, --discriminate-by-pid       Show by process rather than by program
-S, --swap                      Show swap information
-w <N>, --watch <N>             Measure and show process memory every N seconds


Example output:

```
 Private  +   Shared  =  RAM used       Program

 34.6 MiB +   1.0 MiB =  35.7 MiB       gnome-terminal
139.8 MiB +   2.3 MiB = 142.1 MiB       firefox
291.8 MiB +   2.5 MiB = 294.3 MiB       gnome-shell
272.2 MiB +  43.9 MiB = 316.1 MiB       chrome (12)
913.9 MiB +   3.2 MiB = 917.1 MiB       thunderbird
---------------------------------
                          1.9 GiB
=================================
```

The `[-p PID|--pid PID]` option allows filtering the results. Note that
each `PID` needs its own `-p` flag, in contrast to providing a
comma-separated list. For example to restrict output to the current `$USER`
you could:

```
sudo ps_mem -p $(pgrep -d ' -p ' -u $USER)
```

or to summarize the total RAM usage per user you could:

```
for i in $(ps -e -o user= | sort | uniq); do
  printf '%-20s%10s\n' $i $(sudo ps_mem --total -p $(pgrep -d ' -p ' -u $i))
done
```

Todo:
* provide options for reverse sorting
* provide `-n` flag for filtering top `n` programs.
* provide ability to search by program name.
