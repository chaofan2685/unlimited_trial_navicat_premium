# Mac/Win定时自动重置NavicatPremium16试用期

## 免责声明

本脚本为网络收集而来，免费使用，只供个人学习使用，使用需严格遵守开源许可协议。严禁用于商业用途，禁止进行任何盈利活动。对一切非法使用所产生的后果，概不负责！

## 脚本说明

- 脚本只对`Navicat Premium 16`有效，其他版本暂未测试
- 脚本不会破解程序，仅仅是删除了Navicat试用期相关的文件数据，以达到无限试用的目的，因此大家最好在[官网](http://www.navicat.com.cn/download/navicat-premium)下载Navicat才最安全，且后续升级方便
- Mac使用`reset_navicat.sh`，Win使用`reset_navicat.exe`
- 后面主要介绍的是如何在两个系统上设置定时任务自动执行各自的脚本

## 使用说明

**我们假定让自己的电脑在每天上午9:10自动执行脚本重置Navicat Premium 16试用期，下面是操作步骤。**

### Mac

1. 首先[下载](https://gitee.com/chaofan2685_admin/reset_navicat_premium_for_mac/releases)`reset_navicat.zip`，解压得到以下两个文件
   
   - com.chaofan.reset.navicat.premium.trial.period.plist
   - reset_navicat.sh
   > 此时只要使用命令`chmod u+x reset_navicat.sh`给`reset_navicat.sh`文件赋予可执行权限，然后双击执行该脚本即可重置NP16的试用期。
   
2. 按照注释修改`com.chaofan.reset.navicat.premium.trial.period.plist`文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
   <plist version="1.0">
   <dict>
       <key>Label</key>
       <!-- 此处定义的是定时任务的名称，之后可用于搜索或停止该任务，建议与文件名一致即可 -->
       <string>com.chaofan.reset.navicat.premium.trial.period</string>
       <!-- 以下两个<string>标签填写reset_navicat.sh脚本的绝对路径，请以实际为准 -->
       <key>Program</key>
       <string>/Users/chaofan/Public/MyShell/reset_navicat.sh</string>
       <key>ProgramArguments</key>
       <array>
           <string>/Users/chaofan/Public/MyShell/reset_navicat.sh</string>
       </array>
     	<!-- 在加载该文件时就执行任务，如果不需要可以删掉或改为false，调试阶段建议打开，以便查看脚本执行结果 -->
       <key>RunAtLoad</key>
       <true/>
       <!-- 在指定时间执行任务 -->
       <key>StartCalendarInterval</key>
       <dict>
           <!-- 下面表示每天9点10分执行任务 -->
           <key>Hour</key>
           <integer>9</integer>
           <key>Minute</key>
           <integer>10</integer>
       </dict>
       <!-- 运行日志，请以实际为准，调试阶段建议打开，以便查看脚本执行结果 -->
       <key>StandardOutPath</key>
       <string>/Users/chaofan/Public/MyShell/reset_navicat.log</string>
       <!-- 错误日志，请以实际为准，调试阶段建议打开，以便查看脚本执行结果 -->
       <key>StandardErrorPath</key>
       <string>/Users/chaofan/Public/MyShell/reset_navicat.log</string>
   </dict>
   </plist>
   ```
   
3. 打开终端，切换到当前目录，依次执行下面的命令加载定时任务

   ```shell
   # 为reset_navicat.sh文件授予可执行权限
   chmod u+x reset_navicat.sh
   # 将com.chaofan.reset.navicat.premium.trial.period.plist复制到~/Library/LaunchAgents文件夹中，当前用户登录后便会自动加载该定时任务
   cp com.chaofan.reset.navicat.premium.trial.period.plist ~/Library/LaunchAgents/com.chaofan.reset.navicat.premium.trial.period.plist
   # 加载定时任务，如果没有报错则任务就加载成功了，会按照计划执行重置脚本，如果上面开启了加载即执行任务和任务日志输出，此时可以去查看日志文件，获取脚本执行情况
   launchctl load -w ~/Library/LaunchAgents/com.chaofan.reset.navicat.premium.trial.period.plist
   # 如果要调整plist文件或是停止任务，请执行以下命令后再进行调整，更多launchctl使用技巧请看文末的参考链接
   launchctl unload -w ~/Library/LaunchAgents/com.chaofan.reset.navicat.premium.trial.period.plist
   ```

### Win

1. 首先[下载](https://gitee.com/chaofan2685_admin/reset_navicat_premium_for_mac/releases)`reset_navicat.exe`，双击执行即可即可重置NP16的试用期
2. `Win+R`打开运行窗口，输入`taskschd.msc`点确定打开`任务计划程序`
3. 鼠标右击`任务计划程序库`，选择`创建基本任务(B)...`，打开`创建基本任务向导`窗口
4. 在`名称(A):`处填写一个自己喜欢的名称，之后点击`下一步(N) >`
5. `希望该任务何时开始?`默认选`每天(D)`即可，之后点击`下一步(N) >`
6. 将`开始(S):`处的时间调整到上午9:10，日期和其他选项保持不变即可，之后点击`下一步(N) >`
7. `希望该任务执行什么操作?`默认选`启动程序(T)`，之后点击`下一步(N) >`
8. 点击`浏览(R)...`，找到并双击上面下载的`reset_navicat.exe`，之后点击`下一步(N) >`
9. 点击`完成(F)`
![操作流程](Win/iShot2022-04-18_11.07.08.gif)

## 参考连接

- [yhan219/navicat_reset_mac: navicat16 mac版无限重置试用期脚本 (github.com)](https://github.com/yhan219/navicat_reset_mac)
- [Mac 下的定时任务工具：Launchctl](http://wu.run/2019/03/27/mac-launchctl-guidance/)
- [Abeautifulsnow/navicat-premium-crack: This script is used to crack navicat premium application for another 14 days trial. (github.com)](https://github.com/Abeautifulsnow/navicat-premium-crack/)