# nfs-filewatch-multi-sync
A tool for watching for new files non-recursive in a list of folders a NFS and
sync them to multiple destinations. Inotify has limitations with NFS.

# Available switches

```
  --help|-h                           Print this help message.
  --dormant-threshold                 Time to consider file to be dormant aka
                                      not modified for x minute ago.
                                      Default 2 minutes
  --delete-source-file                Delete source file after complete.
                                      Default is 1
  --parallel-jobs                     Number of parallel jobs
  --watch-folder-fromfile             Path to file contains folders to watch
  --dest-folder-fromfile              Path to file of contains folders to output
  --ignore-hidden-and-partial-file    Ignore (.) hidden files (rsync partial)
                                      and .part file (ftp partial).
                                      Default is 1
  --polling-interval                  Interval between each poll.
                                      Default is 30 seconds
```
# Sample:
```
./nfs-filewatch-multi-sync.sh --dormant-threshold 2 --parallel-jobs 3 \
    --polling-interval 30 \
    --watch-folder-fromfile ./sample/watch_folder_fromfile.txt.sample \
    --dest-folder-fromfile ./sample/dest_folder_fromfile.txt.sample
```

Given the following contents:
```
# Watch folders file content
/tmp/watcher/folder_1
/tmp/watcher/folder_2

# Dest folders file content
/tmp/dest
/tmp/dest2
```

Any new files being added in `folder_1` and `folder_2` will be rsynced over
to `dest` and `dest2` if the file is not modified for 2 minutes. Say, if a
file name `kitten.txt` stayed dormant for 2 minutes in `/tmp/watcher/folder_1`,
after the job ran, the dest folders will contains `/tmp/dest/folder_1/kitten.txt`
and `/tmp/dest2/folder_1/kitten.txt`

# Dependencies:
* rsync
* parallel

# Licenses:
* MIT
