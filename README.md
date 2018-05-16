## 项目描述
图片作为App中重要的一个元素，非常具有表现力，图片既要让用户能看清楚，又能让发布图片的用户能快速的上传。所以开发者要对图片进行裁切和质量压缩。但是裁切尺寸质量压缩比设置成多少却很难控制好，如果设置不当会导致图片显示效果很差。

微信里面的图片压缩是一个很好的参照物，被大家广为使用并接受。这个扩展就是通过朋友圈和会话发送了大量图片，对比原图与微信压缩后的图片逆向推算出来的压缩策略。

## Requirements
* iOS 8.0+ | macOS 10.10+ | tvOS 9.0+ | watchOS 2.0+
* Xcode 8

## Integration
CocoaPods (iOS 8+, OS X 10.9+)
```ruby
platform :ios, '8.0'
use_frameworks!

target 'MyApp' do
pod 'WXImageCompress', '~> 0.1.1'
end
```


## Usage
```swift
import WXImageCompress
```
```swift
let image = UIImage(named: "imageName")!
```
```swift
let thumbImage = image.wxCompress()
```


## Effect comparison
| original | wechat | this |
| --------   | -----   | ---- |
| 1500 * 4000,  2.5MB | 800 * 2134, 325KB | 800 * 2134, 306KB |
| 960 * 600,    210KB | 960 * 600, 147KB | 960 * 600, 147KB |
| 800 * 1280,   595KB | 800 * 1280, 140KB | 800 * 1280, 142KB |
| 1080 * 1920,  1.8MB | 720 * 1280, 139KB | 720 * 1280, 140KB |
| 640 * 1136,   505KB | 640 * 1136, 68KB | 640 * 1136 69KB |
| 4000 * 3000,  497KB | 1280 * 960, 140KB | 1280 * 960, 139KB |
| 2560 * 1600,  232KB | 1280 * 800 112KB | 1280 * 800, 112KB |
| 800 * 2138,   307KB | 800 * 2134, 649KB | 800 * 2138, 599KB |
| 3351 * 1430,  386KB | 1874 * 800, 296KB | 1875 * 800, 286KB |
| 3000 * 1300,   458KB | 1846 * 800 322KB | 1847 * 800, 307KB |
| 8323 * 5793,  19.67MB | 1280 * 890, 428KB | 1280 * 891, 465KB |


## 策略
### 图片尺寸
* 宽高均 <= 1280，图片尺寸大小保持不变
* 宽或高   >  1280 && 宽高比 <= 2，取较大值等于1280，较小值等比例压缩
* 宽或高   >  1280 && 宽高比  > 2 && 宽或高 < 1280，图片尺寸大小保持不变
* 宽高均 >  1280 && 宽高比  > 2，取较小值等于1280，较大值等比例压缩

```
注：当宽和高均小于1280，并且宽高比大于2时，微信聊天会话和微信朋友圈的处理不一样。
朋友圈：取较小值等于1280，较大值等比例压缩
聊天会话：取较小值等于800，较大值等比例压缩
```
### 图片质量
经过大量的测试，微信的图片压缩质量值 ≈0.5 

```swift
UIImageJPEGRepresentation(resizeImage, 0.5)
```
