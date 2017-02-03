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

