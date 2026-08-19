---
name: shared-resource-leases
description: Coordinate cooperative agents or users that share local services, databases, ports, GPUs, simulators, or development environments using health checks, capacity policies, and expiring read/write/destroy leases. Use when concurrent access can conflict; this is coordination, not security isolation.
---

# Shared resource leases

Use the project-approved resource coordinator as the collaboration source of
truth for resource definitions, health checks, capacity, and active leases.
Do not invent a second lock file or bypass an existing registry.

The coordinator helps well-behaved clients cooperate. It does not enforce
security, grant permissions, or expand the authority of the current user or
agent.

## Before using a resource

1. Establish a stable owner identity for the current session. Subagents use
   distinct owners; retries by the same agent reuse the same owner.
2. Inspect existing registrations before adding a resource. Avoid aliases that
   describe the same underlying service.
3. Confirm the resource has a fast health check that never prints credentials,
   tokens, connection strings, or sensitive payloads.
4. Model actual concurrency:
   - `read` for observation proven to have no side effects;
   - `write` for ordinary mutations, task triggers, or uncertain operations;
   - `destroy` for stop, restart, rebuild, deletion, or anything that disrupts
     existing readers and writers.
5. Use conservative capacity. Allow mixed reads and writes only when the
   underlying resource genuinely supports them.

## Acquire atomically

Availability checks are advisory and race with other clients. Immediately
before touching the resource, acquire the required lease atomically.

- Use a bounded TTL; never create an infinite lease.
- Treat an acquisition conflict as a coordination result. Report the holder
  and expiry when available, then wait, choose another execution surface, or
  ask the user.
- Never steal or silently bypass a lease.
- A repair may acquire `write` or `destroy` even when health is failing; check
  health again after the repair.
- Upgrading from a weaker mode to `destroy` requires releasing the old lease
  and competing again. Do not describe the gap as atomic.

## Finish cleanly

Renew before expiry during long work. Release the lease on success, failure,
cancellation, handoff, and session end. Keep lease tokens out of Git, reviews,
shared logs, and user-facing output. TTL is the recovery path when cleanup hooks
do not run.

```text
Registry + health + capacity
→ advisory availability
→ atomic expiring lease
→ resource operation
→ health/evidence
→ release
```
