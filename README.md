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

Example with the VOGO Frameworks 5.0.7:
```bash
#!/bin/bash
framework2swiftpm --framework=vogolib_iOS_r5.0.7/5.0.7/devices/VOGOPlayerCore.framework,armv7,arm64 \
                  --framework=vogolib_iOS_r5.0.7/5.0.7/devicesAndSimulator/VOGOPlayerCore.framework,i386,x86_64 \
                  --name=VOGOPlayerCore \
                  --swiftpm \
                  --version=5.0.7 \
                  --cdn=https://cdn.company.com \
                  --git

framework2swiftpm --framework=vogolib_iOS_r5.0.7/5.0.7/devices/VOGOPlayerUI.framework,armv7,arm64 \
                  --framework=vogolib_iOS_r5.0.7/5.0.7/devicesAndSimulator/VOGOPlayerUI.framework,i386,x86_64 \
                  --name=VOGOPlayerUI \
                  --swiftpm \
                  --version=5.0.7 \
                  --cdn=https://cdn.company.com \
                  --git
```
SwiftPM package for VOGOPlayerCore version 5.0.7:
```swift
// swift-tools-version:5.3
import PackageDescription

let package = Package(
    name: "VOGOPlayerCore",
    platforms: [
        .macOS(.v10_14), .iOS(.v12)
    ],
    products: [
        // Products define the executables and libraries a package produces, and make them visible to other packages.
        .library(
            name: "VOGOPlayerCore",
            targets: ["VOGOPlayerCore"])
    ],
    dependencies: [
        // Dependencies declare other packages that this package depends on.
    ],
    targets: [
        // Targets are the basic building blocks of a package. A target can define a module or a test suite.
        // Targets can depend on other targets in this package, and on products in packages this package depends on.
        .binaryTarget(
            name: "VOGOPlayerCore",
            url: "https://cdn.company.com/VOGOPlayerCore/5.0.7/VOGOPlayerCore.xcframework.zip",
            checksum: "bbf1e16d26c2e9d7ea6c356ca5e08f12d81bb5896d2309677f2c79b08cbdf9a4"
        )
    ]
)
```

XCFrameworks on the CDN:
```
├── VOGOPlayerCore
│   ├── 5.0.6
│   │   └── VOGOPlayerCore.xcframework.zip
│   └── 5.0.7
│       └── VOGOPlayerCore.xcframework.zip
└── VOGOPlayerUI
    ├── 5.0.6
    │   └── VOGOPlayerUI.xcframework.zip
    └── 5.0.7
        └── VOGOPlayerUI.xcframework.zip
```
