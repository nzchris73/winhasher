# 01-prerequisites: Verify SDK and toolchain compatibility

Ensure .NET 10 SDK is installed and global.json (if present) allows .NET 10 or later. Check for any platform-specific requirements for Windows Forms applications on .NET 10.

The foundation for a successful upgrade begins with verified prerequisites: correct SDK version, compatible toolchain, and no blocking environment issues.

## Research Findings

**Environment Status**:
- **.NET SDK version**: 10.0.400 ✓ (Required: .NET 10)
- **global.json**: Not present in repo root ✓ (No version constraint to check)
- **System**: Windows (supports WinForms on .NET 10)
- **Projects**: 2 WinForms applications + 1 class library (all require Windows platform support)

**Status**: ✅ All prerequisites met. Environment is ready for upgrade.

**Done when**:
- .NET 10 SDK is installed and available on the system ✓
- global.json (if present) is compatible with .NET 10 ✓
- No blocking toolchain or environment issues detected ✓
