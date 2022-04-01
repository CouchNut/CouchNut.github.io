---
title: Swift 判断设备 VPN 是否打开
date: 2021-04-27 16:09:53
tags: Swift
---

Swift 判断设备VPN是否打开，各种 `VPN` 类型 **（tap、tun、ppp、ipsec、ipsec0）** 都可判断。

代码如下：

```swift
/// 判断VPN是否打开
@objc static var isVPNOpen: Bool {
    guard let cfDict = CFNetworkCopySystemProxySettings()
    else {
        return false
    }

    let nsDict = cfDict.takeRetainedValue() as NSDictionary

    guard let keys = nsDict["__SCOPED__"] as? [String:Any]
    else {
        return false
    }

    let keyValues = [
        "tap",
        "tun",
        "ppp",
        "ipsec",
        "ipsec0",
    ]

    var result = false
    for key in keys.keys {
        keyValues.forEach { (value) in
            if key.contains(value) {
                result = true
            }
        }
    }

    return result
}
```

以上。