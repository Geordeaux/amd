---
id: ios-module-guide-2
title: ios module guide 2
hide_title: false
---

`Bundle/Reusable/UIImage` 在模块中都需要进行改造
# Bundle
创建module的bundle
```
class LiteBundle {

}

extension Bundle {
    static let lite: Bundle = {
        let bundle = Bundle(for: LiteBundle.self)

        guard let path = bundle.path(forResource: "BinanceLite", ofType: "bundle") else {
            return bundle
        }

        guard let liteBundle = Bundle(path: path) else {
            return bundle
        }

        return liteBundle
    }()

}
```
# Image
图片需要从module 的bundle 中取
```
extension UIImage {
    class func image(with named: String, in bundle: Bundle? = .lite) -> UIImage? {
        guard !named.isEmpty else { return nil }
        if #available(iOS 13.0, *) {
            return UIImage(named: named, in: bundle, with: nil)
        } else {
            return UIImage(named: named, in: bundle, compatibleWith: nil)
        }
    }

    class func themedImage(with name: String) -> UIImage? {
        guard !name.isEmpty else { return nil }
        switch Themes.current.type {
        case .light:
            return image(with: name + "-light")
        case .dark:
            return image(with: name + "-dark")
        }
    }
}
```

# Reusable
由于`Reusable`默认都是取的主工程bundle 里的资源，所以这里需要处理一下
如使用到了`NibLoadable`
继承自`NibLoadable`重写`nib`方法即可
```
protocol LiteNibLoadable: NibLoadable { }

extension LiteNibLoadable {
    public static var nib: UINib {
        return UINib(nibName: String(describing: self), bundle: .lite)
    }
}

typealias LiteNibReusable = Reusable & LiteNibLoadable

```
`StoryboardBased` 也同理，继承自`StoryboardBased`，重写`nib`方法
```
protocol LiteStoryboardBased: StoryboardBased { }

extension LiteStoryboardBased {
    public static var sceneStoryboard: UIStoryboard {
        return UIStoryboard(name: String(describing: self), bundle: .lite)
    }
}
```