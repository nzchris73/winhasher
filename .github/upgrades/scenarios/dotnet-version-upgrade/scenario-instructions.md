# .NET Version Upgrade

## Preferences
- **Flow Mode**: Automatic
- **Target Framework**: net10.0 (LTS)

## Source Control
- **Source Branch**: master
- **Working Branch**: upgrade-dotnet-10
- **Commit Strategy**: After Each Task
- **Branch Sync**: Auto (Merge)

## Strategy
**Selected**: All-At-Once

**Rationale**: 3 projects on .NET 6/.NET Standard 2.1 with simple linear dependency structure. No .NET Framework projects. Atomic upgrade is safe and efficient.

### Execution Constraints
- All projects are upgraded together in a single atomic operation — no tier ordering or phasing
- Update all project files simultaneously (TFM, imports, conditional logic)
- Update all package references across all projects as one unit  
- Single bounded pass for compilation errors — build and fix all issues once
- Full solution must build with 0 errors before proceeding to validation
- Testing occurs after atomic upgrade completes successfully
