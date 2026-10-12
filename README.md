# 小篆输入方案 · for Rime

在 **Windows（小狼毫）** 和 **安卓（同文）** 上用拼音打小篆 —— 候选窗以篆形显示。

> 这里是 [`xiaozhuan-ime`](https://github.com/Ivan666jjj/xiaozhuan-ime) 的 **Rime 方案**部分。
> macOS 用户请用那个仓库的**原生输入法**（体验更完整：候选窗自绘、字号可调）。

```
敲 shui  ──►  候选窗以篆形显示【水 説 稅 睡 誰】  ──►  上屏普通汉字「水」
```

## 原理

**上屏的是普通汉字，靠字体把候选窗渲染成篆形。**

- 装了 `ShuowenSeal.ttf` → 候选窗里看到**篆书**
- 没装 → 看到**楷体汉字**，照样读得懂

所以打出来的字**能复制、能搜索、能发给别人**，不会因为换台电脑就变豆腐块。

## 一行安装（推荐，需先装好 Rime）

```bash
bash rime-install Ivan666jjj/rime-xiaozhuan
```

> 没装过 plum（东风破）的话，先跑这条：
> `curl -fsSL https://raw.githubusercontent.com/rime/plum/master/rime-install | bash`
> （Windows 用 `rime-install.bat`，见 [东风破文档](https://github.com/rime/plum)）

**⚠️ 注意**：plum 只负责**把方案文件放进去**，**不会替你启用方案**（这是它的设计）。
装完还要做一步 —— **重新部署一次**，然后在方案选单里选「小篆」。

**想让它出现在默认方案列表里？** 在你自己的 `default.custom.yaml` 里加：

```yaml
patch:
  schema_list/+:           # ← 注意这个 /+，是「追加」
    - schema: shuowen
```

> ⚠️ **`/+` 不能省**。Rime 的 patch 默认是「**整体替换**」——
> 写成 `schema_list:` 的话，你原有的其他方案会**全部从列表里消失**。
> `/+` 的语义才是「追加，保留原有」。

> 仓库里那份 `default.custom.yaml.example` 是**给你参考的样例**，
> **不要直接复制覆盖** —— 那样会把你已有的方案列表清掉。
>
> 不想用命令行的话，下面「手工安装」的步骤一样有效。

---

## 先拿字体

**本仓库不含字体**（25.8 MB，放仓库太占地方）。到主仓库的 Release 页下载：

```
https://github.com/Ivan666jjj/xiaozhuan-ime/releases/latest
    → xiaozhuan-ime-Rime-v1.0.3.zip      ← 里面就有 ShuowenSeal.ttf
```

> 🌏 **打不开 github.com？** 网址前面加 `ghfast.top/`：
> `https://ghfast.top/https://github.com/Ivan666jjj/xiaozhuan-ime/releases/latest`

方案文件（本仓库这些）clone 或下载 ZIP 即可。

## 文件清单

| 文件 | 作用 |
|---|---|
| `shuowen.schema.yaml` | 输入方案（含候选窗字体设置） |
| `shuowen.dict.yaml` | 词典：408 个拼音音节 / 8,631 个篆字 |
| `trime.custom.yaml` | 安卓端：指定候选窗字体 |
| `recipe.yaml` | 东风破（plum）配方，供 `rime-install` 用 |
| `default.custom.yaml.example` | **参考样例**，看它怎么把方案挂进列表 |

---

## Windows：小狼毫（Weasel）

**① 装输入法** —— 从 [rime.im/download](https://rime.im/download/) 下**小狼毫**，装好。

**② 装字体** —— 右键 `ShuowenSeal.ttf` → **为所有用户安装**。
（不装也能用，只是候选窗显示楷体。）

**③ 放配置** —— 右键任务栏的小狼毫图标 → **用户文件夹**（会打开 `%APPDATA%\Rime`），
把 `shuowen.schema.yaml`、`shuowen.dict.yaml` 两个文件复制进去。
（想让「小篆」出现在方案选单里，参考 `default.custom.yaml.example`，
在**你自己的** `default.custom.yaml` 里加一行 `- schema: shuowen`。）

**④ 重新部署** —— 右键图标 → **重新部署**（等它转完，词典 8,631 条要跑几秒）。

**⑤ 切换** —— 按 **Ctrl + `**（反引号）或 **F4** 打开方案菜单，选「**小篆**」，然后敲拼音。

> **候选窗还是楷体？** 说明字体没装上，或 `font_face` 名字对不上。
> 确认字体在「设置 → 个性化 → 字体」里显示为 **Shuowen Seal**，改完要**重新部署**。

---

## 安卓：同文输入法（Trime）

**① 装输入法** —— 从 [GitHub Releases](https://github.com/osfans/trime/releases) 或应用商店装**同文输入法**。

**② 放字体**（安卓装不了系统字体，要给 Trime 单独准备一份）

1. 打开同文输入法 → **配置管理 → 用户文件夹**（一般是 `/storage/emulated/0/rime`）
2. 在里面**新建一个 `fonts` 文件夹**（必须叫这个名字）
3. 把 `ShuowenSeal.ttf` 复制进 `rime/fonts/`

**③ 放配置** —— 把 `shuowen.schema.yaml`、`shuowen.dict.yaml`、`trime.custom.yaml`
复制到 `rime/` 目录（跟 `fonts` 同级）。

**④ 部署** —— 同文输入法 → **配置管理 → 重新部署**。

**⑤ 启用** —— 系统设置 → 语言和输入法 → 启用「同文输入法」并切换过去；
在同文里按方案菜单选「**小篆**」，敲拼音。

> **候选窗还是楷体？** 三个常见原因：
> ① 字体没放进 `rime/fonts/`（或文件夹名写错）；
> ② `trime.custom.yaml` 里的文件名跟实际文件**不完全一致**（大小写、扩展名都算）；
> ③ 放完没重新部署。

---

## 两个平台的差异

| | Windows（小狼毫） | 安卓（同文） |
|---|---|---|
| 候选窗字体键名 | `style/font_face` | `style/candidate_font` |
| 字体怎么装 | 右键安装到系统 | 放进 `rime/fonts/` |
| 配置目录 | `%APPDATA%\Rime` | `/storage/emulated/0/rime` |
| 生效方式 | 重新部署 | 重新部署 |

## 覆盖率：哪些字打不出篆形

字体只收《说文》系统里的篆形，**不是每个现代汉字都有**。实测：

| 范围 | 有篆形 |
|---|---|
| **人名常用字** | **97%**（71/73） |
| **百家姓（前 60）** | **94%**（56/60） |
| GB2312 常用字 | 72.2%（4,880 / 6,763） |

**没篆形的大致四类**：

| 类型 | 例子 | 说明 |
|---|---|---|
| **后起字** | 娜、辉、估、住、花 | 《说文》成书时还没有 |
| **古今字** | 他（《说文》作「佗」）、你（本字「爾」） | 篆形挂在古字头下 |
| **异体字** | 丐→匃、坑→阬、碗→盌 | 篆形在异体字那一笔 |
| **超 BMP 字头** | 㐹、㑂、𠆺 | 在扩展 B/C 区，本字体是 BMP 设计 |

**打不出来时不会变豆腐块** —— 会 fallback 到系统字体，**显示成楷体**。
所以打「娜」出来的是普通的「娜」，一眼能看出这个字没有篆形。

## 键盘用法

| 按键 | 作用 |
|---|---|
| 拼音字母 | 输入（如 `shui`） |
| `1` ~ `9` | 选第 N 个候选 |
| 空格 | 选第一个候选 |
| `Ctrl + \`` / `F4` | 切换输入方案 |
| `Shift` | 临时切到西文 |

## 已知限制

- **候选窗样式是 Rime 的**，不能像 macOS 版那样自绘（那边字号、每行候选数都能调）
- **候选窗里的篆形依赖字体**：安卓上如果字体没放对，看到的就是楷体
- **词典只有单字**（8,631 个），没有词组联想 —— 跟 macOS 版保持一致

## 字形来源与授权

字形轮廓不是本方案绘制的，来自两处（**不含汉典字形**，那是仅个人使用的授权）：

| 来源 | 授权 | 占比 |
|---|---|---|
| [开元小篆](https://github.com/frankslin/kaiyuan-small-seal-font) | SIL OFL 1.1 | ~87% |
| 台湾全字库说文解字 | 政府资料开放授权条款第 1 版（OGDL-Taiwan-1.0） | ~13% |

全字库属 OGDL，**显名是强制义务**（明定「未尽显名标示义务者，视为自始未取得授权」）。
分享本方案时（候选窗截图、介绍页、群里说明都算）请带上这句：

> 數位發展部，CNS11643中文標準交換碼全字庫網站，https://www.cns11643.gov.tw

方案文件本身是 MIT。详见主仓库的
[`LICENSE-NOTES.md`](https://github.com/Ivan666jjj/xiaozhuan-ime/blob/main/LICENSE-NOTES.md)
与 [`字形来源与授权.md`](https://github.com/Ivan666jjj/xiaozhuan-ime/blob/main/packaging/字形来源与授权.md)。
