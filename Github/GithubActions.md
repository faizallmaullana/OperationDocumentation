# Github Syntax

## Flush DNS

```bash
Windows: ipconfig /flushdns
macOS: sudo dscacheutil -flushcache; sudo killall -HUP mDNSResponder
```

## Pindah Branch

```bash
git checkout -b [branch_name] [remote_name]/[branch_name]
```

penjelasan

- git checkout -b : membuat branch baru
- [branch_name] : nama branch yang akan dibuat
- [remote_name]/[branch_name] : nama branch yang akan diambil dari remote
