### rsync

你准备好看一些很酷的东西了吗？你是否遇到过需要把服务器上一个巨大的目录中改变的内容下载下来，却不知道你需要下载哪些文件？你可以重新下载整个目录，但是那样会做很多重复的工作；你也可以手动比较并选择下载，但是这样非常繁琐。也许你已经下载了一个大文件，如光盘镜像，但是仅仅只要出现了几个字节的错误，你就不得不重新下载整个文件，这并不合理不是吗？让我们开始使用 `rsync(1)`，一个快速而灵活的远程文件同步工具。

`rsync` 使用一些简单有效的技术来检查服务器上哪些内容发生了改变。通过检查时间戳和大小，`rsync` 可以确定两个文件是否不同，如果文件一些信息发生了变化，它可以确定文件哪些字节发生了改变。并直接下载改变的部分而不是整个文件，这是现代技术的奇迹。

在最简单的使用情景下，`rsync` 连接到*rsync*协议服务器下载文件和目录信息，包括时间戳和大小等信息。然后，`rsync` 会将此和本地文件比较（如果有的话），以确定它需要传输的文件。另外，它把大文件分解成小块，并且使用一个简单快速的哈希函数进行比较，只有不匹配的块才会被同步，因此同步的数据量大大减少。 `rsync` 也支持*压缩*、_详细输出_、_文件删除_、*权限管理*以及更多可选的选项。如果需要一个完整的列表，你可以查看手册。在此，我们列出了一些常用的选项。

#### Table 1.6 rsync Arguments

| 选项 | 说明                                     |
| ---- | ---------------------------------------- |
| -v   | 更详细的输出                             |
| -c   | 通过校验和检查每个文件而不是时间戳和大小 |
| -a   | 归档模式（与-rlptgoD 相同）              |
| -e   | 使用指定的远程 shell                     |
| -r   | 递归模式                                 |
| -u   | 更新模式-跳过时间戳更新的本地文件        |
| -p   | 保留权限信息                             |
| -n   | 测试运行，不更改任何数据                 |
| -z   | 压缩-方便低速的网络环境                  |

`rsync` 的强大功能还在于能通过不同的方式调用。下面的两个例子使用 `rsync` 连接到*rsync*协议的服务器检索一些信息，然后使用*ssh*传输到另一台服务器。

```
darkstar:~# rsync -avz rsync://ftp.osuosl.org/pub/slackware/slackware-current/ \
/src/slackware-current/
darkstar:~# rsync -e ssh ftp.slackware.com:/home/alan/foo /tmp/foo
```
