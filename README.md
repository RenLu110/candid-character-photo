# 偶遇抓拍 · Candid Character Photo

给一张人物参考，或描述一个成年人物，让 Codex 生成具有观察距离、自然动作和前景遮挡的日常模拟抓拍照片。

> v0.1.0，包含经过rc2复测的远景与主体占比控制。本仓库提供 Skill 指令与配方，不包含生图模型、账号或额度。需要当前 Codex 会话具有可用的内置 image_gen 工具。它生成虚构摆拍效果，不是实际事件的偷拍记录。

| 书店观察 | 手机随拍 | 咖啡店隔窗 |
|---|---|---|
| ![书店](examples/01-bookstore.png) | ![街道](examples/02-street-phone.png) | ![咖啡店](examples/03-cafe-window.png) |

以上为同一原创成年人物的初版中近景测试输出。[查看提示词与保留项](examples/README.md)。

rc2新增的远景独立测试：

![公园远景：人物较小，近处栏杆与远处长椅形成距离](examples/04-park-wide.png)

## 使用

安装后，附上照片并说：

```text
用 $candid-character-photo 根据参考人物，随机生成三张不同场景的偷拍感图片。
```

也可以直接描述人物，或指定成年角色：

```text
用 $candid-character-photo，生成一位30岁短发女性的三张日常抓拍。
穿蓝色开衫、白T恤、牛仔裤，其中一张像手机随手拍。
```

```text
用 $candid-character-photo 生成成年夏洛克·福尔摩斯在公共旧书市场的偶遇抓拍。
```

```text
用 $candid-character-photo，只给提示词，不生成图片：成年男性，红夹克，公园，n=2。
```

默认一张、3:4竖图、完整着装、真人摄影，优先环境中远景。三张组图默认各有一张远景、中远景、中景；五张以上至少三分之一安排远景。同组保留人物和服装。用户指定全近景或全远景时优先遵循。

想要更强距离感，可直接说“远景、人物小一点、偷拍感重一点”。远景的人物高度通常占画幅约20%–35%，环境成为主要内容；全身入画不等于远景。远处五官信息会减少，不靠推近人物换取毛孔级清晰度。

## 本地安装

可以直接将下面这句话发给有技能安装能力的 Codex：

```text
请安装 GitHub 仓库 RenLu110/candid-character-photo 中的 skills/candid-character-photo，使用 v0.1.0 标签。
```

下载仓库 ZIP 并解压，将整个 `skills/candid-character-photo` 文件夹复制到 Codex 的技能目录：默认 `~/.codex/skills/`，设置了 `CODEX_HOME` 时用该目录下的 `skills/`。不要把整个仓库当作 Skill 文件夹。

在仓库根目录运行以下任一命令。目标已存在时会停止，避免覆盖已有版本。

PowerShell：

```powershell
$skillHome = if ($env:CODEX_HOME) { Join-Path $env:CODEX_HOME 'skills' } else { Join-Path $env:USERPROFILE '.codex/skills' }
$skillTarget = Join-Path $skillHome 'candid-character-photo'
if (Test-Path -LiteralPath $skillTarget) { throw '目标已存在，请先备份或选择其他测试目录。' }
New-Item -ItemType Directory -Force -Path $skillHome | Out-Null
Copy-Item -LiteralPath './skills/candid-character-photo' -Destination $skillTarget -Recurse
```

macOS / Linux：

```bash
skill_home="${CODEX_HOME:-$HOME/.codex}/skills"
skill_target="$skill_home/candid-character-photo"
if [ -e "$skill_target" ]; then
  echo 'Target exists; back it up before updating.' >&2
else
  mkdir -p "$skill_home"
  cp -R ./skills/candid-character-photo "$skill_target"
fi
```

复制后在下一轮会话尝试调用。若尚未出现在技能列表，可新建会话后再试。安装 Skill 不会自动启用生图工具。

## 如何保持效果

- 提供清晰、角度互补的同人物参考，说明哪张用于服装；避免把不同人物混在一起。
- 需要连续组图时保留原始参考与生成记录，在同一会话追加“再来三张”。
- 支持书店、地铁、咖啡店、街角、公园、展览馆等公共日常环境。
- 默认有距离的日常观察。手机风格保留更多环境和较深景深；长焦风格强调距离与空间压缩。远景通过近处障碍物、中间空间、远处人物形成层次，不只在近景人像旁加模糊黑条。
- 每张图片独立生成并看图检查。技术失败最多重试一次，视觉问题最多定向修正一次，成功图片不随失败图片重跑。

## 测试与限制

见 [测试报告](docs/TEST-REPORT.md) 与 [测试方法](docs/TESTING.md)。样例是 AI 生成的虚构场景。小样本的可用结果不是所有人物、角色或模型的成功率保证。

生图可能出现身份漂移、道具／手部错误、背景文字错误；非常遮挡的画面会牺牲辨识度。只有角色名时的相貌是演绎，并非对某张固定人像的复刻。当前不承诺其他图像模型的同等效果。

本仓库不包含用户提交的人物原图、私人绝对路径、API 密钥或第三方照片。生成记录默认留在用户自己的任务里，不会自动上传。

## 内容

```text
skills/candid-character-photo/
  SKILL.md
  agents/openai.yaml
  references/scene-recipes.md
  references/generation.md
  references/quality.md
docs/
  TESTING.md
  TEST-REPORT.md
examples/
  README.md
  01-bookstore.png / 01-prompt.txt
  02-street-phone.png / 02-prompt.txt
  03-cafe-window.png / 03-prompt.txt
  04-park-wide.png / 04-prompt.txt
```

Skill 指令、配方与代码采用 [MIT License](LICENSE)。示例图片不纳入该软件许可证，不应被视为现实人物或事件的证明。
