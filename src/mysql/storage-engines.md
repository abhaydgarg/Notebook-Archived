# Storage engines

## InnoDB

This is the default storage engine for MySQL 5.5 and higher. It provides transaction-safe (ACID compliant) tables, supports FOREIGN KEY referential-integrity constraints. It supports commit, rollback, and crash-recovery capabilities to protect data. It also support row-level locking. It's "consistent nonlocking reads" increases performance when used in a multiuser environment. It stores data in clustered indexes which reduces I/O for queries based on primary keys.

## MyISAM

This storage engine, manages non transactional tables, provides high-speed storage and retrieval, supports full text searching.

## MEMORY

Provides in-memory tables, formerly known as HEAP. It sores all data in RAM for faster access than storing data on disks. Useful for quick looks up of reference and other identical data.

## MERGE

Groups more than one similar MyISAM tables to be treated as a single table, can handle non transactional tables, included by default.

## EXAMPLE

You can create tables with this engine, but can not store or fetch data. Purpose of this is to teach developers about how to write a new storage engine.

## ARCHIVE

Used to store a large amount of data, does not support indexes.

## CSV

Stores data in Comma Separated Value format in a text file.

## BLACKHOLE

Accepts data to store but always returns empty.

## FEDERATED

Stores data in a remote database.
