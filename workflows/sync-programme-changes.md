# Sync Programme Changes

## Context
Integration Partner picking up changes since its last sync.

## Role
Integration Partner

## Prerequisites
- Partner holds the time of its last sync (see `download-full-programme.md`).

## Steps
1. Partner requests talks changed since its last-sync time.
2. System returns every talk with Last updated later than that time, Cancelled ones included, oldest change first (BR-SYN-04).
3. Partner updates its copy; cancelled talks are shown as cancelled.
4. Partner compares with a full download now and then to detect deleted talks (BR-SYN-05).

## Expected Result
The partner's copy matches the live programme.
