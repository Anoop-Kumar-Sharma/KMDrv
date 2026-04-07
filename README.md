# KMDrv - Kernel Mode Memory Driver

## Overview

**KMDrv** is a Windows kernel-mode driver that provides basic
functionality for interacting with the virtual memory of user-mode
processes. It allows attaching to a process and performing read/write
operations on its memory using IOCTL communication.

This project demonstrates low-level usage of Windows NT kernel APIs such
as `MmCopyVirtualMemory` and `PsLookupProcessByProcessId`.

------------------------------------------------------------------------

## Features

-   Attach to a target process via PID
-   Read memory from a target process
-   Write memory to a target process
-   Uses buffered IOCTL communication
-   Safe probing with `ProbeForRead` / `ProbeForWrite`

------------------------------------------------------------------------

## File Structure

    .
    ├── driver.c      # Main driver implementation
    └── imports.h     # Structures, enums, and external declarations

------------------------------------------------------------------------

## IOCTL Interface

### Request Structure

``` c
typedef struct _Request {
    HANDLE pid;
    PVOID target;
    PVOID buffer;
    SIZE_T size;
    SIZE_T return_size;
} Request, *PRequest;
```

### Supported IOCTL Codes

  Code     Description
  -------- --------------------------
  attach   Attach to target process
  read     Read memory from process
  write    Write memory to process

------------------------------------------------------------------------

## How It Works

### Driver Initialization

-   Entry point: `DriverEntry`
-   Creates:
    -   Device: `\Driver\KMDrv`
    -   Symbolic link: `\DosDevices\KMDrv`

------------------------------------------------------------------------

### Memory Operations

-   Uses `MmCopyVirtualMemory` for read/write
-   Uses `PsLookupProcessByProcessId` to resolve processes

------------------------------------------------------------------------

## Usage (User Mode)

1.  Open handle:


```cpp
CreateFile("\\.\KMDrv", ...)
```

2.  Send IOCTL:


```cpp
DeviceIoControl(...)
```
------------------------------------------------------------------------

## Important Notes

-   Kernel-mode driver (can crash system)
-   No persistent process context
-   Minimal validation

------------------------------------------------------------------------

## Disclaimer

Educational purposes only.
