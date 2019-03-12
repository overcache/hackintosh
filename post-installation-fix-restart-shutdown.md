# Post installation: fix restart and shutdown

With `AptioMemoryFix-64.efi`, system can boot, but when you restart or shutdown, kernel panic happened. To fix those issues, replace `AptioMemoryFix-64.efi` with [OsxAptioFix2Drv-free2000.efi](./kexts/OsxAptioFix2Drv-free2000.efi) (sorry, I can't find any official download links)

notes: I tested `OsxAptioFixDrv-64`, `OsxAptioFix3Drv-64`, `OsxLowMemFixDrv-64`, none of them boots.
