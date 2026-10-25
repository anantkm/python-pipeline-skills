# Pipeline failure checks

Read common checks and only the route/change sections relevant to the review. Establish reachability, requirements, and evidence before reporting a defect. These are prompts for investigation, not automatic findings.

## Common ingestion and recovery

- Identity: actual record grain, composite/null/changing keys, duplicates, update ordering, deletes/tombstones.
- Extraction: empty page versus end, pagination token lifecycle, stable ordering, equal cursor timestamps, mutation during pagination, cursor inclusivity/precision, time zones/DST, late records, API versions and source retention.
- Partial failure: timeout after successful write, partial batches, throttling, retry budget, poison records, cancellation, process death. Trace checkpoint-before-write and write-before-checkpoint crash windows against the actual delivery contract.
- Replay: duplicate batches, partial retry, overlapping jobs, concurrent backfill/live ingestion, old checkpoints. Establish allowed duplicate/deduplication semantics; do not impose exactly-once by default.
- Conversion: absent/null/empty, zero/False, nested arrays, Unicode/quoting, decimals/overflow, timestamp units/time zones, binary and malformed/oversized records.
- Operations: no data versus extraction failure; errors advancing progress; required freshness/volume/schema/rejection signals, sensitive logs, bounded memory, evidenced throughput/load limits.

## Source → Iceberg

Establish actual engine/writer versions, catalog, table format version, and write mode.
- Schema identity versus names; rename/drop/re-add, nested evolution, requiredness, compatible types, existing-file readability, mixed reader/writer versions and cached schemas.
- Concurrent commit conflicts and retry semantics; files created before failed commits; stale metadata; intended atomic visibility. Do not assume multi-table transactions.
- Merge keys and update ordering; partition movement; actual delete support; replay when successful commit acknowledgement was lost.
- Where relevant: partition-spec evolution, old/new readers, maintenance overlapping writes, snapshot retention versus active readers/backfills, orphan cleanup scope/retention.

## Source → S3 → Fivetran → Snowflake

Establish actual file format/publication convention, connector type/settings and documented behavior, and Snowflake destination settings.
- Delivery: overwritten versus new object keys on retry, key collisions, producer batch completeness, zero-byte files, compression, encoding, headers, quoting, mixed schemas and malformed records.
- Discovery: real prefix/filter rules, updated-object detection, load history, deduplication, replay/re-sync, rejected files and load delays. S3 behavior does not prove connector guarantees.
- Schema propagation: field selection, new-column discovery, normalization/case, type changes, nesting, historical missing fields, destination types/nullability.
- Destination: append/merge semantics, keys, updates/deletes, metadata fields, downstream models/views and access.
- Recovery: reconcile source, object, connector and destination states. Re-extracting safely does not establish reloading safely.

## Column additions and type changes

Trace payload → extractor projection → transformation → serialization → writer/connector → table → known consumers. Check explicit field lists and positional mappings. Distinguish future rows from historical completeness; establish whether defaults/backfills are actually required. Examine missing fields in old data, old/new files and writers, precision, naming collisions and evidenced select-star/positional consumers. Code rollback may not reverse persisted schema/data changes.

## Bug fixes and new pipelines

For fixes reproduce or establish the original failure and whether the change prevents it without hiding errors or dropping records. Separate recovery of existing bad data from preventing new bad data.
For new pipelines assess stated acceptance criteria, progress ownership and recovery boundaries. Existing example pipelines provide conventions, not proof their source assumptions apply here.
