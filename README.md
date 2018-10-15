# sortar
A script to create an efficiently compressible .tar using a simple heuristic.
Best use case is probably a directory of source code.

This is a proof of concept and many things can be improved, but it works.
The constants were chosen absolutely unscientifically.

### Dependencies

* Python 3
* GNU find
* GNU tar

### Usage

For example, with zstd:
```
EXCLUDE_PATTERN="/\.git/" sortar <source_dir> | zstd -zc3 -o target.tar.zst
```

### Rationale

The assumptions about the compression:

1. Binary files compress poorly, and they should be in the end of the tar file.
2. Text files are more likely to be small than binary files.
3. Similar files should be close together in the tar file.
4. The small files (even binary) are more likely to have similar content because of the headers and boilerplate.
5. For the smallest files the tar's file headers with the file path is more likely to be the most similar thing about the file.