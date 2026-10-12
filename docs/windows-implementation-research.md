# Windows Support Implementation Research - Fixes #9

## 1. Why WinFsp
WinFsp is best for Windows: stable, used by SSHFS-Win, borgbackup. Has Rust crates: winfsp https://crates.io/crates/winfsp and winfsp-rs https://github.com/SnowflakePowered/winfsp-rs . Docs: https://github.com/winfsp/winfsp/wiki/Known-File-Systems . Dokany has no official Rust bindings. Decision: WinFsp.

## 2. Licensing
WinFsp GPLv3, compatible with rencfs GPL. Users must install winfsp.msi from https://winfsp.dev/rel/

## 3. Crate Layout
Create src/mount/windows.rs similar to linux.rs. New struct EncryptedFsWinFsp wrapping EncryptedFs implementing FileSystemContext. Cargo.toml: winfsp = { version = "2.0", features = ["fuse"] }

## 4. Callback Mapping
lookup->Open/Create, getattr->GetFileInfo, readdir->ReadDirectory, read/write->Read/Write, create/mkdir->Create, unlink/rmdir->Cleanup/Delete. Windows is case-insensitive, need normalization.

## 5. Metadata
Map times to FILETIME, DIRECTORY vs NORMAL attributes, default permissive security descriptor, disable symlinks for v1, case_insensitive=true.

## 6. CI
Add windows-latest job: choco install winfsp -y, cargo build.

## 7. Smoke Test
1. winget install WinFsp.WinFsp
2. cargo build --release
3. rencfs mount --data-dir ./data --mount-point X:
4. test create/read/write/rename/delete and remount

This completes research for basic working implementation.
Fixes #9
