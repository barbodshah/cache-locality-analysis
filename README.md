# Cache locality analysis

Coursework tools for analyzing reuse in storage traces and comparing cache replacement policies. The C++ analyzer ranks an offset's last access in an AVL tree augmented with subtree sizes. The Python simulator compares LRU, N-Hit, N-Hit LRU, ARC, LARC, and the offline Belady oracle.

## Run on the included synthetic trace

From this directory, with a C++17 compiler and Python:

```sh
g++ -std=c++17 -O2 reuse_distance.cpp -o reuse_distance
./reuse_distance examples/tiny.csv
python -m pip install -r requirements.txt
python replacement_policies.py examples/tiny.csv --capacity 2
```

On Windows the compiled executable may be named `reuse_distance.exe`.

Use a headerless CSV with five fields per row: timestamp, unused field, byte offset, request size, and operation (`Read` or `Write`). Both tools use exact offsets as identities and do not expand requests across cache lines. Reuse-distance statistics cover repeated offsets. The example has distances 1 and 2, so bins 1 and 2 each contain one count. Bins are inclusive upper bounds and the mean is truncated to an integer. The policy tool uses capacity in entries and prints read and write hit/miss counts. At capacity 2 the example has one LRU read hit, three read misses, and one write miss. Belady has two read hits, two read misses, and one write miss. The example is a smoke test, not a performance workload.
