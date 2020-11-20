# framework2swiftpm
Bash script to generate an XCFramework and a SwiftPM package from a binary framework

```
$ framework2swiftpm

This script is used to generate an XCFramework from a framework.
It can also generate the binary package for SwiftPM.

Syntax:
    ./framework2swiftpm --framework=<framework path>,i386,x86_64 \
                        --framework=<framework path>,armv7,arm64 \
                        --name=<XCFramework name> \
                        --swiftpm \
                        --version=<version> \
                        --cdn=<url> \
                        --macOS=<macOS version> \
                        --iOS=<iOS version> \
                        --git

    Required:
        --framework: Framework path followed by the list of architectures to extract
        --name: XCFramework name without extension "xcframework"

    Optionals:
        --swiftpm: Will generate the Package.swift file for SwiftPM by default "false"
        --version: Version of the XCFramework by default empty
        --cdn: Url of the CDN where the XCFramework will be stored by default empty
        --macOS: macOS version for the Package.swift by default ".v10_14"
        --iOS: iOS version for the Package.swift by default ".v12"
        --git: Will prepare a git repo for SwiftPM by default "false"

```

Example with the VOGO Frameworks:
```
#!/bin/bash
framework2swiftpm --framework=vogolib_iOS_r5.0.6/5.0.6/devices/VOGOPlayerCore.framework,armv7,arm64 \
                  --framework=vogolib_iOS_r5.0.6/5.0.6/devicesAndSimulator/VOGOPlayerCore.framework,i386,x86_64 \
                  --name=VOGOPlayerCore \
                  --swiftpm \
                  --version=5.0.6 \
                  --cdn=https://cdn.company.fr \
                  --git

framework2swiftpm --framework=vogolib_iOS_r5.0.6/5.0.6/devices/VOGOPlayerUI.framework,armv7,arm64 \
                  --framework=vogolib_iOS_r5.0.6/5.0.6/devicesAndSimulator/VOGOPlayerUI.framework,i386,x86_64 \
                  --name=VOGOPlayerUI \
                  --swiftpm \
                  --version=5.0.6 \
                  --cdn=https://cdn.company.fr \
                  --git
```
