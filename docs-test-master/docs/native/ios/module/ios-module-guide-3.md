---
id: ios-module-guide-3
title: ios module guide 3
hide_title: false
---

上一篇中添加了新的`Image`方法和`Reusable`协议，我们需要在模块中进行替换
# 替换获取`Image`方法
使用正则表达式搜索
`#imageLiteral\(resourceName: *\"(.*?)\"\)`/`UIImage.themedImage\(with: *\"(.*?)\"\)`/
`UIImage.image\(with: *\"(.*?)\"\)`
使用新的方法替换,此处的`$1`就是`(.*?)`里的内容，会自动变化
```
UIImage.image\(with: "$1"\)
```
还有其他类似的image 初始化方法也需要检查，例如拼接字符串创建image的

备注：
> 这个可以做成脚本
# 替换nib中的customModule
==customModule 如果不修改，或者是绑定错误的时候会造成crash==
## 自动替换
使用项目中的脚本自动替换匹配，文件是ios-client/Script/storyboard_scanner.py，可以联系serena li大佬

## 手动替换
进入`YourModuleName` 目录下然后执行
`
grep -rl 'customModule="Binance"' ./
`
就可以检索出nib中有多少个customModule 设置的Binance，同理Binance 可以替换为任意字符
`
grep -rE 'customModule=\"(.*?)\"' ./
`
使用该命令可以在控制台打印出nib中的customModule都是什么类型

`grep -rl 'customModule=\"Binance\"' ./  | xargs sed -i "" "s/customModule=\"Binance\"/customModule=\"BinanceMarketsModule\"/g"`
使用该命令可以将目录下的所有`Binance`字符替换成`BinanceMarketsModule`

# 图片迁移
图片迁移脚本使用说明：

该脚本的工作原理

1.先扫描模块的swift 文件和storyboard 文件中都使用到了哪些图片文件名（all_images）

2.去主工程主工程找到该图片文件的路径，并且打印出来，返回参数（found_images, not_found_images）not_founds_images 需要开发大佬自己手动找一下图片路径

3.根据图片文件名查询主工程是否有其他地方在使用该图片，打印出两个库都在使用的图片资源的文件列表（both_use_images）

4.拷贝模块中使用到的图片到指定目录

5.all_images - both_use_images 就是可以删除的图片列表，can_remove_images, 根据参数判断是否需要删除这些图片



根据上述步骤，那么我们的执行脚本的时机应该是在模块的.swift 文件和.storyboard都迁移完成之后开始使用该脚本自动迁移图片资源



根据上述原理我们可以得出一下参数：

-s source path, ios-client 主工程路径
-d destination_path, copy image to destination_path 图片拷贝的目的地路径
-m module path, in which module the picture is used 正在进行模块化的模块路径
-r Whether to delete the images from the source path 是否需要删除主工程不在使用的图片
`
$ruby ./Script/images-migration-script.rb -h #可以查看参数和说明
$ruby ./Script/images-migration-script.rb -s /Users/user/Desktop/ios-client/Binance -d /Users/user/Desktop/images -m /Users/user/Desktop/ios-client/Modules/BinanceConvertModule #可以这样使用
`

注意：
**暂时没有筛选xib图片，后续会补上**

storyboard 正则匹配图片的代码如下：

`pattern = re.compile("image=\"(.*?)\"|imageNameBase=\"(.*?)\"")`

.swift文件正则匹配图片的代码如下：

`pattern = re.compile("UIImage\(named: *\"(.*?)\"\)|UIImage\(themed: *\"(.*?)\"\)|#imageLiteral\(resourceName: *\"(.*?)\"\)")`

正则可能覆盖不完善，比如说手动拼接图片名等操作，或者是使用了自定义的图片init 方法，都会存在图片扫描不完全的情况

该脚本的目的是为了减少人工操作成本，尽量可以多的自动化，仍需要大佬们做一次double check 确保图片都已经迁移完毕
