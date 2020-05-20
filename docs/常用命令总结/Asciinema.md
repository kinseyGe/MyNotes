- 安装asciinema

```
apt-get install asciinema
```

- 开始记录视频

```
asciinema rec first.cast
```

- 以倍速重播

```
asciinema play -s 2 first.cast
```

- 分享或上传至网络

```
asciinema upload first.cast
```

 - play <filename>
 
> 重放录制在终端asciicast。该命令rec直接在终端中重播给定的asciicast（由命令记录）。

```
以下键盘快捷键可用：
    Space - 切换暂停，
    . - 逐步完成一帧录制（暂停时），
    Ctrl+C - 出口。
```
    
```
可用选项：
    -i, --idle-time-limit=<sec>- 将重播终端的不活动限制在最大<sec>秒数
    -s, --speed=<factor> - 播放速度（可以是小数）
```
    

```
从本地文件播放
    asciinema play /path/to/asciicast.cast
```

```
从http(s)URL播放
    asciinema play https://asciinema.org/a/22124.cast
    asciinema play http://example.com/demo.cast
```

- rec [name]

> 记录终端回话命令。

```
可用选项：
    --stdin - 启用标准输入（键盘）录制（请参阅下文）
    --append - 追加到现有的录音
    --raw - 保存原始STDOUT输出，无需定时信息或其他元数据
    --overwrite - 覆盖已存在的记录
    -c, --command=<command> - 指定要记录的命令，默认为$ SHELL
    -e, --env=<var-names> - 要捕获的环境变量列表，默认为 SHELL,TERM
    -t, --title=<title> - 指定asciicast的标题
    -i, --idle-time-limit=<sec>- 将记录的终端非活动<sec>时间限制为最大秒数
    -y, --yes - 对所有提示回答“是”（例如上传确认）
    -q, --quiet - 保持安静，压制所有通知/警告（暗示-y）
```

- cat [filename]

> 将记录的全部shell命令输出打印到终端。


```
把记录的shell命令输出到output.txt文件中
    asciinema cat existing.cast >output.txt
```

- upload <filename>

> 上传记录的asciicast到asciinema.org网站

```
asciinema upload first.cast
```
