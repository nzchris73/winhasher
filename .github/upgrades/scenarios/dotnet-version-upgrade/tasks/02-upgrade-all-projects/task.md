# 02-upgrade-all-projects: Update target frameworks, packages, and fix compatibility issues

Upgrade all three projects simultaneously: change target frameworks (WinHasherCore to net10.0, hash and WinHasher to net10.0-windows), update NuGet package versions to .NET 10 compatible releases, and fix API breaking changes across all projects.

The assessment identified 2429 issues across the solution, primarily in the WinForms project (WinHasher) related to System.Windows.Forms API compatibility with .NET 10. The hash project has 1 compatibility issue. WinHasherCore (class library) has no identified issues and can be upgraded cleanly. All changes are applied atomically — the solution will not build until all updates are complete, then validation occurs in a single bounded pass.

## Scope & Projects

### 1. WinHasherCore (Class Library)
- **Current**: netstandard2.1 (SDK-style, no special features)
- **Target**: net10.0
- **Current properties**: GenerateAssemblyInfo=false, TreatWarningsAsErrors=True (Debug+Release)
- **Assessment**: 0 issues, clean upgrade
- **Changes needed**: Update TargetFramework property only

### 2. hash (Console Application)
- **Current**: net6.0-windows10.0.22621.0 (SDK-style)
- **Target**: net10.0-windows
- **Current properties**: ErrorReport=none, RollForward=LatestMajor, TreatWarningsAsErrors=True, NoWarn=empty
- **Assessment**: 1 mandatory issue (API incompatibility with System.Windows.Forms)
- **Depends on**: WinHasherCore
- **Changes needed**: Update TargetFramework, fix API issues

### 3. WinHasher (WinForms Application)
- **Current**: net6.0-windows10.0.22621.0 (SDK-style with WinForms)
- **Target**: net10.0-windows
- **Current properties**: UseWindowsForms=true, ImportWindowsDesktopTargets=true, SupportedOSPlatformVersion=10.0.22621.0, NoWarn includes CA1416 (platform-specific)
- **Assessment**: 2428 mandatory issues (primarily System.Windows.Forms API compatibility - ~2403 binary incompatible, ~24 source incompatible)
- **Depends on**: WinHasherCore
- **Changes needed**: Update TargetFramework, fix API issues

## Upgrade Sequence

**All-at-once approach**: All three projects upgraded together in single atomic operation.

1. **Update all target frameworks** in .csproj files:
   - WinHasherCore: netstandard2.1 → net10.0
   - hash: net6.0-windows10.0.22621.0 → net10.0-windows
   - WinHasher: net6.0-windows10.0.22621.0 → net10.0-windows

2. **Check and update NuGet packages** to .NET 10 compatible versions (if any)

3. **Restore dependencies** with `dotnet restore`

4. **Build solution** and fix all compilation errors in single pass:
   - Resolve System.Windows.Forms API breaking changes
   - Address any other compatibility issues

5. **Verify zero compilation errors** before moving to final validation task

## Build Tool Decision

**Selected**: `dotnet build` with full solution scope (all three projects are SDK-style .NET 6/.NET Standard)

**Done when**:
- All project target frameworks updated (TFM changes in .csproj files)
- All NuGet package versions updated to .NET 10 compatible releases
- All API compatibility issues fixed (System.Windows.Forms breaking changes, etc.)
- Solution builds with zero compilation errors
- All projects restore correctly
