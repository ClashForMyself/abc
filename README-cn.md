# subconverter

在各种订阅格式之间进行转换的实用程序.

[![Build Status](https://travis-ci.com/tindy2013/subconverter.svg?branch=master)](https://travis-ci.com/tindy2013/subconverter)
[![GitHub tag (latest SemVer)](https://img.shields.io/github/tag/tindy2013/subconverter.svg)](https://github.com/tindy2013/subconverter/tags)
[![GitHub release](https://img.shields.io/github/release/tindy2013/subconverter.svg)](https://github.com/tindy2013/subconverter/releases)
[![GitHub license](https://img.shields.io/github/license/tindy2013/subconverter.svg)](https://github.com/tindy2013/subconverter/blob/master/LICENSE)

- [subconverter](#subconverter)
  - [支持类型](#支持类型)
  - [简易用法](#简易用法)
    - [调用地址](#调用地址)
    - [调用说明](#调用说明)
  - [进阶用法](#进阶用法)
    - [阅前提示](#阅前提示)
    - [进阶地址](#进阶地址)
    - [配置文件](#配置文件)
    - [外部配置](#外部配置)
  - [自动上传](#自动上传)

## 支持类型

| 类型         | 作为源类型 | 作为目标类型 | 参数        |
| ------------ | :--------: | :----------: | ----------- |
| Clash        |     ✔      |      ✔       | clash       |
| ClashR       |     ✔      |      ✔       | clashr      |
| Quantumult (完整配置)   |     ✔      |      ✔       | quan        |
| Quantumult X (完整配置) |     ✔      |      ✔       | quanx       |
| SS (SIP002)  |     ✔      |      ✔       | ss          |
| SS (软件订阅)|     ✔      |      ✔       | sssub       |
| SSD          |     ✔      |      ✔       | ssd         |
| SSR          |     ✔      |      ✔       | ssr         |
| Surfboard    |     ✔      |      ✔       | surfboard   |
| Surge 2      |     ✔      |      ✔       | surge&ver=2 |
| Surge 3      |     ✔      |      ✔       | surge&ver=3 |
| Surge 4      |     ✔      |      ✔       | surge&ver=4 |
| V2Ray        |     ✔      |      ✔       | v2ray       |

**注意**：Shadowrocket 用户可以使用 `ss`、`ssr`以及 `v2ray`参数

---

## 简易用法

> 即生成的配置文件默认套用 **神机规则**

### 调用地址

```txt
http://127.0.0.1:25500/sub?target=%TARGET%&url=%URL%&config=%CONFIG%
```

### 调用说明

| 调用参数 | 必要性 | 示例      | 解释         |
| ------- | :----: | :------------------- | ---------------- |
| target |  必要  | surge&ver=4   | 指想要生成的配置类型，详见上方 [支持类型](#支持类型) 中的参数 |
| url  |  必要  | https%3A%2F%2Fwww.xxx.com | 指机场所提供的订阅链接，需要经过 [URLEncode](https://www.urlencoder.org/) 处理 |
| config |  可选  | https%3A%2F%2Fwww.xxx.com | 指远程 `pref.ini` (包含分组和规则部分)，需要经过 [URLEncode](https://www.urlencoder.org/) 处理，可查看 [示例仓库](https://github.com/lzdnico/subconverteriniexample) 寻找灵感，默认加载本地设置文件 |

运行 subconverter 主程序后，按照 [调用说明](#调用说明) 的对应内容替换即可得到一份使用**神机规则**的配置文件。

由于此部分篇幅较长，点击下方条目即可展开详解：

<details>
<summary><b>处理单份订阅</b></summary>

如果你需要将一份 Surge 订阅转换成 Clash 的订阅, 可以按以下操作：

```txt
有以下一个订阅，且想转换成 Clash 的订阅:
1. https://dler.cloud/subscribe/ABCDE?surge=ss

首先将订阅通过 URLEncode 后可以得到:
https%3A%2F%2Fdler.cloud%2Fsubscribe%2FABCDE%3Fsurge%3Dss

然后将想要的 %TARGET% (即 clash) 和上一步所得到的 %URL% 填入调用地址中:
http://127.0.0.1:25500/sub?target=clash&url=https%3A%2F%2Fdler.cloud%2Fsubscribe%2FABCDE%3Fsurge%3Dss

最后将该链接填写至 Clash 的订阅处就大功告成了。
```

</details>

<details>
<summary><b>处理多份订阅</b></summary>

如果你需要将多个订阅合成一份, 则要在上方所提及的 URLEncode 之前使用 '|' 来分隔链接, 可以按以下操作：

```txt
有以下两个订阅，且想合并转换成 Clash 的订阅:
1. https://dler.cloud/subscribe/ABCDE?clash=vmess
2. https://rich.cloud/subscribe/ABCDE?clash=vmess

首先使用 '|' 将两个订阅分隔开:
https://dler.cloud/subscribe/ABCDE?clash=vmess|https://rich.cloud/subscribe/ABCDE?clash=vmess

接着通过 URLEncode 后可以得到:
https%3A%2F%2Fdler.cloud%2Fsubscribe%2FABCDE%3Fclash%3Dvmess%7Chttps%3A%2F%2Frich.cloud%2Fsubscribe%2FABCDE%3Fclash%3Dvmess

然后将想要的 %TARGET% (即 clash) 和上一步所得到的 %URL% 填入调用地址中:
http://127.0.0.1:25500/sub?target=clash&url=https%3A%2F%2Fdler.cloud%2Fsubscribe%2FABCDE%3Fclash%3Dvmess%7Chttps%3A%2F%2Frich.cloud%2Fsubscribe%2FABCDE%3Fclash%3Dvmess

最后将该链接填写至 Clash 的订阅处就大功告成了。
```

</details>

<details>
<summary><b>处理单份链接</b></summary>

如果你需要将自建的一条 SS 的 SIP002 链接转换成 Clash 的订阅, 可以按以下操作：

```txt
有以下自建的一条 SS 的 SIP002 链接，且想转换成 Clash 的订阅:
1. ss://YWVzLTEyOC1nY206dGVzdA==@192.168.100.1:8888#Example1

首先将订阅通过 URLEncode 后可以得到:
ss%3A%2F%2FYWVzLTEyOC1nY206dGVzdA%3D%3D%40192%2E168%2E100%2E1%3A8888%23Example1

然后将想要的 %TARGET% (即 clash) 和上一步所得到的 %URL% 填入调用地址中:
http://127.0.0.1:25500/sub?target=clash&url=ss%3A%2F%2FYWVzLTEyOC1nY206dGVzdA%3D%3D%40192%2E168%2E100%2E1%3A8888%23Example1

最后将该链接填写至 Clash 的订阅处就大功告成了。
```

</details>

<details>
<summary><b>处理多份链接</b></summary>

如果你需要将多个链接合成一份, 则要在上方所提及的 URLEncode 之前使用 '|' 来分隔链接, 可以按以下操作：

```txt
有以下两个链接，且想合并转换成 Clash 的订阅:
1. ss://YWVzLTEyOC1nY206dGVzdA==@192.168.100.1:8888#Example1
2. vmess://eyJ2IjoiMiIsInBzIjoidm1lc3MtcHJveHkxIiwiYWRkIjoiZXhhbXBsZS5jb20iLCJwb3J0Ijo0NDMsInR5cGUiOiIiLCJpZCI6IjEyMzQ1Njc4LWFiY2QtMTIzNC0xMjM0LTQ3ZmZjYTBjZTIyOSIsImFpZCI6NDQzLCJuZXQiOiJ3cyIsInBhdGgiOiIvdjIiLCJob3N0IjoiZXhhbXBsZS5jb20iLCJ0bHMiOiJ0bHMifQ==

首先使用 '|' 将两个链接分隔开:
ss://YWVzLTEyOC1nY206dGVzdA==@192.168.100.1:8888#Example1|vmess://eyJ2IjoiMiIsInBzIjoidm1lc3MtcHJveHkxIiwiYWRkIjoiZXhhbXBsZS5jb20iLCJwb3J0Ijo0NDMsInR5cGUiOiIiLCJpZCI6IjEyMzQ1Njc4LWFiY2QtMTIzNC0xMjM0LTQ3ZmZjYTBjZTIyOSIsImFpZCI6NDQzLCJuZXQiOiJ3cyIsInBhdGgiOiIvdjIiLCJob3N0IjoiZXhhbXBsZS5jb20iLCJ0bHMiOiJ0bHMifQ==

接着通过 URLEncode 后可以得到:
ss%3A%2F%2FYWVzLTEyOC1nY206dGVzdA%3D%3D%40192%2E168%2E100%2E1%3A8888%23Example1%7Cvmess%3A%2F%2FeyJ2IjoiMiIsInBzIjoidm1lc3MtcHJveHkxIiwiYWRkIjoiZXhhbXBsZS5jb20iLCJwb3J0Ijo0NDMsInR5cGUiOiIiLCJpZCI6IjEyMzQ1Njc4LWFiY2QtMTIzNC0xMjM0LTQ3ZmZjYTBjZTIyOSIsImFpZCI6NDQzLCJuZXQiOiJ3cyIsInBhdGgiOiIvdjIiLCJob3N0IjoiZXhhbXBsZS5jb20iLCJ0bHMiOiJ0bHMifQ%3D%3D

然后将想要的 %TARGET% (即 clash) 和上一步所得到的 %URL% 填入调用地址中:
http://127.0.0.1:25500/sub?target=clash&url=ss%3A%2F%2FYWVzLTEyOC1nY206dGVzdA%3D%3D%40192%2E168%2E100%2E1%3A8888%23Example1%7Cvmess%3A%2F%2FeyJ2IjoiMiIsInBzIjoidm1lc3MtcHJveHkxIiwiYWRkIjoiZXhhbXBsZS5jb20iLCJwb3J0Ijo0NDMsInR5cGUiOiIiLCJpZCI6IjEyMzQ1Njc4LWFiY2QtMTIzNC0xMjM0LTQ3ZmZjYTBjZTIyOSIsImFpZCI6NDQzLCJuZXQiOiJ3cyIsInBhdGgiOiIvdjIiLCJob3N0IjoiZXhhbXBsZS5jb20iLCJ0bHMiOiJ0bHMifQ%3D%3D

最后将该链接填写至 Clash 的订阅处就大功告成了。
```

</details>

---

## 进阶用法

> 在不满足于本程序所提供的神机规则或者对应的分组时，可以考虑尝试进阶用法
>
> 即 对 `调用地址` 甚至程序目录下的 `pref.ini` 进行个性化的编辑以满足不同的需求

### 阅前提示

在进行下一步操作前，十分推荐您阅读以下内容：

1. 与 `pref.ini` 相关的：[INI 语法介绍](https://zh.wikipedia.org/wiki/INI%E6%96%87%E4%BB%B6)
1. 与 `Clash` 配置相关的： [YAML 语法介绍](https://zh.wikipedia.org/wiki/YAML#%E8%AA%9E%E6%B3%95)
1. 会经常涉及到的： [正则表达式入门](https://github.com/ziishaned/learn-regex/blob/master/translations/README-cn.md)
1. 当遇到问题需要提交 ISSUE 时的： [提问的智慧](https://github.com/ryanhanwu/How-To-Ask-Questions-The-Smart-Way/blob/master/README-zh_CN.md)

当您尝试进行进阶操作时，即默认您有相关的操作能力，本程序仅保证在默认配置文件下能够正常运行。

### 进阶地址

#### 调用地址 (进阶)

```txt
http://127.0.0.1:25500/sub?target=%TARGET%&url=%URL%&emoji=%EMOJI%····
```

#### 调用说明 (进阶)

| 调用参数 | 必要性 | 示例  | 解释   |
| -------- | :----: | :--------------- | :------------------------ |
| target |  必要  | surge&ver=4   | 指想要生成的配置类型，详见上方 [支持类型](#支持类型) 中的参数   |
| url   |  可选  | https%3A%2F%2Fwww.xxx.com | 指机场所提供的订阅链接，需要经过 [URLEncode](https://www.urlencoder.org/) 处理，**可选的前提是在 `default_url` 中进行指定**。也可以使用 data URI    |
| config |  可选  | https%3A%2F%2Fwww.xxx.com | 指远程 `pref.ini` (包含分组和规则部分)，需要经过 [URLEncode](https://www.urlencoder.org/) 处理，可查看 [示例仓库](https://github.com/lzdnico/subconverteriniexample) 寻找灵感，默认加载本地设置文件 |
| upload |  可选  | true / false  | 指将生成的订阅文件上传至 `Gist`，需要填写`gistconf.ini`，默认为 false (即不上传)    |
| upload_path |  可选  | MySS.yaml  | 指将生成的订阅文件上传至 `Gist` 后的名称，需要经过 [URLEncode](https://www.urlencoder.org/) 处理    |
| emoji |  可选  | true / false  | 指在节点名称前加入 Emoji，默认为 true  |
| group |  可选  | MySS  | 指设置该订阅的组名，多用于 SSD/SSR  |
| tfo |  可选  | true / false  | 指开启该订阅链接的 TCP Fast Open，默认为 false  |
| udp |  可选  | true / false  | 指开启该订阅链接的 UDP，默认为 false  |
| scv |  可选  | true / false  | 指关闭 TLS 节点的证书检查，默认为 false  |
| list |  可选  | true / false  | 指输出 Surge Node List 或者 Clash Proxy Provider 或者 Quantumult (X) 的节点订阅 或者 解码后的 SIP002 |
| sort |  可选  | true / false  | 指对输出的节点或策略组进行再次排序，默认为 false  |
| include |  可选  | 详见下文中 `include_remarks`  | 指仅保留匹配到的节点，支持正则匹配，需要经过 [URLEncode](https://www.urlencoder.org/) 处理，会覆盖配置文件里的设置  |
| exclude |  可选  | 详见下文中 `exclude_remarks`  | 指排除匹配到的节点，支持正则匹配，需要经过 [URLEncode](https://www.urlencoder.org/) 处理，会覆盖配置文件里的设置  |

举个例子：

```txt
有订阅 `https://dler.cloud/subscribe/ABCDE?clash=vmess`，想转换成 Surge 4 的订阅，且需要开启 TFO 和 UDP
顺便再给节点名加上 EMOJI 同时排除掉订阅中显示流量和官网的节点（节点名为"剩余流量：1024G"，"官网地址：dler.cloud"）

首先确认需要用到的参数：
target=surge&ver=4 、 tfo=true 、 udp=true 、 emoji=true 、exclude=(流量|官网)
url=https://dler.cloud/subscribe/ABCDE?clash=vmess

然后将需要 URLEncode 的部分进行处理：
exclude=%28%E6%B5%81%E9%87%8F%7C%E5%AE%98%E7%BD%91%29
url=https%3A%2F%2Fdler.cloud%2Fsubscribe%2FABCDE%3Fclash%3Dvmess

接着将所有元素进行拼接：
http://127.0.0.1:25500/sub?target=surge&ver=4&tfo=true&udp=true&emoji=true&exclude=%28%E6%B5%81%E9%87%8F%7C%E5%AE%98%E7%BD%91%29&url=https%3A%2F%2Fdler.cloud%2Fsubscribe%2FABCDE%3Fclash%3Dvmess

最后将该链接填写至 Surge 的订阅处就大功告成了。
```

### 配置文件

> 关于 subconverter 主程序目录中 `pref.ini` 文件的解释

由于此部分篇幅较长，点击下方条目即可展开详解：

<details>
<summary><b>[common] 部分</b></summary>

> 该部分主要涉及到的内容为 **全局的节点排除或保留** 、**各配置文件的基础**
>
> 其他设置项目可以保持默认或者在知晓作用的前提下进行修改

1. **api_mode**

    > API 模式，设置为 true 以防止直接加载本地订阅或直接提供本地文件。（多用于架设于服务器上）

    - 当值为 `false` 时, 每次更新配置都会读取 `pref.ini` , 为 `true` 时则仅启动时读取。

1. **default_url**

    > 无 %URL% 参数时，默认加载的订阅链接， **不需要 URLEncode**。 如果有多个链接，仍然需要使用 "|" 分隔，支持`文件`/`url`

    - 例如:

     ```ini
     default_url='https://dler.cloud/subscribe/ABCDE?clash=vmess'
     ```

    - 解释：

     ```txt
     此时订阅链接:
     http://127.0.0.1:25500/sub?target=clash
     等同于:
     http://127.0.0.1:25500/sub?target=clash&url=https%3A%2F%2Fdler.cloud%2Fsubscribe%2FABCDE%3Fclash%3Dvmess
     ```

1. **exclude_remarks**

   > 排除匹配到的节点，支持正则匹配

    - 例如:

     ```ini
     exclude_remarks=(流量|时间|官网|产品)
     ```

1. **include_remarks**

   > 仅保留匹配到的节点，支持正则匹配

    - 例如:

     ```ini
     include_remarks=(?<=美).*(BGP|GIA|IPLC)
     ```

1. **clash_rule_base**

   > 生成的 Clash 配置文件基础。支持 `本地文件` 和 `在线URL`

    - 例如:

     ```ini
     clash_rule_base=clash.yaml # 加载本地的 clash.yaml 文件作为基础
     # 或者
     clash_rule_base=https://raw.githubusercontent.com/ConnersHua/Profiles/master/Clash/Pro.yaml
     # 加载神机的 Github 中相关文件作为基础
     ```

1. **surge_rule_base**

   > 生成的 Surge 配置文件基础，用法同上

1. **surfboard_rule_base**

   > 生成的 Surfboard 配置文件基础，用法同上

1. **mellow_rule_base**

   > 生成的 Mellow 配置文件基础，用法同上

1. **proxy_ruleset**

   > 更新 RuleSet 时是否使用代理
   >
   > 填写 `NONE` 或者空白禁用，或者填写 `SYSTEM` 使用系统代理
   >
   > 也可填写如同 `socks5://127.0.0.1:1080` 的 HTTP 或 SOCKS 代理

    - 例如:

     ```ini
     proxy_ruleset=SYSTEM # 使用系统代理
     # 或者
     proxy_ruleset=socks5://127.0.0.1:1080 # 使用本地的 1080 端口进行 SOCKS5 代理
     ```

1. **proxy_subscription**

   > 更新 原始订阅 时是否使用代理，用法同上

1. **append_proxy_type**

   > 节点名称是否需要加入属性，设置为 true 时在节点名称前加入 \[SS\] \[SSR\] \[VMess\] 以作区别，
   >
   > 默认为 false

    - 例如（设置为 true时）：

     ```txt
     [SS] 香港中转
     [VMess] 美国 GIA
     ```

</details>
<details>
<summary><b>[node_pref] 部分</b></summary>

> 该部分主要涉及到的内容为 **开启节点的 UDP 及 TCP Fast Open** 、**节点的重命名** 、**重命名节点后的排序**
>
> 相关设置项目建议保持默认或者在知晓作用的前提下进行修改

1. **udp_flag**

   > 为节点打开 UDP 模式，设置为 true 时打开，默认为 false

    - 当不清楚机场的设置时**请勿调整此项**。

1. **tcp_fast_open_flag**

   > 为节点打开 TFO (TCP Fast Open) 模式，设置为 true 时打开，默认为 false

    - 当不清楚机场的设置时**请勿调整此项**。

1. **sort_flag**

   > 对生成的订阅中的节点进行 A-Z 的排序，设置为 true 时打开，默认为 false

1. **skip_cert_verify_flag**

   > 关闭 TLS 节点的证书检查。设置为 true 时打开，默认为 false

    - **请勿随意将此设置修改为 true**

1. **rename_node**

   > 重命名节点，支持正则匹配
   >
   > 使用方式：原始命名@重命名

    - 例如:

     ```ini
     rename_node=中国@中
     rename_node=\(?((x|X)?(\d+)(\.?\d+)?)((\s?倍率?:?)|(x|X))\)?@(倍率:$1)
     ```

</details>
<details>
<summary><b>[managed_config] 部分</b></summary>

> 该部分主要涉及到的内容为 **订阅文件的更新地址**

1. **write_managed_config**

   > 是否将 '#!MANAGED-CONFIG' 信息附加到 Surge 或 Surfboard 配置，设置为 true 时打开，默认为 true

1. **managed_config_prefix**

   > 具体的 '#!MANAGED-CONFIG' 信息，地址前缀不用添加 "/"。Surge 或 Surfboard 会向此地址发出更新请求
   >
   > 局域网用户需要将此处改为本程序运行设备的局域网 IP

    - 例如:

    ```ini
    managed_config_prefix = http://192.168.1.5:25500
    ```

</details>
<details>
<summary><b>[surge_external_proxy] 部分</b></summary>

> 为 Surge 添加 SSR 的支持路径

</details>
<details>
<summary><b>[emojis] 部分</b></summary>

1. **add_emoji**

   > 是否在节点名称前加入下面自定义的 Emoji，设置为 true 时打开，默认为 true

1. **remove_old_emoji**

   > 是否移除原有订阅中存在的 Emoji，设置为 true 时打开，默认为 true

1. **rule**

   > 在匹配到的节点前添加自定义 emojis，支持正则匹配

    - 例如:

    ```ini
    rule=(流量|时间|应急),⌛time
    rule=(美|美国|United States),🇺🇸
    ```

</details>
<details>
<summary><b>[ruleset] 部分</b></summary>

> 如果你对原本订阅自带的规则不满意时，可以使用如下配置

1. **enabled**

   > 启用自定义规则集的**总开关**，设置为 true 时打开，默认为 true

1. **overwrite_original_rules**

   > 覆盖原有规则，即 [common] 中 xxx_rule_base 中的内容，设置为 true 时打开，默认为 false

1. **update_ruleset_on_request**

   > 根据请求执行规则集更新，设置为 true 时打开，默认为 false

1. **surge_ruleset**

   > 从本地或 url 获取规则片段
   >
   > [] 前缀后的文字将被当作规则，而不是链接或路径，主要包含 `[]GEOIP` 和 `[]MATCH`(等同于 `[]FINAL`)。

    - 例如：

    ```ini
    surge_ruleset=🍎 苹果服务,https://raw.githubusercontent.com/ConnersHua/Profiles/master/Surge/Ruleset/Apple.list
    # 表示引用 https://raw.githubusercontent.com/ConnersHua/Profiles/master/Surge/Ruleset/Apple.list 规则
    # 且将此规则指向 [clash_proxy_group] 所设置 🍎 苹果服务 策略组
    surge_ruleset=🎯 全球直连,rules/NobyDa/Surge/Download.list
    # 表示引用本地 rules/NobyDa/Surge/Download.list 规则
    # 且将此规则指向 [clash_proxy_group] 所设置 🎯 全球直连 策略组
    surge_ruleset=🎯 全球直连,[]GEOIP,CN
    # 表示引用 GEOIP 中关于中国的所有 IP
    # 且将此规则指向 [clash_proxy_group] 所设置 🎯 全球直连 策略组
    ```

</details>
<details>
<summary><b>[clash_proxy_group] 部分</b></summary>

> 为 Clash 、Mellow 、Surge 以及 Surfboard 等程序创建策略组, 可用正则来筛选节点
>
> [] 前缀后的文字将被当作引用策略组

```ini
custom_proxy_group=🍎 苹果服务`url-test`(美国|US)`http://www.gstatic.com/generate_204`300
# 表示创建一个叫 🍎 苹果服务 的 url-test 策略组,并向其中添加名字含'美国','US'的节点，每隔300秒测试一次
custom_proxy_group=🇯🇵 日本延迟最低`url-test`(日|JP)`http://www.gstatic.com/generate_204`300
# 表示创建一个叫 🇯🇵 日本延迟最低 的 url-test 策略组,并向其中添加名字含'日','JP'的节点，每隔300秒测试一次
custom_proxy_group=🇯🇵 JP`select`沪日`日本`[]🇯🇵 日本延迟最低
# 表示创建一个叫 🇯🇵 JP 的 select 策略组,并向其中**依次**添加名字含'沪日','日本'的节点，以及引用上述所创建的 🇯🇵 日本延迟最低 策略组
```

- 还可使用一些特殊筛选条件

  ```ini
  custom_proxy_group=g1`select`!!GROUPID=0
  # 指订阅链接中的第一条订阅
  custom_proxy_group=g2`select`!!GROUPID=1
  # 指订阅链接中的第二条订阅
  custom_proxy_group=v2ray`select`!!GROUP=V2RayProvider
  # 指订阅链接中组名为 V2RayProvider 的节点
  ```

- 现在也可以使用双条件进行筛选

  ```ini
  custom_proxy_group=g1hk`select`!!GROUPID=0!!(HGC|HKBN|PCCW|HKT|hk|港)
  # 订阅链接中的第一条订阅内名字含 HGC、HKBN、PCCW、HKT、hk、港 的节点
  ```

</details>
<details>
<summary><b>[server] 部分</b></summary>

> 此部分通常**保持默认**即可

1. **listen**

   > 绑定到 Web 服务器的地址，将地址设为 0.0.0.0，则局域网内设备均可使用。

1. **port**

   > 绑定到 Web 服务器地址的端口，默认为 25500

</details>

<details>

<summary><b>[advanced] 部分</b></summary>

> 此部分通常**保持默认**即可

</details>

### 外部配置

> 本部分用于 链接参数 `**&config=**`

将文件按照以下格式写好，上传至 Github Gist 或者 其他**可访问**网络位置
经过 [URLEncode](https://www.urlencoder.org/) 处理后，添加至 `&config=` 即可调用
需要注意的是，由外部配置中所定义的值会**覆盖** `pref.ini` 里的内容
即，如果你在外部配置中定义了 

```
emoji=(流量|时间|应急),🏳️‍🌈
emoji=阿根廷,🇦🇷
```

那么本程序只会匹配以上两个 Emoji，不再使用 `pref.ini` 中所定义的 国别 Emoji

<details>
<summary><b>点击查看文件内容</b></summary>

```ini
[custom]
;这是一个外部配置文件示例
;所有可能的自定义设置如下所示

;用于自定义组的选项 会覆盖 pref.ini 里的内容
;使用以下模式生成 Clash 代理组，带有 "[]" 前缀将直接添加
;Format: Group_Name`select`Rule_1`Rule_2`...
;        Group_Name`url-test|fallback|load-balance`Rule_1`Rule_2`...`test_url`interval
;Rule with "[]" prefix will be added directly.

custom_proxy_group=Proxy`select`.*`[]AUTO`[]DIRECT`.*
custom_proxy_group=UrlTest`url-test`.*`http://www.gstatic.com/generate_204`300
custom_proxy_group=FallBack`fallback`.*`http://www.gstatic.com/generate_204`300
custom_proxy_group=LoadBalance`load-balance`.*`http://www.gstatic.com/generate_204`300

;custom_proxy_group=g1`select`!!GROUPID=0
;custom_proxy_group=g2`select`!!GROUPID=1
;custom_proxy_group=v2ray`select`!!GROUP=V2RayProvider

;custom_proxy_group=g1hk`select`!!GROUPID=0!!(HGC|HKBN|PCCW|HKT|hk|港)
;custom_proxy_group=sstw`select`!!GROUP=V2RayProvider!!(深台|彰化|新北|台|tw)


;用于自定义规则的选项 会覆盖 pref.ini 里的内容
;Ruleset addresses, supports local files/URL
;Format: Group name,URL
;        Group name,[]Rule
enable_rule_generator=false
overwrite_original_rules=false
;surge_ruleset=DIRECT,https://raw.githubusercontent.com/ConnersHua/Profiles/master/Surge/Ruleset/Unbreak.list
;surge_ruleset=🎯 全球直连,rules/LocalAreaNetwork.list
;surge_ruleset=🎯 全球直连,[]GEOIP,CN
;surge_ruleset=🐟 漏网之鱼,[]FINAL

;用于自定义基础配置的选项 会覆盖 pref.ini 里的内容
clash_rule_base=base/forcerule.yml
;surge_rule_base=base/surge.conf
;surfboard_rule_base=base/surfboard.conf
;mellow_rule_base=base/mellow.conf
;quan_rule_base=base/quan.conf
;quanx_rule_base=base/quanx.conf

;用于自定义重命名的选项 会覆盖 pref.ini 里的内容
;rename=Test-(.*?)-(.*?)-(.*?)\((.*?)\)@\1\4x测试线路_自\2到\3
;rename=\(?((x|X)?(\d+)(\.?\d+)?)((\s?倍率?)|(x|X))\)?@$1x

;用于自定义 Emoji 的选项 会覆盖 pref.ini 里的内容
;emoji=(流量|时间|应急),🏳️‍🌈
;emoji=阿根廷,🇦🇷

```

</details>

## 自动上传

> 自动上传 gist ，可以用于 Clash For Android / Surge 等进行远程订阅

在程序目录内的 [gistconf.ini](./base/gistconf.ini) 中添加 `Personal Access Token`（[在此创建](https://github.com/settings/tokens/new?scopes=gist&description=Subconverter)）例如：

```ini
[common]
;uncomment the following line and enter your token to enable upload function
token = xxxxxxxxxxxxxxxxxxxxxxxx(所生成的 Personal Access Token)
```

在 [调用地址](#调用地址) 或 [调用地址 (进阶)](#调用地址-进阶) 所生成的链接后加上 `&upload=true` 就会在更新好后自动上传 gist
此时，subconverter 程序窗口内会出现如下所示的**神秘代码**：

```cmd
No gist id is provided. Creating new gist...
Writing to Gist success!
Generator: surge4
Path: surge4
Raw URL: https://gist.githubusercontent.com/xxxx/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/raw/surge4
Gist owner: xxxx
```

上方所提到的 `Raw URL: https://gist.githubusercontent.com/xxxx/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/raw/surge4`
中的 `https://gist.githubusercontent.com/xxxx/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/raw/surge4` 即是你的在线订阅链接。

注意，本程序默认会将此链接设为**秘密状态**

根据 [`官方手册 - 创建 Gist`](https://help.github.com/cn/github/writing-on-github/creating-gists) 的解释为：

> 秘密 gists 不会显示在 Discover 中，也不可搜索。
>
> 秘密 gists 不是私人的。 如果将秘密 gist 的 URL 发送给朋友，他们可以查看。
>
> 但是，如果您不认识的人发现该 URL，也能看到您的 gist。

所以请务必保管好所生成的 `Raw URL` 链接。
