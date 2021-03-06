---
layout: post
title:  "iOS tips!"
date:   2017-09-06 10:35:21 +0800
categories: jekyll update
---

## 1.  使用storyboard reference
点击需要缩小的控制器 ---> 菜单editor ---> Refactor to Storyboard 

## 2. NSNumberFormatter
可以用来处理NSString和NSNumber之间的转化,可以满足基本的数据形式的转化
>四舍五入到整数

>货币的形式，带本地化的货币符号

>科学计数形式

>本地化拼写形式

## 3. Nil,nil,NULL,NSNull的区别
nil:定义一个对象为空值,例如: NSObject *obj = nil
Nil:定义类为空,例如:Class someClass = Nil
NULL:定义基本数据对象指针为空例如:int *point = NULL
NSNull:结合对象无法包含nil,用特定对象NSNull表示,注意:调用特定方法是注意剔除NSNull对象
-----
## 4.defer
关键字可以用来包裹一段代码，这个代码块将会在当前作用域结束的时候被调用。这通常被用来对当前的代码进行一些清理工作，比如关闭打开的文件等。可以在同一个作用域中指定多个 defer 代码块，在当前作用域结束时，它们会以相反的顺序被调用，即先定义的后执行，后定义的先执行



```
openFile()
defer {
    // defer block 1
    closeFile()
}

startPortListener(42)
defer {
   // defer block 2
   stopPortListener(42)
}
```
这段代码在作用域结束的时候，第二个 defer 块将首先被调用，其次再调用第一个 defer 块。

## 5.associatedtype
swift中的协议(protocol)采用的是“Associated Types”的方式来实现泛型功能的，通过associatedtype关键字来声明一个类型的占位符作为协议定义的一部分。swift的协议不支持下面的定义方式：



```
protocol GeneratorType<Element> {
    public mutating func next() -> Element?
}
而是应该使用这样的定义方式：

protocol GeneratorType {
    associatedtype Element
    public mutating func next() -> Self.Element?
}
```

## 6.值类型和应用类型
在swift中,enum,struct,String,Array,Dictionary都是值类型,
他们之间赋值通过赋值一个新的地址(测试过).在需要处理大量数据并且频繁操作 (增减) 其中元素时，选择 NSMutableArray 和 NSMutableDictionary 会更好，而对于容器内条目小而容器本身数目多的情况，应该使用 Swift 语言内建的 Array 和 Dictionary

## 7.控制台命令expression
```
expr self.loginBtn.backgroundColor = UIColor.blue
expr CATransaction.flush() //立刻刷新界面


```

## 8.正则
要在正则表达式中假如^ 和 $ 表示从头到尾都要符合正则表达式
英文和数字：^[A-Za-z0-9]+$
```
NSRegularExpressionCaseInsensitive             = 1 << 0,     // 不区分大小写字母模式
    NSRegularExpressionAllowCommentsAndWhitespace  = 1 << 1,     // 忽略掉正则表达式中的空格和#号之后的字符
    NSRegularExpressionIgnoreMetacharacters        = 1 << 2,     // 将正则表达式整体作为字符串处理
    NSRegularExpressionDotMatchesLineSeparators    = 1 << 3,     // 允许.匹配任何字符，包括换行符
    NSRegularExpressionAnchorsMatchLines           = 1 << 4,     // 允许^和$符号匹配行的开头和结尾
    NSRegularExpressionUseUnixLineSeparators       = 1 << 5,     // 设置\n为唯一的行分隔符，否则所有的都有效。
    NSRegularExpressionUseUnicodeWordBoundaries    = 1 << 6      // /使用Unicode TR#29标准作为词的边界,否则所有传统正则表达式的词边界都有效
    */

    //创建正则表达式对象
    NSRegularExpression *regex = [NSRegularExpression
                                  regularExpressionWithPattern:pattern
                                  options:NSRegularExpressionCaseInsensitive
                                  error:nil];

NSString *matchString = @"dsampoewaf";

```
    // 2.2 设置进行匹配时的参数===>下面的options
    /*
    NSMatchingReportProgress         = 1 << 0,       // 找到最长的匹配字符串后调用block回调
    NSMatchingReportCompletion       = 1 << 1,       // 找到任何一个匹配串后都回调一次block
    NSMatchingAnchored               = 1 << 2,       // 从匹配范围的开始进行极限匹配
    NSMatchingWithTransparentBounds  = 1 << 3,       // 允许匹配的范围超出设置的范围
    NSMatchingWithoutAnchoringBounds = 1 << 4        // 禁止^和$自动匹配行开始和结束
    */

    // 2.3 设置匹配字符串的范围
    NSRange range = NSMakeRange(0, matchString.length);

    // 2.4 开始匹配
    NSArray<NSTextCheckingResult *> *textCheckingResults = [regex matchesInString:matchString
                                                                          options:NSMatchingReportProgress
                                                                            range:range];


## 9.String去除两端空格和换行符
`str.trimmingCharacters(in: .whitespacesAndNewlines)`

## 10.@discardableResult
@discardableResult :默认情况下编译器就是会去检查返回参数是否有被使用，没有的话就会给出警告。如果你不想要这个警告，可以自己手动加上

## 11.删除Xcode缓存数据
>- 手动删除Derived Data文件夹
>- 调用xcodebuild clean 清空缓存
>- 调用 xcodebuild archive自动忽略缓存

## 12.设置app共享文件功能(可在iTunes -> 应用 ->文件共享 ->目标APP目录下放入文件)
在info.plist中设置Application supports iTunes file sharing(UIFileSharingEnabled)         =         yes

## 13.跨APP传输数据

### UIPasteboard

 在app1中:
```
let borad = UIPasteboard(name: UIPasteboardName(rawValue: "myBorad"), create: true)

borad?.string = "存入数据"
```
app2:
 ```
let borad = UIPasteboard(name: UIPasteboardName(rawValue: "myBorad"), create: true)

 dataLb.text = borad?.string
```
**注意:APP的证书需要时一致的,不一致app2读取结果为空**
### Scheme

APP1:
```
let str = "app2://message=读取数据" //app2为App2设置的scheme
let url = URL(string: str.addingPercentEncoding(withAllowedCharacters: .urlHostAllowed)!)
        
if let _ = url,UIApplication.shared.canOpenURL(url!) {
            UIApplication.shared.open(url!, options: [:], completionHandler: nil)
            print("open url")
        }

**plist中 设置**
<key>LSApplicationQueriesSchemes</key>
    <array>
        <string>app2</string>
    </array>
```

APP2:
```
func application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey : Any] = [:]) -> Bool {
        if url.scheme == "app2" {
            let message = url.relativeString.removingPercentEncoding
            print("message = \(message)")
        }
        
        return true
    }
```
优点:不同证书间可以传递消息

### Keychain

一. 将APP1和APP2中Xcode的capabilities中的Keychain Shareing打开,在KeyChainGroup中输入和APP2同一字符

APP1:(使用了KeychainSwift)
```
 keychain.set("value", forKey: "key")
```
APP2:
```
let str = keychain.get("key")
dataLb.text = str
```
优点:不同证书间可以传递消息

### App Groups(iOS 8Later)

     APP1:
打开Capabilities中的APP Groups,创建一个新的group
APP2:
打开Capabilities中的APP Groups,选择创建的group(证书需相同),**测试的时候使用了中文的app名,添加group失败了,需要标准命名**

UserDefaults:
```
APP1:
 let userDefult = UserDefaults(suiteName: "group.com.damo.shareData")
 let array = ["a","b","c"]
 userDefult?.setValue(array, forKey: "key")

APP2:
 let userDefult = UserDefaults(suiteName: "group.com.damo.shareData")
 let array = userDefult?.value(forKey: "key") as? Array<String>
 print(array ?? "")
```
FileManager:
```
APP1:
let array = ["a","b","c"]
        let data = NSKeyedArchiver.archivedData(withRootObject: array)
        
        var fileURL = FileManager.default.containerURL(forSecurityApplicationGroupIdentifier: "group.com.damo.shareData")
        fileURL = fileURL?.appendingPathComponent("Library/Caches/good")

        
        do {s
            try data.write(to: fileURL!)
            
        } catch  {
            print("error")
        }

APP2:
var fileURL = FileManager.default.containerURL(forSecurityApplicationGroupIdentifier: "group.com.damo.shareData")
         fileURL = fileURL?.appendingPathComponent("Library/Caches/good")
        
        do {
            let data = try Data.init(contentsOf: fileURL!)
            let array  = NSKeyedUnarchiver.unarchiveObject(with: data)
            
            print(array ?? "")
            
        } catch  {
            print("error")
        }
```
### copy文件
fileManager remove item to path 
or copy Location path to  containing app path(Bundle取值)
有可能调用的时候文件还没有复制过去,审核可能也通不过

container app的文件目录里只有Library目录(含Crash,Preferences),container app不能单独存在,必须包含在一个app中,能够调起extension的app被称为host app



## 14.URL Types 中role选项
>- viewer : 该应用程序可以读取和呈现该类型的数据
>- editor : 该应用程序可以读取、操作和保存类型
>- none  : 该应用程序不理解数据，但只是声明有关类型的信息(例如，Finder声明字体的图标)
