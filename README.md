# aria2.conf
本项目提供一个不错的 aria2 配置文件，同时提供 MacOS 下的开机启动并可控的解决方案

### conf 注意事项
使用请将配置文件中三处文件路径修改为自己的路径

将 aria2.conf 放在 ~/.aria2/ 下

使用 aria2 命令时，aria2 会自动加载 ~/.aria2/aria2.conf

配置文件中没有启用 input-file 选项，理由在文件中有说明

### MacOS 开机启动详细

将 Aria2.sh 放在你喜欢的地方😆

修改 plist 中 shell 的路径

将 local.Aria2.plist 放在 ~/Library/LaunchAgents/ 下

打开终端执行以下命令添加启动计划

```
launchctl load ~/Library/LaunchAgents/local.Aria2.plist
```

添加完后任务便立刻开始

可以通过以下命令查看是否添加成功

```
launchctl list | grep Aria2
```

可以通过以下命令进入 tmux 查看 aria2 的运行状态/日志

```
tmux a -t Aria2
```

要退出 tmux 请按下 Ctrl+b 后输入 d

若要重启 rpc，

进入 tmux，按下 Ctrl+c 终止任务

开启 aria-rpc 使用
```
launchctl start local.Aria2
```
或
```
tmux -d -s Aria2 '/path/to/shell/Aria2.sh'
```

~~**注：**你可能会发现 plist 中 ProgramArgument 部分有一个奇怪的地方 `&& w`~~

~~没错他是多余的无用的，但没有他这个 launchd 项目就会启动失败~~

~~我在 stackoverflow 上对问题作了详细的描述-->[链接](http://stackoverflow.com/questions/37990530/use-launch-daemon-spawn-a-screen-session-run-aria2-rpc)~~

换成 tmux 以后似乎就没有这个问题了。

然后链接里那个问题由于长期无人回答，被 stackoverflow 删除了。。。

----

如果要删除开机启动 请把最开始的命令中的 load 改成 unload

~~PS: shell 中 aria2 使用了绝对路径，这是 brew 安装的 aria2 所在路径，之所以使用绝对路径是因为如果不这样做会有 bug(bug 似乎仅限于 sh，bash 应该就没事)~~

其他系统的话可以把 tmux 和 aria2 命令写在同一行里添加到 rc.local

大概是这个样子

```
su - username -c 'tmux new -d -s Aria2 aria2c --enable-rpc=true --input-file=/home/username/.aria2/aria2.session --conf-path=/home/username/.aria2/aria2.conf'
```

嵌套太多可能会失败，所以建议拆成 shell 后添加到 rc.local

*完*
