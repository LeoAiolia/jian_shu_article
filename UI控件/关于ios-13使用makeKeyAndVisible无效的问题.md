如果在iOS13之前，你是这样使用弹框
```
let window = UIWindow(frame: UIScreen.main.bounds)
let viewController = UIViewController()
viewController.view.backgroundColor = .clear
window.rootViewController = viewController
window.windowLevel = UIWindow.Level.statusBar + 1
window.makeKeyAndVisible()
```
那么在iOS13时，你会发现UI界面并没有任何反应，之所以无效，是因为你使用了iOS 13 的SceneDelegate，

此时你需要使用下面这种方式来创建window
```
let windowScene = UIApplication.shared
                .connectedScenes
                .filter { $0.activationState == .foregroundActive }
                .first
if let windowScene = windowScene as? UIWindowScene {
    // 该window是全局变量
    window = UIWindow(windowScene: windowScene)
    window?.frame = UIScreen.main.bounds
    window?.backgroundColor = .clear
}
```
之后展示的时候
```
window?.windowLevel = UIWindow.Level.statusBar + 1
window?.rootViewController = viewController
window?.makeKeyAndVisible()
```
此时你会发现makeKeyAndVisible又起作用了。。。

另：
在iOS13中，keyWindow已被废弃，若使用了SceneDelegate, 则在iOS13上获取会得到nil，可以使用
```
UIApplication.shared.windows.first
```
或
```
    var window: UIWindow?
    if #available(iOS 13.0, *) {
          window = (UIApplication.shared.connectedScenes.first?.delegate as? SceneDelegate)?.window
    } else {
          window = UIApplication.shared.keyWindow
    }
```
