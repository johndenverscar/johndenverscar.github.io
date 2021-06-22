---
layout: post
title: Production Database Checklist
date: 2021-06-22 10:10
---

A non-definitive, quickly put together, hastily turned into a blog post checklist for deploying production DBs.

## Stages:

1. Plan
2. Install
3. Configure
4. Backup/Restore
5. Auth/Auth
4. Prepare
5. Utilities
6. Monitor
7. Test

### Stage 1: Plan

Get the schema, data types, permissions, and optionally indexes planned out and ready to go.
Use your development environments as a foundation here.

- [] Table schema
- [] Column datatypes
- [] Table workloads
- [] Table authorization
- [] Table indexes ( can be done later )

### Stage 2: Install

Decide how and where you're going to install your databases. 
A lot of this checklist comes baked in when using AWS/GCP/Azure, but not everyone has that option.
If you're deploying on-prem, decide on how.
Docker can be a massive help.
Vagrant VMs go a long way to separate environments.

It's also useful here, to build a deployment script here.
No need to go full Chef/Ansible, but a simple bash script can be a huge help in the long run.

- [] Installation method ( VM, Docker, etc... )
- [] Primary server
- [] Secondary server ( 2+ )
- [] Shards ( optional, 2+ )
- [] Server healthchecks

### Stage 3: Configure

Please don't run a single-node production server. Please.

- [] Replication
- [] Clustering
- [] Scale up policy
- [] Scale down policy
- [] Automatic failover
- [] Sharding ( optional )
- [] Read/Write prioritization

### Stage 4: Backup & Restore

Bare minimum here is a snapshot backup and restore process that runs via a `bash`/`sh` script.
Everyone on the team needs to be able to backup and restore the database.
This will also be used to automate the backups via `crontab` or a platform like Jenkins.

- [] Point-in-time rollback
- [] Snapshot backup
- [] Restore from snapshot
- [] Archiving ( optional )
- [] Archive restore

### Stage 4: Auth/Auth

Figure out how you're going to manage the users.
You'll at least want the following roles: admin, developer, application, and readonly.

Private data needs to be encrypted both at rest and in motion.

You don't need the entire internet to be able to connect to your DB, that's what REST is for.
Whitelist based firewall and don't look back.

- [] Authentication ( PAM )
- [] Connection auditing
- [] Change auditing
- [] Roles, users, and permissions
- [] Encryption at rest
- [] Encryption in motion
- [] Firewall

### Stage 4: Query Control

These are mostly optional but limiting results and connections can really save your performance.

- [] Enforce commits
- [] Result limits
- [] Pagination
- [] Query timeout
- [] Thread pool
- [] Buffer pool
- [] Cacheing & timeout

### Stage 4: Prepare

Nothing like running out of disk space in production, right?

- [] Storage
- [] Partitioning
- [] Table compression
- [] Column compression

### Stage 5: Utilities

These are the scripts I typically use.
I try to eliminate all possible sources of human error that I can.

- [] Create user
- [] Create table
- [] Add table to role

### Stage 6: Monitor

Not everyone is a DB admin.
Find or build a tool to allow everyone to monitor the database.
Listing connections, finding who made what changes, etc...
Even if it's just a bunch of bash scripts or one big bash script.
Most developers can manage running `./db-stats --host <...>`.
Not all developers are going to know how or where to look to find the in the weeds data.

- [] Connection monitoring
- [] Role permissions
- [] Role inspection
- [] Table metadata
- [] Table permissions
- [] Change log
- [] Event notifications

### Stage 7: Test

This isn't a one time thing.
Test this as often as you possibly can.
Your backups don't exist until you restore from them.

- [] Backup
- [] Restore from snapshot
- [] Point-in-time rollback
- [] Migrate from snapshot
- [] Utility testing
- [] Automatic scaling
- [] Automatic failover
- [] Event notifications
