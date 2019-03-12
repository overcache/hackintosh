# Minimal USB stick clover config explanation

The [minimal-usb-stick-config.plist](./minimal-usb-stick-config.plist) for installation is simple, it's not perfect, but it works and easy to debug. Here is the explanation for each section:

- `Boot` Section. Add below content to config.plist. `-v` argument will print log to screen, `debug=0x100` will prevent autoreboot when kernel panic, `keepsyms=1` will print symbols on kernel panic. All this 3 arguments help us understand what's going on when booting.
```xml
<key>Boot</key>
<dict>
    <key>Arguments</key>
    <string>-v debug=0x100 keepsyms=1</string>
</dict>
```

- `Devices` Section. Must set 'fixOwnership' to true, otherwise we will stuck at 'waiting for booting media'. You can ignore this option if you set your BIOS 'XHCI Hand-off' to 'Enabled'.
```xml
<key>USB</key>
<dict>
    <key>FixOwnership</key>
    <true/>
</dict>
```

- `Graphics` Section. When Clover detects an Intel iGPU, it automatically enables Intel Injection if the Graphics section doesn't exist in the config.plist. We don't need injection to make iGPU works, even worse, we can't boot with injection on. To bypass this, we must explicitly disable injection.
```xml
<key>Graphics</key>
<dict>
    <key>Inject</key>
    <false/>
</dict>
```

- `KernelAndKextPatches` Section. In order to make all usb port works, we need a patch, otherwise will stuck at 'waiting for boot media' if the port which usb stick plugged in doesn't work.
```xml
<key>KernelAndKextPatches</key>
<dict>
    <key>KextsToPatch</key>
    <array>
        <dict>
            <key>Comment</key>
            <string>Port limit increase (Ricky)</string>
            <key>Disabled</key>
            <false/>
            <key>Find</key>
            <data>
            g/sPD4OPBAAA
            </data>
            <key>InfoPlistPatch</key>
            <false/>
            <key>MatchOS</key>
            <string>10.14.x</string>
            <key>Name</key>
            <string>com.apple.driver.usb.AppleUSBXHCI</string>
            <key>Replace</key>
            <data>
            g/sPkJCQkJCQ
            </data>
        </dict>
    </array>
</dict>
```

- `RtVariables` Section. We place kexts in EFI folder, so we don't need to disabled sip. According to [Clover's wiki](https://clover-wiki.zetam.org/Configuration/RtVariables#CsrActiveConfig), SIP disabled default, we don't need any setting in RtVariables.

- `SMBIOS` Section. choose iMac18,1 as ProductName, it doesn't matter whether you have dGPU or not.
```xml
<key>SMBIOS</key>
<dict>
    <key>ProductName</key>
    <string>iMac18,1</string>
</dict>
```

- `SystemParameters` Section. We set `InjectKexts` to Yes to make sure that all the kexts we added before get injected properly.
```xml
<key>SystemParameters</key>
<dict>
    <key>InjectKexts</key>
    <string>Yes</string>
</dict>
```

- What about `AppleRTC`, `AppleIntelCPUPM`, `KernelPm`, `KernelXCPM` etc, all of them are [unnecessary](https://www.tonymacx86.com/threads/pastrychefs-asus-rog-strix-z370-g-gaming-wi-fi-ac-build-w-i9-9900k-amd-vega-56.239969/page-457#post-1865430)
