# Windows Installation Guide

This guide explains how to install and run `rencfs` on Windows.

Since `rencfs` uses FUSE which is Linux-only, on Windows you need to use **WSL2 (Windows Subsystem for Linux)**.

## Prerequisites

1.  **Windows 10 version 2004+ or Windows 11**
2.  **WSL2 installed** - Follow official guide: https://learn.microsoft.com/en-us/windows/wsl/install
3.  Quick install command (in PowerShell as Admin):
    ```powershell
    wsl --install
