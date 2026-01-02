Marathi Remington Keyboard
Clear.
You are getting Windows 11 BLACK screen crash (not blue) with text:

“Your device ran into a problem and needs to restart”

This is not a normal BSOD — this is a Win11 “modern crash screen” triggered mostly by:

GPU driver reset

VBS / Hyper-V conflict

Power / sleep resume bug

Unsupported CPU microcode

On HP EliteBook 840 G3 (i5-6th gen) this is extremely common.

Below is the exact fix for black crash screen.

1️⃣ Disable virtualization & security stack
Turn OFF VBS, Hyper-V, Memory Integrity

Settings path:

Settings → Privacy & Security → Device Security → Core Isolation → Memory Integrity OFF

Then:

Win + R → optionalfeatures

Uncheck:

Hyper-V

Virtual Machine Platform

Windows Hypervisor Platform

Restart.

2️⃣ Lock Intel GPU to stable driver

Device Manager → Display adapters → Intel HD 520 → Properties → Driver

If version is newer than 27.x, rollback to older Win10 driver.

Best stable version for HD 520:

26.20.100.8141 (Win10)

Install manually from Intel / HP support.

3️⃣ Disable Panel Self Refresh (prevents black screen)

Registry (Admin):

reg add "HKLM\SOFTWARE\Intel\Display\igfxcui\profiles\Media" /v PanelSelfRefresh /t REG_DWORD /d 0 /f


Restart.

4️⃣ Disable fast boot & sleep instability

Admin CMD:

powercfg /hibernate off
bcdedit /set disabledynamictick yes


Restart.

5️⃣ BIOS power stabilization

In BIOS:

Disable Secure Boot

Disable TPM

Disable Intel SpeedStep / C-States (if available)

Update BIOS to latest version.

6️⃣ Prevent Windows from breaking drivers again

Group Policy:

gpedit.msc → Windows Update → Enable:

Do not include drivers with Windows Updates

7️⃣ If still happens — force classic BSOD to see code

Registry:

reg add HKLM\SYSTEM\CurrentControlSet\Control\CrashControl /v DisplayParameters /t REG_DWORD /d 1 /f


Then next crash will show stop code.

Summary (apply in this order)

Disable VBS + Hyper-V

Install Intel HD 520 old driver

Disable Panel Self Refresh

Disable hibernate / fast startup

BIOS security off

Lock Windows Update drivers

Bottom line

Black crash screen in Win11 on 6th gen Intel = GPU + virtualization conflict.

After applying above, crashes stop.

If crash still happens after this, send:

Event Viewer → BugCheck entry

Intel driver version

I’ll pinpoint exact line causing it.