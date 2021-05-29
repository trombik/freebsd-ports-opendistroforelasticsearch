## `textproc/opendistroforelasticsearch`

FreeBSD port of Open Distro for Elasticsearch.

## Current status

* `portlint` is happy
* `poudriere bulk` passed
* the package is being tested

## Usage

the repository is intended to be used by [`portshaker`](https://github.com/smortex/portshaker)
and [`poudriere`](https://github.com/freebsd/poudriere).

An example `portshaker.conf`.

```text
mirror_base_dir="/var/cache/portshaker"
ports_trees="default"
default_ports_tree="/usr/local/poudriere/ports/default"
default_merge_from="portsnap \
	github:trombik/freebsd-ports-opendistroforelasticsearch/master \
	"
```
