# osc7501

生成符合 OSC 7501（Program Status Protocol）格式的终端状态消息，帮助支持该协议的终端显示程序当前状态。

## 安装

```sh
mkdir -p ~/bin
curl -fsSL https://raw.githubusercontent.com/is/osc7501-sh/main/osc7501 -o ~/bin/osc7501
chmod +x ~/bin/osc7501
```

确保 `~/bin` 已加入 `PATH`。

## 用法

```text
osc7501 STATE [选项]
```

支持的状态：`idle`、`working`、`done`、`blocked`、`error`、`clear`。

| 选项 | 说明 |
| --- | --- |
| `--id ID` | 状态记录标识，可使用 `/` 表示层级，例如 `build/test` |
| `--kind KIND` | 阻塞原因：`permission`、`question` 或 `auth`；仅用于 `blocked` 状态 |
| `--app APP` | 应用名称 |
| `--title TITLE` | 状态标题 |
| `--msg MESSAGE` | 状态消息；脚本会自动进行 Base64 编码 |
| `--bel` | 使用 BEL (`0x07`) 结束序列，默认使用 ST (`ESC \\`) |
| `--printable` | 将控制字符显示为 `ESC`/`BEL` 文本，便于调试 |

### 示例

报告任务正在运行：

```sh
osc7501 working --app deploy --msg '正在部署生产环境'
```

报告等待用户批准：

```sh
osc7501 blocked --kind permission --app deploy --title '需要批准' --msg '是否继续部署？'
```

结束或清除状态：

```sh
osc7501 done --app deploy --msg '部署完成'
osc7501 clear
```

默认输出是终端控制序列，会直接发送给终端。调试时可查看可读形式：

```sh
osc7501 working --msg '处理中' --printable
```

OSC 7501 格式为 `ESC ] 7501 ; key=value[:key=value...] ST`。`state` 必填；`msg` 使用 Base64 编码。终端需支持 OSC 7501 才会展示状态。

## 许可证

本项目采用 GNU 通用公共许可证第 3 版或更高版本（GPL-3.0-or-later）。完整文本请参阅 [LICENSE](LICENSE)。
