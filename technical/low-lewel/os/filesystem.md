# [btrfs (B-tree file system)](https://btrfs.readthedocs.io/en/latest/Introduction.html)

- ***Copy-on-write architecture*** – All block modifications are written to new locations, preserving the old copy. Enables easy snapshots and integrity checks.
- ***Snapshots*** – Read-only point-in-time snapshots can be created instantly, consuming no extra space until changes are made. Helpful for backups and reverting mistakes.
- ***Online defragmentation*** – Mitigates fragmentation to optimize read performance while filesystem remains online.
- ***Online resizing*** – Total storage and individual logical volumes can be grown or shrunk on the fly.
- ***Compression*** – Per-file zlib or LZO compression can save significant space with minimal overhead.
- ***SSD optimization*** – TRIM/discard support, SSD-friendly allocation schemes, and other tweaks to extend SSD lifespan.
- ***Integrated multiple device support*** – Btrfs filesystems can natively span across multiple disks and RAID configurations.
- ***Checksumming*** – All metadata and data is checksummed to detect silent corruption.

# [xfs](https://xfs.wiki.kernel.org/#documentation)

- ***High throughput I/O*** – Optimized for streaming large files like media content. Used in video editing pipelines.
- ***Metadata scalability*** – Flexible B+ tree directory structure scales to billions of files and folders.
- ***Delayed allocation*** – Separates metadata logging from actual data allocation to boost write speeds.
- ***Space preallocation*** – Entire files can be allocated upfront in contiguous sections for optimal throughput.
- ***Parallel I/O*** – Concurrent threads can read/write different regions of the same file in parallel.
- ***Stripe-aware allocation*** – Stores files intelligently across RAID stripes to maximize performance.
- ***Huge filesystem support*** – Supports yottabyte-scale filesystem sizes and 8 exabyte max file sizes.
- ***Rock-solid reliability*** – Enterprise-grade resiliency against crashes and corruption.

Comparison
- [btrfs vs xfs](https://thelinuxcode.com/btrfs-vs-xfs-brief-comparison/)