LFLiveKit
==============


<==========================================================
推流端,这里我才用的 开源的推流框架,开源的iOS推流框架LFLiveKit. 是用OC写的, 很适合学习集成也非常简单, 几句代码就OK了,LFLiveKit已经集成了GPUImage, 如果项目中有集成GPUImage, 需要将之前的移除掉. 且集成LFLiveKit需要关闭Bitcode
拉流端 主要是基于ijkplayer(https://github.com/Bilibili/ijkplayer)的. 最好是打包成framework.

@#import /IJKMediaFramework/IJKMediaFramework.h>
@interface ViewController : UIViewController 
@property (atomic, strong) NSURL *url;
@property (atomic, retain) id /IJKMediaPlayback> player;
@property (weak, nonatomic) IBOutlet UIView *PlayerView;
@property (weak, nonatomic) IBOutlet UIButton *playButton;
然后就通过初试化一些基本设置就可以播放了： 
1、url 
2、PlayView 
3、notification 
4、prepareToPlay

简单把界面上的东西部署一下，url等初始化

self.url = [NSURL URLWithString:@"http://live.hkstv.hk.lxdns.com/live/hks/playlist.m3u8"];

    _player = [[IJKFFMoviePlayerController alloc] initWithContentURL:self.url withOptions:nil];
    UIView *playerView = [_player view];
    playerView.frame = self.PlayerView.frame;
    playerView.autoresizingMask = UIViewAutoresizingFlexibleWidth | UIViewAutoresizingFlexibleHeight;

    [self.PlayerView insertSubview:playerView atIndex:1];
    [_player setScalingMode:IJKMPMovieScalingModeAspectFill];
    [self installMovieNotificationObservers];

别忘记了这个

[self.player prepareToPlay];

最后在你需要的地方调用播放等接口

 [self.player play];
===========================================================>



![icon~](https://raw.github.com/LaiFengiOS/LFLiveKit/master/samples/Icon.png)


[![Build Status](https://travis-ci.org/LaiFengiOS/LFLiveKit.svg)](https://travis-ci.org/LaiFengiOS/LFLiveKit)&nbsp;
[![License MIT](https://img.shields.io/badge/license-MIT-green.svg?style=flat)](https://raw.githubusercontent.com/chenliming777/LFLiveKit/master/LICENSE)&nbsp;
[![CocoaPods](http://img.shields.io/cocoapods/v/LFLiveKit.svg?style=flat)](http://cocoapods.org/?q=LFLiveKit)&nbsp;
[![Support](https://img.shields.io/badge/ios-7-orange.svg)](https://www.apple.com/nl/ios/)&nbsp;
![platform](https://img.shields.io/badge/platform-ios-ff69b4.svg)&nbsp;


**LFLiveKit is a opensource RTMP streaming SDK for iOS.**  

## Features

- [x] 	Background recording
- [x] 	Support horizontal vertical recording
- [x] 	Support Beauty Face With GPUImage
- [x] 	Support H264+AAC Hardware Encoding
- [x] 	Drop frames on bad network 
- [x] 	Dynamic switching rate
- [x] 	Audio configuration
- [x] 	Video configuration
- [x] 	RTMP Transport
- [x] 	Switch camera position
- [x] 	Audio Mute
- [x] 	Support Send Buffer
- [x] 	Support WaterMark
- [x] 	Swift Support
- [x] 	Support Single Video or Audio 
- [x] 	Support External input video or audio(Screen recording or Peripheral)
- [ ] 	~~FLV package and send~~

## Requirements
    - iOS 7.0+
    - Xcode 7.3
  
## Installation

#### CocoaPods
	# To integrate LFLiveKit into your Xcode project using CocoaPods, specify it in your Podfile:

	source 'https://github.com/CocoaPods/Specs.git'
	platform :ios, '7.0'
	pod 'LFLiveKit'
	
	# Then, run the following command:
	$ pod install


#### Carthage
    1. Add `github "LaiFengiOS/LFLiveKit"` to your Cartfile.
    2. Run `carthage update --platform ios` and add the framework to your project.
    3. Import \<LFLiveKit/LFLiveKit.h\>.


#### Manually

    1. Download all the files in the `LFLiveKit` subdirectory.
    2. Add the source files to your Xcode project.
    3. Link with required frameworks:
        * UIKit
        * Foundation
        * AVFoundation
        * VideoToolbox
        * AudioToolbox
        * libz
        * libstdc++
	
## Usage example 

#### Objective-C
```objc
- (LFLiveSession*)session {
	if (!_session) {
	    _session = [[LFLiveSession alloc] initWithAudioConfiguration:[LFLiveAudioConfiguration defaultConfiguration] videoConfiguration:[LFLiveVideoConfiguration defaultConfiguration]];
	    _session.preView = self;
	    _session.delegate = self;
	}
	return _session;
}

- (void)startLive {	
	LFLiveStreamInfo *streamInfo = [LFLiveStreamInfo new];
	streamInfo.url = @"your server rtmp url";
	[self.session startLive:streamInfo];
}

- (void)stopLive {
	[self.session stopLive];
}

//MARK: - CallBack:
- (void)liveSession:(nullable LFLiveSession *)session liveStateDidChange: (LFLiveState)state;
- (void)liveSession:(nullable LFLiveSession *)session debugInfo:(nullable LFLiveDebug*)debugInfo;
- (void)liveSession:(nullable LFLiveSession*)session errorCode:(LFLiveSocketErrorCode)errorCode;
```
#### Swift
```swift
// import LFLiveKit in [ProjectName]-Bridging-Header.h
#import <LFLiveKit.h> 

//MARK: - Getters and Setters
lazy var session: LFLiveSession = {
	let audioConfiguration = LFLiveAudioConfiguration.defaultConfiguration()
	let videoConfiguration = LFLiveVideoConfiguration.defaultConfigurationForQuality(LFLiveVideoQuality.Low3, landscape: false)
	let session = LFLiveSession(audioConfiguration: audioConfiguration, videoConfiguration: videoConfiguration)
	    
	session?.delegate = self
	session?.preView = self.view
	return session!
}()

//MARK: - Event
func startLive() -> Void { 
	let stream = LFLiveStreamInfo()
	stream.url = "your server rtmp url";
	session.startLive(stream)
}

func stopLive() -> Void {
	session.stopLive()
}

//MARK: - Callback
func liveSession(session: LFLiveSession?, debugInfo: LFLiveDebug?) 
func liveSession(session: LFLiveSession?, errorCode: LFLiveSocketErrorCode)
func liveSession(session: LFLiveSession?, liveStateDidChange state: LFLiveState)
```

## Release History
    * 2.0.0
        * CHANGE: modify bugs,support ios7 live.
    * 2.2.4.3
        * CHANGE: modify bugs,support swift import.
    * 2.5 
        * CHANGE: modify bugs,support bitcode.


## License
 **LFLiveKit is released under the MIT license. See LICENSE for details.**




