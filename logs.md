## How to merge multiple log files:
https://stackoverflow.com/questions/15866772/merging-multiple-log-files-by-date-including-multilines

gist:

`sort -nbms -k1.1,1.2 -k1.4,1.5 -k1.7,1.8 -k1.10,1.12 myLog1.txt myLog2.txt > combined.txt`

flag definitions:

```
-n, --numeric-sort - compare according to string numerical value.
-b, --ignore-leading-blanks - ignore leading blanks.
-s, --stable - stabilize sort by disabling last-resort comparison
-m, --merge - merge already sorted files; do not sort
-k, --key=POS1[,POS2] - start a key at POS1 (origin 1), end it at POS2 (default end of line)
```

**Notes:**
- log files are already ordered so we don't need to sort them again, only determine which line goes where upon merging. That's why `-m`. It's crucial to keep stacktraces from getting scrambled.
- `-b` is not necessary in this case as somehow `-n` and `-m` combined keeps stacktrace lines from getting clustered. I left it just in case as most of stacktrace lines starts with blanks.
- `-n` apparently stops comparing key whenever there is a non-numeric character in the key. That's the second crucial bit for keeping stacktraces in place. Important is if it was `-n -k1,1` it would only sort the log files by hour as colon is non-numeric. Apart from that `-n` speeds up numeric comparison so we would like to have it anyway.
- the problem mentioned in the previous point is solved by pointing to specific characters positions in each key, that's why `-k1.1,1.2` (first and second digit of hour) `-k1.4,1.5` (first and second digit  of minutes) and so on. The first digit before the dot is always '1' as it points to the first column of the file line (which in our case is time). Shortly it's `-kA,B` where `A` and `B` are column positions in a given line (by default lines are delimited by blanks). Format of A and B used is <column-position>.<character-position-in-a-column>. Keep in mind that whenever there is a non-numeric character between `A` and `B` everything after it will be ignored in comparison if `-n` used.
- `-s` disables default behaviour which is: whenever keys by which comparison is being done are the same full string comparison of the lines is done. We don't want that to preserve original log entries order. Not sure if it's necessary with `-m` though.

## Set up logrotate

- get config parameters: `pm2 get pm2-logrotate`

Meris parameters: 
- `pm2 set pm2-logrotate:max_size 50M`
- `pm2 set pm2-logrotate:retain 30`
- `pm2 set pm2-logrotate:compress false`
- `pm2 set pm2-logrotate:dateFormat YYYY-MM-DD_HH-mm-ss`
- `pm2 set pm2-logrotate:workerInterval 30`
- `pm2 set pm2-logrotate:rotateInterval "0 0 0 * *"`
- `pm2 set pm2-logrotate:rotateModule true`
