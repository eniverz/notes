---
type: skills
tags:
  - cli
  - software
  - GUI
  - code
done: false
topic:
  - "[[Coding]]"
---
# Codex CLI Newline

- 在 macOS 中优先试 <kbd>Option+Enter</kbd> 或 <kbd>Control+J</kbd>。
- 在 Windows/Linux 中优先试 <kbd>Ctrl+J</kbd>。
- 如果 <kbd>Shift+Enter</kbd> 没用，通常是终端没有把它作为独立按键传给 Codex。进入 Codex 后输入 `/keymap`，使用 keypress inspector 看 Codex 实际收到了什么键。

# Codex CLI Shortcuts

记录时间：2026-07-03  
本机版本：`codex-cli 0.142.5`  
主要依据：Codex CLI 当前 TUI keymap 结构和官方源码 `codex-rs/tui/src/keymap.rs`。  
配置入口：在 Codex CLI 中输入 `/keymap`。配置文件路径通常是 `~/.codex/config.toml`，对应配置键为 `tui.keymap.*`。

> 注意：同一个按键在不同 TUI 上下文里可能对应不同动作。例如 <kbd>Enter</kbd> 在普通 composer 中通常提交 prompt，但在某些编辑上下文中也可能是换行。终端如果不传递某个组合键，Codex 就无法绑定它。

## Global

| 快捷键 | 动作 | 配置动作名 |
|---|---|---|
| <kbd>Ctrl+R</kbd> | 打开 transcript overlay | `global.open_transcript` |
| <kbd>Ctrl+X</kbd> | 用外部编辑器打开当前 draft | `global.open_external_editor` |
| <kbd>Ctrl+Y</kbd> | 复制上一条 agent 回复 | `global.copy` |
| <kbd>Ctrl+L</kbd> | 清空终端 UI | `global.clear_terminal` |
| <kbd>Ctrl+V</kbd> | 切换 Vim composer mode | `global.toggle_vim_mode` |
| <kbd>Ctrl+T</kbd> | 切换 Fast mode | `global.toggle_fast_mode` |
| <kbd>Ctrl+O</kbd> | 切换 raw scrollback mode | `global.toggle_raw_output` |

## Chat

| 快捷键 | 动作 | 配置动作名 |
|---|---|---|
| <kbd>Esc</kbd> | 中断当前 turn | `chat.interrupt_turn` |
| <kbd>Ctrl+-</kbd> | 降低 reasoning effort | `chat.decrease_reasoning_effort` |
| <kbd>Ctrl+=</kbd> | 提高 reasoning effort | `chat.increase_reasoning_effort` |
| <kbd>Ctrl+E</kbd> | 编辑最近 queue 的消息 | `chat.edit_queued_message` |

## Composer

| 快捷键 | 动作 | 配置动作名 |
|---|---|---|
| <kbd>Enter</kbd> | 提交当前 composer draft | `composer.submit` |
| <kbd>Ctrl+Q</kbd> | 任务运行中 queue 当前 draft | `composer.queue` |
| <kbd>Ctrl+G</kbd> | 显示/隐藏 composer shortcut overlay | `composer.toggle_shortcuts` |
| <kbd>Up</kbd> | 打开历史搜索或上一条匹配 | `composer.history_search_previous` |
| <kbd>Down</kbd> | 下一条历史搜索匹配 | `composer.history_search_next` |

## Editor

| 快捷键 | 动作 | 配置动作名 |
|---|---|---|
| <kbd>Enter</kbd>, <kbd>Shift+Enter</kbd>, <kbd>Alt+Enter</kbd>, <kbd>Ctrl+J</kbd> | 在编辑器中插入换行 | `editor.insert_newline` |
| <kbd>Left</kbd> | 光标左移 | `editor.move_left` |
| <kbd>Right</kbd> | 光标右移 | `editor.move_right` |
| <kbd>Up</kbd> | 光标上移 | `editor.move_up` |
| <kbd>Down</kbd> | 光标下移 | `editor.move_down` |
| <kbd>Alt+Left</kbd>, <kbd>Ctrl+Left</kbd>, <kbd>Esc B</kbd> | 移动到前一个词的开头 | `editor.move_word_left` |
| <kbd>Alt+Right</kbd>, <kbd>Ctrl+Right</kbd>, <kbd>Esc F</kbd> | 移动到下一个词的结尾 | `editor.move_word_right` |
| <kbd>Home</kbd>, <kbd>Ctrl+A</kbd> | 移动到行首 | `editor.move_line_start` |
| <kbd>End</kbd>, <kbd>Ctrl+E</kbd> | 移动到行尾 | `editor.move_line_end` |
| <kbd>Backspace</kbd>, <kbd>Ctrl+H</kbd> | 删除左侧一个 grapheme | `editor.delete_backward` |
| <kbd>Delete</kbd>, <kbd>Ctrl+D</kbd> | 删除右侧一个 grapheme | `editor.delete_forward` |
| <kbd>Alt+Backspace</kbd>, <kbd>Ctrl+Backspace</kbd>, <kbd>Ctrl+W</kbd>, <kbd>Esc Backspace</kbd> | 删除前一个词 | `editor.delete_backward_word` |
| <kbd>Alt+Delete</kbd>, <kbd>Ctrl+Delete</kbd>, <kbd>Esc Delete</kbd> | 删除下一个词 | `editor.delete_forward_word` |
| <kbd>Ctrl+U</kbd> | 从光标删除到行首 | `editor.kill_line_start` |
| <kbd>Ctrl+K</kbd> | 从光标删除到行尾 | `editor.kill_line_end` |
| <kbd>Ctrl+L</kbd> | 删除当前整行 | `editor.kill_whole_line` |
| <kbd>Ctrl+Y</kbd> | 粘贴 kill buffer | `editor.yank` |

## Vim Normal

| 快捷键 | 动作 | 配置动作名 |
|---|---|---|
| <kbd>i</kbd> | 在光标处进入 insert mode | `vim_normal.enter_insert` |
| <kbd>a</kbd> | 在光标后进入 insert mode | `vim_normal.append_after_cursor` |
| <kbd>A</kbd> | 在行尾进入 insert mode | `vim_normal.append_line_end` |
| <kbd>I</kbd> | 在第一个非空白字符处进入 insert mode | `vim_normal.insert_line_start` |
| <kbd>o</kbd> | 在下方新开一行并进入 insert mode | `vim_normal.open_line_below` |
| <kbd>O</kbd> | 在上方新开一行并进入 insert mode | `vim_normal.open_line_above` |
| <kbd>h</kbd>, <kbd>Left</kbd> | 左移 | `vim_normal.move_left` |
| <kbd>l</kbd>, <kbd>Right</kbd> | 右移 | `vim_normal.move_right` |
| <kbd>k</kbd>, <kbd>Up</kbd> | 上移，或召回更早的历史 | `vim_normal.move_up` |
| <kbd>j</kbd>, <kbd>Down</kbd> | 下移，或召回更新的历史 | `vim_normal.move_down` |
| <kbd>w</kbd> | 移动到下一个词开头 | `vim_normal.move_word_forward` |
| <kbd>b</kbd> | 移动到前一个词开头 | `vim_normal.move_word_backward` |
| <kbd>e</kbd> | 移动到当前或下一个词结尾 | `vim_normal.move_word_end` |
| <kbd>0</kbd> | 移动到行首 | `vim_normal.move_line_start` |
| <kbd>x</kbd> | 删除光标下字符 | `vim_normal.delete_char` |
| <kbd>s</kbd> | 删除光标下字符并进入 insert mode | `vim_normal.substitute_char` |
| <kbd>D</kbd> | 从光标删除到行尾 | `vim_normal.delete_to_line_end` |
| <kbd>C</kbd> | 从光标修改到行尾并进入 insert mode | `vim_normal.change_to_line_end` |
| <kbd>Y</kbd> | yank 当前整行 | `vim_normal.yank_line` |
| <kbd>p</kbd> | 在光标后粘贴 | `vim_normal.paste_after` |
| <kbd>d</kbd> | 开始 delete operator，等待 motion | `vim_normal.start_delete_operator` |
| <kbd>y</kbd> | 开始 yank operator，等待 motion | `vim_normal.start_yank_operator` |
| <kbd>c</kbd> | 开始 change operator，等待 text object | `vim_normal.start_change_operator` |
| <kbd>Esc</kbd> | 取消 pending Vim operator | `vim_normal.cancel_operator` |

## Vim Operator

| 快捷键 | 动作 | 配置动作名 |
|---|---|---|
| <kbd>d</kbd> | 重复 delete operator，删除整行 | `vim_operator.delete_line` |
| <kbd>y</kbd> | 重复 yank operator，yank 整行 | `vim_operator.yank_line` |
| <kbd>h</kbd>, <kbd>Left</kbd> | operator motion 左移 | `vim_operator.motion_left` |
| <kbd>l</kbd>, <kbd>Right</kbd> | operator motion 右移 | `vim_operator.motion_right` |
| <kbd>k</kbd>, <kbd>Up</kbd> | operator motion 上移 | `vim_operator.motion_up` |
| <kbd>j</kbd>, <kbd>Down</kbd> | operator motion 下移 | `vim_operator.motion_down` |
| <kbd>w</kbd> | operator motion 到下一个词开头 | `vim_operator.motion_word_forward` |
| <kbd>b</kbd> | operator motion 到前一个词开头 | `vim_operator.motion_word_backward` |
| <kbd>e</kbd> | operator motion 到词尾 | `vim_operator.motion_word_end` |
| <kbd>0</kbd> | operator motion 到行首 | `vim_operator.motion_line_start` |
| <kbd>$</kbd> | operator motion 到行尾 | `vim_operator.motion_line_end` |
| <kbd>i</kbd> | 选择 inner text object | `vim_operator.select_inner_text_object` |
| <kbd>a</kbd> | 选择 around text object | `vim_operator.select_around_text_object` |
| <kbd>Esc</kbd> | 取消 pending operator | `vim_operator.cancel_operator` |

## Vim Text Object

| 快捷键 | 动作 | 配置动作名 |
|---|---|---|
| <kbd>w</kbd> | 当前 word | `vim_text_object.word` |
| <kbd>W</kbd> | 当前 WORD | `vim_text_object.big_word` |
| <kbd>(</kbd>, <kbd>)</kbd> | 括号 text object | `vim_text_object.parentheses` |
| <kbd>[</kbd>, <kbd>]</kbd> | 方括号 text object | `vim_text_object.brackets` |
| <kbd>{</kbd>, <kbd>}</kbd> | 花括号 text object | `vim_text_object.braces` |
| <kbd>"</kbd> | 双引号 text object | `vim_text_object.double_quote` |
| <kbd>'</kbd> | 单引号 text object | `vim_text_object.single_quote` |
| <kbd>`</kbd> | 反引号 text object | `vim_text_object.backtick` |
| <kbd>Esc</kbd> | 取消 pending text object | `vim_text_object.cancel` |

## Pager

| 快捷键 | 动作 | 配置动作名 |
|---|---|---|
| <kbd>Up</kbd>, <kbd>k</kbd> | 向上滚动一行 | `pager.scroll_up` |
| <kbd>Down</kbd>, <kbd>j</kbd> | 向下滚动一行 | `pager.scroll_down` |
| <kbd>PageUp</kbd>, <kbd>b</kbd> | 向上滚动一页 | `pager.page_up` |
| <kbd>PageDown</kbd>, <kbd>Space</kbd> | 向下滚动一页 | `pager.page_down` |
| <kbd>u</kbd> | 向上滚动半页 | `pager.half_page_up` |
| <kbd>d</kbd> | 向下滚动半页 | `pager.half_page_down` |
| <kbd>g</kbd>, <kbd>Home</kbd> | 跳到开头 | `pager.jump_top` |
| <kbd>G</kbd>, <kbd>End</kbd> | 跳到底部 | `pager.jump_bottom` |
| <kbd>Esc</kbd>, <kbd>q</kbd> | 关闭 pager overlay | `pager.close` |
| <kbd>Esc</kbd>, <kbd>q</kbd> | 关闭 transcript overlay | `pager.close_transcript` |

## List

| 快捷键 | 动作 | 配置动作名 |
|---|---|---|
| <kbd>Up</kbd>, <kbd>k</kbd> | 选择上一项 | `list.move_up` |
| <kbd>Down</kbd>, <kbd>j</kbd> | 选择下一项 | `list.move_down` |
| <kbd>Left</kbd>, <kbd>h</kbd> | 在 list picker 中向左移动 | `list.move_left` |
| <kbd>Right</kbd>, <kbd>l</kbd> | 在 list picker 中向右移动 | `list.move_right` |
| <kbd>PageUp</kbd>, <kbd>Ctrl+B</kbd> | 上翻页 | `list.page_up` |
| <kbd>PageDown</kbd>, <kbd>Ctrl+F</kbd> | 下翻页 | `list.page_down` |
| <kbd>Home</kbd>, <kbd>g</kbd> | 跳到第一项 | `list.jump_top` |
| <kbd>End</kbd>, <kbd>G</kbd> | 跳到最后一项 | `list.jump_bottom` |
| <kbd>Enter</kbd> | 接受当前选择 | `list.accept` |
| <kbd>Esc</kbd>, <kbd>q</kbd> | 取消并关闭选择视图 | `list.cancel` |

## Approval

| 快捷键 | 动作 | 配置动作名 |
|---|---|---|
| <kbd>v</kbd> | 全屏打开 approval 详情 | `approval.open_fullscreen` |
| <kbd>t</kbd> | 打开 approval 来源 thread | `approval.open_thread` |
| <kbd>y</kbd> | 批准主要选项 | `approval.approve` |
| <kbd>s</kbd> | 本 session 内批准 | `approval.approve_for_session` |
| <kbd>p</kbd> | 使用 exec-policy prefix 批准 | `approval.approve_for_prefix` |
| <kbd>n</kbd> | 显式 deny | `approval.deny` |
| <kbd>d</kbd> | 拒绝并提供修正指导 | `approval.decline` |
| <kbd>Esc</kbd> | 取消 elicitation request | `approval.cancel` |

## 自定义示例

如果想把换行固定成 <kbd>Ctrl+J</kbd>：

```toml
[tui.keymap.editor]
insert_newline = "ctrl-j"
```

如果想给换行同时保留多个按键：

```toml
[tui.keymap.editor]
insert_newline = ["ctrl-j", "alt-enter", "shift-enter"]
```

如果某个组合键绑定不上，先在 `/keymap` 里的 keypress inspector 检查终端是否真的把这个键发给 Codex。
