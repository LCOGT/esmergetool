Elasticsearch Merge Tool
========================

This is a tool to combine several Elasticsearch indices into a single
destination index.

This tool was originally designed to merge a month worth of "daily" Logstash
indices into a single "monthly" Logstash index for long term information
archival purposes.

This tool was written to support large merge jobs. This includes support for:

- Merge one source index into the destination at a time
- Automatically open/close the source indices as needed
- Interrupt a reindex job at any time
- Resume an interrupted reindex job
- Automatically set the source indices to read-only before using them
- Automatically set the destination index to read-only after the job is complete
- Provides a "dry run" mode so you can check your settings safely

Dependencies
============

This tool uses the [Elasticsearch Reindex API](https://www.elastic.co/guide/en/elasticsearch/reference/2.4/docs-reindex.html),
which is only available in Elasticsearch 2.4 and newer.

This code is written for Python 2.7.

The Python installation dependencies are specified in `requirements.txt`, and
only consist of the Official Elasticsearch Python API.

Known Limitations
=================

- Does not copy field mappings from the source indices to the destination index

Command Line Interface
======================

The command line interface help output is reproduced below:

    usage: esmergetool [-h] [--host HOST] [-n] [-y] [-s SOURCE_INDEX_PATTERN]
                       [-d DESTINATION_INDEX] [-r] [--set-source-readonly]
                       [--set-destination-readonly] [--status-index STATUS_INDEX]

    A Tool to combine many Elasticsearch indices into a single combined index

    optional arguments:
      -h, --help            show this help message and exit
      --host HOST           Elasticsearch cluster host(s) (comma separated)
      -n, --dry-run         Do not perform any action
      -y, --yes             Assume a yes answer to any prompt
      -s SOURCE_INDEX_PATTERN, --source-index-pattern SOURCE_INDEX_PATTERN
                            Source indices pattern
      -d DESTINATION_INDEX, --destination-index DESTINATION_INDEX
                            Destination index
      -r, --resume          Continue a previous job matching these parameters
      --set-source-readonly
                            Set source indices read-only as we finish with them
      --set-destination-readonly
                            Set destination index read-only when complete
      --status-index STATUS_INDEX
                            Override default status index (default: esmergetool-
                            status)

Example Output
==============

Here is some example output to get you started:

    $ python esmergetool --host mycluster.example.com --source-index-pattern 'logstash-2017.01.*' --destination-index 'testmerge' --resume
    YOU MUST CONFIRM THAT THIS INFORMATION IS CORRECT BEFORE PROCEEDING:

    Source Indices:
    - logstash-2017.01.05 (count=26303, closed=False)
    - logstash-2017.01.06 (count=17651, closed=False)
    - logstash-2017.01.07 (count=14700, closed=False)
    - logstash-2017.01.08 (count=11370, closed=False)
    - logstash-2017.01.09 (count=11407, closed=False)
    - logstash-2017.01.10 (count=21365, closed=False)
    - logstash-2017.01.11 (count=29566, closed=False)
    - logstash-2017.01.12 (count=18109, closed=False)
    - logstash-2017.01.13 (count=23662, closed=False)
    - logstash-2017.01.14 (count=18029, closed=False)
    - logstash-2017.01.15 (count=10577, closed=False)
    - logstash-2017.01.16 (count=17640, closed=False)
    - logstash-2017.01.17 (count=19286, closed=False)
    - logstash-2017.01.18 (count=15298, closed=False)
    - logstash-2017.01.19 (count=24461, closed=False)
    - logstash-2017.01.20 (count=17033, closed=False)
    - logstash-2017.01.21 (count=10132, closed=False)
    - logstash-2017.01.22 (count=12235, closed=False)
    - logstash-2017.01.23 (count=14152, closed=False)
    - logstash-2017.01.24 (count=18764, closed=False)
    - logstash-2017.01.25 (count=21512, closed=False)
    - logstash-2017.01.26 (count=18134, closed=False)
    - logstash-2017.01.27 (count=14161, closed=False)
    - logstash-2017.01.28 (count=24787, closed=False)
    - logstash-2017.01.29 (count=14754, closed=False)
    - logstash-2017.01.30 (count=21124, closed=False)
    - logstash-2017.01.31 (count=17237, closed=False)

    Destination Index:
    - testmerge (count=N/A, closed=N/A)

    Additional Actions:
    - Source indices will be automatically opened (if necessary) during merge
    - Destination index will be created

    Please confirm by typing "y" now: y
    Confirmation given interactively by user

    Creating destination index: testmerge
    Configuring destination index in read-write mode
    Configuring destination index for maximum indexing speed
    Begin Background Reindex: src=logstash-2017.01.05 dst=testmerge
    Elasticsearch reindex task id: fv10hW3QRz-_vBzGMK1RhA:9387881
    Reindex is running (source_index: logstash-2017.01.05, runtime: 0.01 seconds, 0/0 => 0.00% complete)
    Reindex is running (source_index: logstash-2017.01.05, runtime: 1.23 seconds, 4000/26303 => 15.21% complete)
    Reindex is running (source_index: logstash-2017.01.05, runtime: 2.24 seconds, 8000/26303 => 30.41% complete)
    Reindex is running (source_index: logstash-2017.01.05, runtime: 3.26 seconds, 14000/26303 => 53.23% complete)
    Reindex is running (source_index: logstash-2017.01.05, runtime: 4.27 seconds, 21000/26303 => 79.84% complete)
    Begin Background Reindex: src=logstash-2017.01.06 dst=testmerge
    Elasticsearch reindex task id: fv10hW3QRz-_vBzGMK1RhA:9388330
    Reindex is running (source_index: logstash-2017.01.06, runtime: 0.01 seconds, 0/0 => 0.00% complete)
    Reindex is running (source_index: logstash-2017.01.06, runtime: 1.03 seconds, 6000/17651 => 33.99% complete)
    Reindex is running (source_index: logstash-2017.01.06, runtime: 2.04 seconds, 12000/17651 => 67.98% complete)
    ... output truncated here ...
    Begin Background Reindex: src=logstash-2017.01.30 dst=testmerge
    Elasticsearch reindex task id: fv10hW3QRz-_vBzGMK1RhA:9396448
    Reindex is running (source_index: logstash-2017.01.30, runtime: 0.01 seconds, 0/0 => 0.00% complete)
    Reindex is running (source_index: logstash-2017.01.30, runtime: 1.03 seconds, 6000/21124 => 28.40% complete)
    Reindex is running (source_index: logstash-2017.01.30, runtime: 2.04 seconds, 7000/21124 => 33.14% complete)
    Reindex is running (source_index: logstash-2017.01.30, runtime: 3.05 seconds, 14000/21124 => 66.28% complete)
    Begin Background Reindex: src=logstash-2017.01.31 dst=testmerge
    Elasticsearch reindex task id: fv10hW3QRz-_vBzGMK1RhA:9396773
    Reindex is running (source_index: logstash-2017.01.31, runtime: 0.01 seconds, 0/0 => 0.00% complete)
    Reindex is running (source_index: logstash-2017.01.31, runtime: 1.02 seconds, 7000/17237 => 40.61% complete)
    Reindex is running (source_index: logstash-2017.01.31, runtime: 2.04 seconds, 15000/17237 => 87.02% complete)
    Reindex job is complete, optimize and reconfigure destination index settings

