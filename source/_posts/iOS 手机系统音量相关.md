---
title: iOS 手机系统音量相关
date: 2021-05-10 10:51:57
tags:
---

注意：设置或者获取音量可能很多地方都用的到，因此可以把下面的方法写在最 `appdelegate` 中

## 一、设置音量相关

**注意：**

设置音量大小,如果不在播放音乐的时候设置这个的话可能会直接设置铃声，而且设置的值可能设置不上，如果要在app刚刚启动的时候设置音量，需要将AVAudioSession设置为活跃状态:

```
[[AVAudioSession sharedInstance]setActive:YES error:error];
```

### 1.1 获取MPVolumeView

```
- (MPVolumeView *)volumeView
{
    if (!_volumeView) {
        _volumeView = [[MPVolumeViewalloc] initWithFrame:CGRectMake(-100, -100, 40, 40)];
        // 将x、y设置成 -100 就会将音量界面隐藏
        // 下面两行代码都会使音量界面重新显示
        // [_volumeView setHidden:YES];
        // [_volumeView removeFromSuperview];
        [self.window addSubview:_volumeView];
    }
    return _volumeView;
}
```

### 1.2、 获取MPVolumeView上面的滑条

```
- (UISlider*)volumeSlider
{
    UISlider* volumeSlider =nil;
    for(UIView*viewin[self.volumeViewsubviews]) {
        if ([view.class.description isEqualToString:@"MPVolumeSlider"]){
            volumeSlider = (UISlider*)view;
            break;
        }
    }
    returnvolumeSlider;
}
```

### 1.3、设置大小：


```
// 参数值范围：0~100
- (void)setVolume:(float)value
{
    self.volumeSlider = [self volumeSlider];
    // 需要设置 showsVolumeSlider 为 YES
    self.volumeView.showsVolumeSlider = YES;
    [self.volumeSlidersetValue:value animated:NO];
    [self.volumeSlider sendActionsForControlEvents:UIControlEventTouchUpInside];
    [self.volumeView sizeToFit];
}
```

## 二、获取音量大小

> 首先你需要导入 #import <MediaPlayer/MediaPlayer.h>


**注意：**

在 app 刚刚初始化的时候使用 MPVolumeView 获取音量大小可能为 0，因此使用`[[AVAudioSession sharedInstance]outputVolume]`，使用 AVAudioSession 需要导入头文件 `#import <AVFoundation/AVFoundation.h>`

```
- (float)getVolume
{
    return self.volumeSlider.value > 0 ? self.volumeSlider.value : [[AVAudioSession sharedInstance] outputVolume];
}
```

## 三、监听系统音量的变化

### 3.1、使用通知监听

相关的通知名称：

> 系统音量变化通知名称

**@"AVSystemController_SystemVolumeDidChangeNotification"**

> 接收到通知之后获取音量的值：

notification.info[@"AVSystemController_AudioVolumeNotificationParameter"]

具体使用如下：

```
[[NSNotificationCenter defaultCenter] addObserver:self
                                         selector:@selector(systemVolumeDidChangeNoti:)
                                             name:@"AVSystemController_SystemVolumeDidChangeNotification"
                                           object:nil];

- (void)systemVolumeDidChangeNoti:(NSNotification* )noti
{
    // 当前手机音量
    float volume = [[noti.userInfo valueForKey:@"AVSystemController_AudioVolumeNotificationParameter"] floatValue];
}
```

## 3.2销毁监听

```
- (void)dealloc
{
    [[NSNotificationCenter defaultCenter]removeObserver:self];
}
```
