---
title: iOS 设置手机常亮
date: 2021-06-10 10:48:34
tags:
---

使用 UIApplication 中的 idleTimerDisabled 属性来设置手机常亮，代码如下：

```objc
[UIApplication sharedApplication].idleTimerDisabled = YES;
```

`idleTimerDisabled` 默认为 NO， 即代表未开启屏幕常亮，设置为 YES 即可开启屏幕常量，值得注意的是，当程序进入后台，无论当前 APP 的 `idleTimerDisabled` 值是什么，都会关闭屏幕常亮。

还有如果当前页面有播放音乐或者视频，当播放停止后，系统会自动关闭屏幕常亮，因此在需要屏幕常亮的页面对 idleTimerDisabled 进行 KVO，一旦系统关闭屏幕常亮，可立即打开。代码如下：

```objc
[[UIApplication sharedApplication] addObserver:self forKeyPath:@"idleTimerDisabled" options:NSKeyValueObservingOptionNew context:nil];

- (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary *)change context:(void*)context
{
    if(![UIApplication sharedApplication].idleTimerDisabled) {
        [UIApplication sharedApplication].idleTimerDisabled = YES;
    }
}
```

APP 内其他页面不需要开启屏幕常亮，可在离开页面的时候关闭即可

```
[UIApplication sharedApplication].idleTimerDisabled = NO;
```