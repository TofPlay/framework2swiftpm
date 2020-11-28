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
## Example with VOGO Framework
First version with the VOGO Frameworks 5.0.6:
```bash
#!/bin/bash
framework2swiftpm --framework=vogolib_iOS_r5.0.6/5.0.6/devices/VOGOPlayerCore.framework,armv7,arm64 \
                  --framework=vogolib_iOS_r5.0.6/5.0.6/devicesAndSimulator/VOGOPlayerCore.framework,i386,x86_64 \
                  --name=VOGOPlayerCore \
                  --swiftpm \
                  --version=5.0.6 \
                  --cdn=https://cdn.company.com \
                  --git

framework2swiftpm --framework=vogolib_iOS_r5.0.6/5.0.6/devices/VOGOPlayerUI.framework,armv7,arm64 \
                  --framework=vogolib_iOS_r5.0.6/5.0.6/devicesAndSimulator/VOGOPlayerUI.framework,i386,x86_64 \
                  --name=VOGOPlayerUI \
                  --swiftpm \
                  --version=5.0.6 \
                  --cdn=https://cdn.company.com \
                  --git
```
Second version with the VOGO Frameworks 5.0.7:
```bash
#!/bin/bash
framework2swiftpm --framework=vogolib_iOS_r5.0.7/5.0.7/devices/VOGOPlayerCore.framework,armv7,arm64 \
                  --framework=vogolib_iOS_r5.0.7/5.0.7/devicesAndSimulator/VOGOPlayerCore.framework,i386,x86_64 \
                  --name=VOGOPlayerCore \
                  --swiftpm \
                  --version=5.0.7 \
                  --cdn=https://cdn.company.com 

framework2swiftpm --framework=vogolib_iOS_r5.0.7/5.0.7/devices/VOGOPlayerUI.framework,armv7,arm64 \
                  --framework=vogolib_iOS_r5.0.7/5.0.7/devicesAndSimulator/VOGOPlayerUI.framework,i386,x86_64 \
                  --name=VOGOPlayerUI \
                  --swiftpm \
                  --version=5.0.7 \
                  --cdn=https://cdn.company.com 
```
Latest version of SwiftPM package for VOGOPlayerCore version 5.0.7:
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
XCFrameworks on the CDN (root: https://cdn.company.com):
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

### Repo Git
![PastedGraphic-0](https://user-images.githubusercontent.com/1082222/100499199-e0edb780-3167-11eb-996f-7d2c0715303c.png)

### Use with Xcode
![PastedGraphic-1](https://user-images.githubusercontent.com/1082222/100499252-4346b800-3168-11eb-84f3-c62081497267.png)
![PastedGraphic-2](https://user-images.githubusercontent.com/1082222/100499255-522d6a80-3168-11eb-88fa-7a4aecd7a2b3.png)
![PastedGraphic-3](https://user-images.githubusercontent.com/1082222/100499302-b6e8c500-3168-11eb-9c58-b22cee67475e.png)
![PastedGraphic-4](https://user-images.githubusercontent.com/1082222/100499271-77ba7400-3168-11eb-8a37-a0a05fb2849d.png)
