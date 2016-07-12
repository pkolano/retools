Restriping Tools for Lustre (Retools)
=====================================

Modern parallel file systems achieve high performance by distributing
("striping") the contents of a single file across multiple physical
disks to overcome single disk I/O bandwidth limitations.  The striping
characteristics of a file determine how many disks it will be striped
across and how large each stripe is.  These characteristics can only be
set at the time a file is created so cannot be changed later on.
Standard open source tools do not typically take striping into account
when creating files meaning that files created by those tools will have
their striping characteristics set to the default.  The default stripe
count is typically set to a small number to favor small files that are
more numerous.  A small default stripe count, however, penalizes large
files that use the default settings as they will be striped over fewer
disks, hence access to these files will only achieve a fraction of the
performance that is possible with a larger stripe count.  A large
default stripe count, however, causes small files to be striped over too
many disks, which increases contention and reduces the performance of
the file system as a whole.

To maximize I/O performance for large files while keeping the default
stripe count low for small files, a set of modifications, called
Retools, have been made to some open source utilities that are commonly
used with large files in high performance computing environments.  These
modifications make the tools "stripe-aware" so they can set an optimum
stripe size for each file created instead of using the default striping.
The currently supported utilities are bzip2, gzip, rsync, and tar.
The compression utilities bzip2 and gzip set the striping of the
compressed/decompressed file based on the size of the corresponding
decompressed/compressed file, respectively.  The synchronization utility
rsync sets the striping of each destination file based on the size of
the corresponding source file.  Finally, the archival utility tar sets
the striping of each archived/extracted file based on the size of the
corresponding source/archived file, respectively.

Note that a stripe-aware version of cp is available in a separate
project at https://pkolano.github.io/projects/mutil.html.

The benefit of using these tools is that files on Lustre file systems
are created/extracted with a stripe count appropriate for their size.
This means they will be striped across more physical disks and
operations on these files will achieve greater I/O performance.

For full details of the implementation and expected performance, see
https://pkolano.github.io/papers/hpdic13.pdf.  For installation
details, see "INSTALL".  For usage details, see the standard
documentation of each individual tool.

Questions, comments, fixes, and/or enhancements welcome.

--Paul Kolano <paul.kolano@nasa.gov>

