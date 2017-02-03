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

Dependencies
============

The Python installation dependencies are specified in `requirements.txt`, and
only consist of the Official Elasticsearch Python API.

This code is written for Python 2.7.
