# PXE Boot Troubleshooting: COM32 Module Load Failure

## 🚨 Problem

During a Legacy PXE boot:

- DHCP succeeds  
- `pxelinux.0` loads  
- The client shows repeated errors:

```
Failed to load libutil.c32
Failed to load COM32 file menu.c32
```

- PXE menu never appears  
- System loops back to error  

---

## 🔍 Root Cause

This happens when:

- Required Syslinux **COM32 modules are missing**, OR  
- Files were copied from **different Syslinux versions**

⚠️ All PXELINUX files **must come from the same Syslinux package version**.

Mixing versions causes COM32 load failures.

---

## ✅ Solution (Step-by-Step)

## Step 1 — Install Syslinux

```bash
dnf install -y syslinux
```

---

## Step 2 — Copy Required PXELINUX Files

Copy ALL files from the **same directory**:

```bash
cp /usr/share/syslinux/pxelinux.0 /var/lib/tftpboot/
cp /usr/share/syslinux/menu.c32 /var/lib/tftpboot/
cp /usr/share/syslinux/libutil.c32 /var/lib/tftpboot/
cp /usr/share/syslinux/libcom32.c32 /var/lib/tftpboot/
cp /usr/share/syslinux/ldlinux.c32 /var/lib/tftpboot/
```

⚠️ **Important:**  
Do NOT mix files from different Syslinux directories or versions.

---

## Step 3 — Verify TFTP Directory

```bash
ls /var/lib/tftpboot
```

Minimum required files:

- `pxelinux.0`
- `menu.c32`
- `libutil.c32`
- `libcom32.c32`
- `ldlinux.c32`
- `pxelinux.cfg/`
- `rocky8/`



## 🔎 How to Confirm It’s Fixed

### Verify COM32 Modules

```bash
ls /var/lib/tftpboot/*.c32
```

**Expected Output:**

```
/var/lib/tftpboot/menu.c32
/var/lib/tftpboot/libutil.c32
/var/lib/tftpboot/libcom32.c32
/var/lib/tftpboot/ldlinux.c32
```

If any of these files are missing, copy them again from the same Syslinux package.

---

### Verify PXELINUX Config

```bash
ls /var/lib/tftpboot/pxelinux.cfg/default
```

**Expected Output:**

```
/var/lib/tftpboot/pxelinux.cfg/default
```

If the file does not exist, create or restore your PXE configuration file before testing.

---


## 🖥 Client-Side Verification

Reboot the PXE client.

A successful fix should show:

1. Intel Boot Agent loads  
2. PXELINUX starts successfully  
3. PXE menu appears  
4. Kernel and initrd load  
5. Installer starts (no COM32 errors)

---

## 📌 Key Takeaways

- `pxelinux.0` alone is NOT enough  
- `menu.c32` depends on:
  - `libutil.c32`
  - `libcom32.c32`
- All files must come from the same Syslinux version  
- COM32 errors are **file/version issues**, not DHCP/network problems  

---

## 📝 One-Line Summary

COM32 load errors during PXE boot mean missing or mismatched Syslinux modules — copy all required `.c32` files from the same Syslinux package to fix it.
