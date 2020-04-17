---
layout: post
title:  "MicroPython ure - 正则表达式库"
date:   2019-08-18 0:0:0 +0000
category: library
---

`ure` 模块实现了正则表达式操作. 支持的语法是 *CPython* `re`模块的一个子集(实际上是POSIX扩展正则表达式的一个子集).

支持的操作符和特殊序列:

- `.` - 匹配任意字符

- `[...]` - 匹配字符集。支持单个字符和范围，包括反集(例如: `a - c [^]`).

- `^` - 匹配字符串的开始

- `$` - 匹配字符串结尾

- `?` - 匹配 0 或 1 个 前面的子模式.

- `*` - 匹配 0 或 多 个 前面的子模式.

- `+` - 匹配 1 或 多 个 前面的子模式.

- `??` - 惰性版本的 `?`, 匹配 0 个 或 1个， 0优先.

- `*?` - 惰性版本的 `*`, 匹配 0 个 或 多个， 更短优先.

- `+?` - 惰性版本的 `+`, 匹配 1 个 或 多个， 更短优先.

- `|` - 匹配该操作符的左或右侧子模式.

- `(...)` - 分组. 捕获分组(可通过`match.group()`方法访问捕获的子字符串).

- `\d` - 匹配数字, 与 `[0-9]` 等效

- `\D` - 匹配非数字, 与 `[^0-9]` 等效

- `\s` - 匹配空白字符. 与 `[ \t-\r]` 等效

- `\S` - 匹配非空白字符. 与 `[^ \t-\r]` 等效

- `\w` - 匹配单词字符(仅 *ASCII*). 与 `[A-Za-z0-9_]` 等效

- `\W` - 匹配非单词字符(仅 *ASCII*). 与 `[^A-Za-z0-9_]` 等效

- `\` - 转义字符. 除上述字符外，`\` 后面的任何其他字符都按字面意义处理. 例如 `\*` 匹配`*`本身(不是通配符), 另外 `\r`和`\n` 并不会转义处理, 因此不建议用 `r""`的形式书写正则表达式. 若要匹配需要用字符串拼接.

**不支持**:

* 重复计数 (`{m,n}`)

* 命名分组 (`(?P<name>...)`)

* 非捕获分组 (`(?:...)`)

* 高级断言 (`\b`, `\B`)

* 特殊转移字符 `\r`, `\n` - 使用 *Python* 本身的代替

* 等等.

例如:

```python
import ure

# 由于 ure 不支持转义书写换行, 所以不用 r"" 表达式
regex = ure.compile("[\r\n]")

regex.split("line1\rline2\nline3\r\n")

# 结果:
# ['line1', 'line2', 'line3', '', '']
```

模块函数
=========

##### `ure.compile(regex_str[, flags])`{:class="func"}

编译正则表达式, 返回 正则表达式 对象.


##### `ure.match(regex_str, string)`{:class="func"}

编译 *regex_str* 并匹配  *string*. 匹配总是从字符串的起始位置开始.


##### `ure.search(regex_str, string)`{:class="func"}

编译 *regex_str* 并在 *string* 中搜索. 与`match`不同的是, 它将搜索与正则匹配的第一个位置.

##### `ure.sub(regex_str, replace, string, count=0, flags=0)`{:class="func"}

编译 *regex_str* 并在 *string* 中搜索, 用 *replace* 替换所有匹配项，并返回新字符串.

*replace* 可为以下类型:
- **字符串**, 可使用 `\<number>` 和 `\g<number>` 转义序列展开到相应组(或用空字符串表示未匹配到).
- **回调函数**, 则必须接收单个参数(匹配到的内容)并返回一个替换字符串.

如果指定了 *count* 且 *非0* ，那么替换只会执行 *count* 次, 然后停止

*flags* 参数 未用到


`Regex` 对象
=============

已编译好的正则表达式. 使用 `ure.compile()` 创建该类实例.

##### `regex.match(string)`{:class="method"}
##### `regex.search(string)`{:class="method"}
##### `regex.sub(replace, string, count=0, flags=0)`{:class="method"}
类似于模块的`match()`、`search()`和`sub()`函数. 若编译后的正则表达式对象 用于多次处理字符串， 则效率比模块对应的方法高得多.

##### `regex.split(string, max_split=-1)`{:class="method"}

使用 *regex* 分割 *string*。如果给定了 *max_split*, 它指定要执行的最大分割数.

返回字符串列表(如果指定了 *max_split* 的话，可能有*max_split+1*个元素).


`Match` 对象
=============

Match 对象 由 `match()`和`search()`方法返回，并传递给`sub()`的 *replace* 回调函数.

##### `match.group(index)`{:class="method"}

返回匹配到的(子)字符串。*index* 对于整个匹配项为0，对于每个捕获分组 >=1. 

##### `match.groups()`{:class="method"}

返回包含匹配分组的所有子字符串的元组.


##### `match.start([index])`{:class="method"}
##### `match.end([index])`{:class="method"}
返回 匹配的子字符串组 的 *开始* 或 *结束* 在 原始字符串中的位置。*index* 默认为整个组，其他情况表示对应的匹配分组.


##### `match.span([index])`{:class="method"}

返回二元组 `(match.start(index)， match.end(index))`

