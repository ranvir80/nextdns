# Windows Blue Screen / Boot Repair Guide (Advanced Mode)

This guide provides the correct and safe order of commands to repair:
- Blue Screen of Death (BSOD)
- Boot loop / Startup failure
- Corrupted system files
- Damaged boot records (MBR / BCD / EFI)

Use this from **Windows Recovery → Advanced Options → Command Prompt**.

---

## ⚠️ Requirements

- Windows Recovery Environment (WinRE)
- Administrator access
- Windows installed on `C:` (verify if unsure)

---

## STEP 0 — Verify Windows Drive (Optional)

```bat
diskpart
list vol
exit
```

Check which volume contains the `Windows` folder.

---

## STEP 1 — Disk Check & Repair

```bat
chkdsk C: /f /r
```

Fixes file system errors and bad sectors.

---

## STEP 2 — Repair Windows Image

```bat
DISM /Online /Cleanup-Image /RestoreHealth
```

Repairs corruption in the Windows component store.

---

## STEP 3 — Repair System Files

```bat
sfc /scannow
```

Scans and repairs corrupted Windows files.

---

## STEP 4 — Repair Boot Records

```bat
bootrec /fixmbr
bootsect /nt60 sys
bootrec /fixboot
bootrec /scanos
bootrec /rebuildbcd
```

### If `/fixboot` gives "Access Denied":

```bat
bootsect /nt60 all /force
bootrec /fixboot
```

---

## STEP 5 — Rebuild EFI Boot (Only if still not booting)

```bat
diskpart
list vol
sel vol X       ← EFI volume (FAT32, 100–300MB)
assign letter=Z
exit
bcdboot C:\Windows /s Z: /f UEFI
```

---

## STEP 6 — Exit and Restart

```bat
exit
```

Restart the system normally.

---

## Summary

| Step | Purpose | Command |
|------|----------|----------|
| 1 | Disk repair | `chkdsk` |
| 2 | Image repair | `DISM` |
| 3 | File repair | `sfc` |
| 4 | Boot repair | `bootrec`, `bootsect` |
| 5 | EFI rebuild | `bcdboot` |

---

## Notes

- Use STEP 5 only on UEFI systems.
- Legacy BIOS systems usually do not need EFI rebuild.
- Always complete each step before proceeding.

---

## Author
Ranvir Pardeshi
