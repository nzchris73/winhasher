# Upgrade Options

## Upgrade Strategy

| Strategy | Description | Selected |
|----------|-------------|----------|
| **All-at-Once** | Upgrade all projects simultaneously in a single atomic operation. Best for small solutions where all projects are tightly coupled and need to work together. | **✓ (selected)** |
| Bottom-Up | Upgrade foundation libraries first, then applications. Good for larger solutions with clear separation. | |
| Top-Down | Start with applications and layer in library support. Useful for phased migration. | |

**Why selected**: Your solution has 3 projects in a simple linear dependency (WinHasherCore → 2 apps). All are .NET 6/.NET Standard 2.1 targeting modern .NET. Atomic upgrade ensures all parts work together immediately.

---

## Rationale

✅ Simple dependency hierarchy makes all-at-once safe
✅ Small project count (3 projects) means manageable scope
✅ No legacy .NET Framework projects requiring multi-targeting
✅ No ground-level infrastructure risks that need phased rollout

**No additional options are applicable to this solution.**
