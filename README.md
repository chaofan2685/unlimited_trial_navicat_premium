# Mac定时自动重置NavicatPremium16试用期

## 免责声明

本脚本为免费使用，本脚本只供个人学习使用，使用需严格遵守开源许可协议。严禁用于商业用途，禁止进行任何盈利活动。对一切非法使用所产生的后果，概不负责！

## 脚本说明

- 本脚本适用于mac系统，不适用于windows，Windows可参考[这里]([Abeautifulsnow/navicat-premium-crack: This script is used to crack navicat premium application for another 14 days trial. (github.com)](https://github.com/Abeautifulsnow/navicat-premium-crack/)
- 不怕麻烦的可以每次试用过期手动执行一下`reset_navicat.sh`就行，但是我选择是用`launchctl`自动执行脚本

## 使用说明

1. 首先[下载](https://gitee.com/chaofan2685_admin/reset_navicat_premium_for_mac/releases)解压得到以下两个文件
   
   - com.chaofan.reset.navicat.premium.trial.period.plist
   - reset_navicat.sh
   > 此时只要使用命令`chmod u+x reset_navicat.sh`给`reset_navicat.sh`文件赋予可执行权限，然后双击执行该脚本就会重置NP16的试用期了，下面的操作是创建定时任务，让Mac可以自动执行该脚本，如果不想搞，本教程到此就完结撒花了🎉！
   
2. 按照注释修改`com.chaofan.reset.navicat.premium.trial.period.plist`文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
   <plist version="1.0">
   <dict>
       <key>Label</key>
       <!-- 此处定义的是定时任务的名称，之后可用于搜索或停止该任务，建议与文件名一致即可 -->
       <string>com.chaofan.reset.navicat.premium.trial.period</string>
       <!-- 以下两个<string>标签填写需要运行的脚本的绝对路径，请以实际为准 -->
       <key>Program</key>
       <string>/Users/chaofan/Public/MyShell/reset_navicat.sh</string>
       <key>ProgramArguments</key>
       <array>
           <string>/Users/chaofan/Public/MyShell/reset_navicat.sh</string>
       </array>
       <!-- 在加载该文件时即启动任务，如果不需要可以删掉或改为false -->
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
       <!-- 间隔多少秒执行任务，下面注释内容表示间隔30秒执行一次任务，两种方式只能选一种 -->
       <!-- <key>StartInterval</key>
       <integer>30</integer> -->
       <!-- 运行日志，请以实际为准 -->
       <key>StandardOutPath</key>
       <string>/Users/chaofan/Public/MyShell/reset_navicat.log</string>
       <!-- 错误日志，请以实际为准 -->
       <key>StandardErrorPath</key>
       <string>/Users/chaofan/Public/MyShell/reset_navicat.log</string>
   </dict>
   </plist>
   ```

3. 打开终端，切换到当前目录，执行下面的命令加载定时任务

   ```shell
   # 为reset_navicat.sh文件授予可执行权限
   chmod u+x reset_navicat.sh
   # 将com.chaofan.reset.navicat.premium.trial.period.plist复制到~/Library/LaunchAgents文件夹中，当前用户登录后便会自动加载该定时任务
   cp com.chaofan.reset.navicat.premium.trial.period.plist ~/Library/LaunchAgents/com.chaofan.reset.navicat.premium.trial.period.plist
   # 加载定时任务
   launchctl load -w ~/Library/LaunchAgents/com.chaofan.reset.navicat.premium.trial.period.plist
   ```

## 参考连接：

- [yhan219/navicat_reset_mac: navicat16 mac版无限重置试用期脚本 (github.com)](https://github.com/yhan219/navicat_reset_mac)
- [Mac 下的定时任务工具：Launchctl](http://wu.run/2019/03/27/mac-launchctl-guidance/)