# Post installation: fill SMBIOS info

- open `EFI/CLOVER/config.plist` with Clover Configurator
- generate SMBIOS info with `iMac19,1`. if you use eGPU, select `iMac19,2` instead.
  ![SMBIOS-0](./screenshots/smbios-0.png)
- Actually, we need 4 values only, delete others if you want.
  ```xml
  <key>BoardSerialNumber</key>
  <string>XXXX</string>
  <key>ProductName</key>
  <string>iMac18,1</string>
  <key>SerialNumber</key>
  <string>XXXX</string>
  <key>SmUUID</key>
  <string>XXXX</string>
  ```
