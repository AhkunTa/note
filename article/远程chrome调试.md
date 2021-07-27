#远程chrome手机真机调试

##一 android手机调试

###1. 打开chrome://inspect
###2. adnroid手机数据线链接电脑，android手机需打开usb调试功能/信任电脑
###3. 若在chrome inspect devices目录下没有远程链接，可以安装ADB `Plugin for remote debugging Chrome on Android (Now deprecated)一个chrome插件` `插件教程在  https://developers.google.com/web/tools/chrome-devtools/remote-debugging`
###4. 有远程链接显示，点击inspect进行调试，调试需要梯子翻墙，因为一个调试的js需要外网，否则打开为空白页

---------------

##二 ios手机调试

### 在mac电脑的safari上调试 

1. 首先选装 **brew** 然后 `brew install ios-webkit-debug-proxy`
2. 手机上需要开启 设置 -> Safari -> 高级 -> Web 检查器
3. USB 连接电脑和手机，手机上弹出提示选**信任设备**
4. 启动 ios_webkit_debug_proxy  直接 输入命令 `ios_webkit_debug_proxy`  如果**错误**执行以下命令
		
		地址： https://github.com/libimobiledevice/libimobiledevice/issues/717#issuecomment-430622355
		
		brew uninstall --ignore-dependencies libimobiledevice
		brew uninstall --ignore-dependencies ideviceinstaller
		brew uninstall --ignore-dependencies usbmuxd
		sudo rm /var/db/lockdown/*
		brew install --HEAD usbmuxd
		brew unlink usbmuxd
		brew link usbmuxd
		brew install --HEAD libimobiledevice
		brew install --HEAD ideviceinstaller


5. 用手机的 Safari 浏览器打开 需要调试的页面
6. 打开电脑的 Safari 浏览器 -> 开发 -> 你的手机名称 -> 调试页面
7. 点击之后可以看到网页检查器，用检查器选择元素可以看到手机上有相应的蓝色蒙层










### 在windows上 利用chrome devtools 调试
1. 首先需要安装工具 **scoop** 然后执行分别 `scoop bucket add extras`和 `scoop install ios-webkit-debug-proxy`
2. 之后需要安装  remotedebug-ios-webkit-adapter `npm install remotedebug-ios-webkit-adapter -g`
3. 手机上需要开启 设置 -> Safari -> 高级 -> Web 检查器
4. USB 连接电脑和手机，手机上弹出提示选**信任设备**
5. 之后运行 	`remotedebug_ios_webkit_adapter --port=9000`
6. 打开chrome调试工具或者vscode 配置端口
7. chrome调试工具 在 `Discover network targets` 和`Discover USB devices` 配置好端口号，和地址

