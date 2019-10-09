---
title: CockroachDB Performance Benchmarking
summary: TBD
toc: true
---
This page provides an overview of CockroachDB's benchmark performance on industry standard benchmarks like TPC-C and Sysbench.

{{site.data.alerts.callout_success}}
If you are looking for specific information optimizing SQL performance see this overview [SQL Performance Best Practices](performance-best-practices-overview.md) or see [Performance Tuning](performance-tuning.html). For guidance on deployment and data location techniques to minimize network latency, see [Topology Patterns](topology-patterns.html).
{{site.data.alerts.end}}

CockroachDB provides predictable scale, throughput, latency, and concurrency at all cluster sizes.

We’ve tested CockroachDB extensively in public and private clouds observing consistent throughput. If you fail to achieve the performance profile listed below please contact us directly. More than likely their is a problem in either the hardware, workload, or test design. We stand by these profile characteristics and provide reproduction steps for all published benchmarks.

## Scale

### TPC-C 50K
<img src="{{ 'images/v19.2/tpcc50k.png' | relative_url }}" alt="TPC-C 50,000" style="max-width:100%" />

TPC-C is the best benchmark for OLTP workloads. The above chart demonstrates the CockroachDB processes 50X the throughput of Amazon Aurora when running TPC-C. To learn more about why we believe TPC-C is the best benchmark [click here](https://www.cockroachlabs.com/blog/cockroachdb-2dot1-performance/).

### Linear Scale
<img src="{{ 'images/v19.2/linearscale.png' | relative_url }}" alt="CRDB Linear Scale" style="max-width:100%" />

CockroachDB has no theoretical limit to scaling throughput or number of nodes. Practically, we can provide near linear performance up to 250 nodes.

This chart shows linear performance for CRDB as we scale out the number of nodes when running Sysbench. We chose Sysbench for this chart because it is easier to see the relationship between the number throughput and number of nodes. We used AWS C5D.4xlarge nodes to run these numbers. CockroachDB can scale both horizontally, number of nodes, and vertically, size of CPU per node. We prefer benchmarks like TPC-C because they offer the complex reads and writes that we think reflect our customers OLTP workloads.

## Throughput

We measure throughput as transactions per unit of time.

In the real world, applications generate transactional workloads which consist of a combination of reads and writes, possibly with concurrency and likely without all data being loaded into memory. If you see benchmark results quoted in QPS, take them with a grain of salt, because anything as simple as a “query” is unlikely to be representative of the workload you need to run in practice.

<img src="{{ 'images/v19.2/sysbench-throughput.png' | relative_url }}" alt="Sysbench Throughput" style="max-width:100%" />

## Latency
Latency, measured in milliseconds (ms), is usually distinguished as either read or write latencies depending upon the transaction.

It’s important to note that it is not sufficient to evaluate median latency. We must also understand the distribution, including tail performance, because these latencies occur frequently in production applications. This means that we must look critically at 95th percentile (p95) and 99th percentile (p99) latencies.

<img src="{{ 'images/v19.2/sysbench-latency.png' | relative_url }}" alt="Sysbench Latency" style="max-width:100%" />

###1 ms reads
CockroachDB returns single-row reads in 1 ms or less on average. We provide a number of important optimizations for both single-region and multi-region read performance including Secondary Indexes and Follower Reads.

###2 ms writes
CockroachDB processes single-row writes in 2 ms or less, and supports a variety of SQL and operational tuning practices for optimizing query performance.

<img src="{{ 'images/v19.2/1ms2ms.png' | relative_url }}" alt="1 ms reads, 2 ms writes" style="max-width:100%" />

## Concurrency

CockroachDB achieves excellent performance even under a large number of concurrent workers from a large number of clients.

<img src="{{ 'images/v19.2/concurrency.png' | relative_url }}" alt="Concurrency" style="max-width:100%" />

## Hardware

CockroachDB works well on commodity hardware. We are cloud-native and built to be cloud-agnostic. You can use CockroachDB with AWS, GCP, Azure, Digital Ocean, Rackspace, etc. You can also use CockroachDB with private datacenters and we have many customers that even mix public and private clouds!

CockroachDB creates a yearly cloud report focused on the performance of hardware. In November 2019, we will provide metrics on AWS, GCP, and Azure. In the meantime, you can read the 2018 Cloud Report that focuses on AWS and GCP.

## Workload Specific Performance

This document is about CockroachDB’s performance on benchmarks. For information about how to tune query performance, design your topology pattern, or otherwise improve performance on your workload, please consult our broader documentation.

## Known Limitations

CockroachDB has no theoretical limitations to scaling, throughput, latency, or concurrency other than the speed of light. Practically, we will be improving botltenecks and addressing challenges over the next several releases. In the meantime, we want you to be aware of the following known limitations.

- CockroachDB is not yet suitable for heavy analytics / OLAP.
- CockroachDB has not yet been tested beyond 512 nodes
- While CockroachDB supports SERIAL and sequential keys they can create hotspots within CockroachDB. We recommend using UUIDs or other methods to avoid writing to sequential keys
- CockroachDB is optimized for for good performance with rotational disk drives when using the durable memory storage engine. It is not recommended that you run with rotational HDDs when using the ssd storage engine.
