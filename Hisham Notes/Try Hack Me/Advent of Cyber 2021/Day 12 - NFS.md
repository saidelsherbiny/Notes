# Day 12 - NFS

## What is NFS?
- Network File System (NFS) is a protocol that allows the ability to transfer files between different computers and is available on many systems, including MS Windows and Linux.

## Usage
1. List available shares
```
showmount -e MACHINE_IP // instead of -e --exports
```

2. Mount share on attacker machine after creating directory (tmp1)
`sudo mount MACHINE_IP:/my-notes tmp1`

