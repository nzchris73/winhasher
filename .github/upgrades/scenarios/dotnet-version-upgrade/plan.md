# .NET 10 Upgrade Plan

## Selected Strategy

**All-At-Once** — All projects upgraded simultaneously in a single atomic operation.

**Rationale**: 3 projects, all on .NET 6 / .NET Standard 2.1, simple linear dependency structure (WinHasherCore → hash and WinHasher). No .NET Framework projects. Atomic upgrade is safe and efficient for this configuration.

---

## Projects in Scope

### Foundation Library
- **WinHasherCore** (.NET Standard 2.1 → .NET 10) — Class library with no dependencies

### Applications  
- **hash** (.NET 6-windows → .NET 10-windows) — Console application, depends on WinHasherCore
- **WinHasher** (.NET 6-windows → .NET 10-windows) — WinForms application, depends on WinHasherCore

---

## Upgrade Tasks

### 01-prerequisites: Verify SDK and toolchain compatibility

Ensure .NET 10 SDK is installed and global.json (if present) allows .NET 10 or later. Check for any platform-specific requirements for Windows Forms applications on .NET 10.

The foundation for a successful upgrade begins with verified prerequisites: correct SDK version, compatible toolchain, and no blocking environment issues.

**Done when**:
- .NET 10 SDK is installed and available on the system
- global.json (if present) is compatible with .NET 10
- No blocking toolchain or environment issues detected

### 02-upgrade-all-projects: Update target frameworks, packages, and fix compatibility issues

Upgrade all three projects simultaneously: change target frameworks (WinHasherCore to net10.0, hash and WinHasher to net10.0-windows), update NuGet package versions to .NET 10 compatible releases, and fix API breaking changes across all projects.

The assessment identified 2429 issues across the solution, primarily in the WinForms project (WinHasher) related to System.Windows.Forms API compatibility with .NET 10. The hash project has 1 compatibility issue. WinHasherCore (class library) has no identified issues and can be upgraded cleanly. All changes are applied atomically — the solution will not build until all updates are complete, then validation occurs in a single bounded pass.

**Done when**:
- All project target frameworks updated (TFM changes in .csproj files)
- All NuGet package versions updated to .NET 10 compatible releases
- All API compatibility issues fixed (System.Windows.Forms breaking changes, etc.)
- Solution builds with zero compilation errors
- All projects restore correctly

### 03-final-validation: Build and test the upgraded solution

Run a full solution build and execute the test suite to verify the upgrade was successful and no functionality was broken.

The upgrade is complete only when the solution compiles cleanly and all functionality works as expected.

**Done when**:
- Full solution builds with zero warnings or errors
- All test suites pass (if present)
- Applications can run successfully
- No runtime compatibility issues detected

