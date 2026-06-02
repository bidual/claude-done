# claude-done

每轮跑完都盖个时间戳，扫一眼 tab 就知道哪个刚跑完，哪个已经晾了一阵了。

[English](README.md) · [한국어](README.ko.md) · [日本語](README.ja.md) · [Español](README.es.md)

claude-done 是给 Claude Code 写的 Stop hook。Claude 每跑完一轮，Claude Code 就执行一条 shell 命令，按你的语言和时间格式打印出当前时间，像这样：

```
完成: 2026年06月03日 03:39
```

## 解决什么问题

只开一个 session 时基本用不上，瞄一眼就知道了。多开几个才真正派上用场：几个 terminal tab 各跑各的，起身去忙别的，回来一看，光凭 transcript 根本分不清哪个 tab 是什么时候跑完的。装上这个 hook，每一轮结束都会盖上时间，扫一眼对应的 tab 就清楚了：是刚刚才好，还是已经晾了一阵子。也就每轮多打一行，平时不碍事。

## 效果

| 区域 | 显示 |
|------|------|
| English | `Done: 03 Jun 2026, 03:39` |
| 한국어 | `완료: 2026년 06월 03일 03:39` |
| 日本語 | `完了: 2026年06月03日 03:39` |
| Español | `Hecho: 03 jun 2026, 03:39` |
| Deutsch | `Fertig: 03.06.2026, 03:39` |

前缀文案和时间格式都跟着 locale 走。Claude 会先按你的设置配好一版，不合适随时改。

## 安装

把下面这段直接丢给 Claude Code：

```
帮我装一下 github.com/bidual/claude-done 的 stop hook。
读它的 INSTALL.md，照着做。
```

接下来 Claude 会自己判断你的语言和时间格式，把 hook 写进 `~/.claude/settings.json`，跑一遍确认没问题，然后就接着干活。[INSTALL.md](INSTALL.md) 是一段提示词，不是二进制，全程透明，你能一眼看完它要做什么。

## 原理

hook 就这一条命令：

```
date +'{ "systemMessage": "完成: %Y年%m月%d日 %H:%M" }'
```

Claude Code 在每轮结束时跑这个 Stop hook，`date` 把时间填进去，输出一行 JSON。Claude Code 读取里面的 `systemMessage` 字段并显示出来。不会留下任何后台常驻进程。格式完全可控，用的是标准 date 格式码（`man date`），比如 `%S` 加秒，`%I:%M %p` 换成 12 小时制。

## 修改和卸载

在 Claude Code 里用 `/hooks` 可以查看、编辑、禁用，也可以手动改 `~/.claude/settings.json` 里的 `hooks.Stop`。想彻底删掉，把那段 Stop 配置删了，或者直接让 Claude 帮你删。

## 许可证

[MIT](LICENSE)
