---
title: ALTER INDEX
summary: Use the ALTER INDEX statement to change an existing index.
toc: false
---

The `ALTER INDEX` [statement](sql-statements.html) applies a schema change to an index.

{% include {{{ page.version.version }}/misc/schema-change-stmt-note.md %}

For information on using `ALTER INDEX`, see the documents for its relevant subcommands.

{% include {{{ page.version.version }}/misc/schema-change-view-job.md %}

Subcommand | Description
-----------|------------
[`CONFIGURE ZONE`](configure-zone.html) | [Configure replication zones](configure-replication-zones.html) for an index.
[`RENAME`](rename-index.html) | Change the name of an index.
[`SPLIT AT`](split-at.html) | Force a range split at the specified row in the index.
[`UNSPLIT AT`](unsplit-at.html) | <span class="version-tag">New in v19.2:</span> Remove a range split enforcement at a specified row in the index.
