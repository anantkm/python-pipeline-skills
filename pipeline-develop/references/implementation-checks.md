# Implementation checks by task and route

Use common checks plus only the task/route sections that apply. Derive expected behavior from actual contracts. These prompts are not universal requirements to add infrastructure or features.

## Common delivery and recovery

Establish source pagination/CDC contract, primary or composite keys, watermark precision and tie handling, cursor inclusivity, late-arrival policy, source deletes and retention, and explicit destination append/upsert semantics.

Before implementing checkpoint logic, trace crash before write, during partial write, after durable write but before acknowledgement, and after write but before checkpoint. Specify whether replay produces allowed duplicates or converges via a real key/operation. Do not claim exactly-once from retries alone. A batch can have mixed outcomes; identify when a checkpoint would skip failed records.

Bound rate-limit retries and resource use. Preserve actionable errors and rejected-record handling according to team policy. Distinguish no data from extraction failure. Test empty pages, shared cursor timestamps, null/missing values, type boundaries, repeated batches, partial success and restart where reachable. Assess overlap between scheduled jobs/backfills if they can share state. Freshness and count checks complement record-level identity/content reconciliation; matching counts alone do not prove correctness.

## New pipeline

Start with a comparable repository pipeline, actual source contract, payload samples/schema, and acceptance criteria. Establish schedule/trigger, file/table naming, key/ordering semantics, configuration/secrets conventions, and ownership of progress. Implement a coherent local slice without inventing infrastructure choices. Include documented external setup and operational checks, with unknowns explicit. Avoid building generic frameworks for one source.

## Add columns or change types

Trace source projection → transformation → serializer/file schema → writer/connector → destination → known consumers. Update explicit field lists and positional mappings. Check old records/files lacking fields, requiredness, defaults, nested fields, type precision, naming/case collisions, and mixed old/new producers/readers. Ask whether historical completeness/backfill is required; do not fabricate defaults or automatically reprocess history. Separate code rollout from persisted schema changes. Prepare compatibility and rollback limits.

## Fix source/pipeline defects

Establish failing input/state and root cause before changing logic. Prefer a regression using the real transformation/checkpoint path. Do not mask source contract violations as success. Check that the fix prevents recurrence and identify whether already missed/duplicated/corrupted data needs separate repair. Prepare repair/reconciliation instructions, but do not run external recovery.

## Source → Iceberg

Confirm engine/writer and table-format versions, catalog, write mode, schema and partition specs. Use supported APIs; do not infer field-ID, rename/type-evolution, merge/delete, or concurrent commit behavior from generic SQL familiarity.

Check data-file creation versus metadata commit, acknowledgement loss and replay, conflict retries, key ordering/uniqueness for merges, partition movement, schema cache/mixed versions and readability of older files. Maintenance/retention/orphan cleanup needs assessment only when touched or depended upon; never run it as part of local development. Do not infer multi-table atomicity.

## Source → S3 → Fivetran → Snowflake

Confirm actual file format, object naming and publication convention, connector type/settings, discovery rules and destination write semantics. S3 storage guarantees do not establish connector consumption behavior.

Handle serialization, quoting, compression, encoding, field ordering and malformed data per contract. Establish whether retry overwrites or generates new object keys; check collisions and replay consequences. A manifest/completion convention is useful only if the actual consumer supports it; do not invent one to imply atomic ingestion.

Check prefix/filter rules, changed-object detection, load history/deduplication/re-sync and failed-file behavior from actual evidence. For schema changes establish field discovery/selection, naming normalization, destination types/nullability and historical missing fields. Do not automatically provision a connector or apply warehouse DDL. Prepare required external changes separately and report unverified end-to-end behavior.

## Backfill preparation

Establish exact affected keys/time range, authoritative source/retention, current checkpoint, existing destination state, live ingestion overlap, deduplication mechanism and acceptance criteria. Prepare a bounded replay procedure, stop conditions, resource limits and reconciliation. Explain irreversible effects and recovery limits. Do not execute production backfills, delete data, or reset external checkpoints.
