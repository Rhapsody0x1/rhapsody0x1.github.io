---
title: CodexClaw 开发之外的碎碎念
description: 聊聊 Vibe Coding，Agent，和龙虾类似物
slug: about-codex-claw
date: 2026-05-12 11:29:06+0000
image: cover.jpg
categories:
  - Blog
tags:
  - Vibe Coding
  - Agent
  - Codex
weight: 1
---

最近一年内，各路编程代理 (Code Agent) 以不可思议的速度迭代升级。现在，许多 Code Agent 都能独立完成相当复杂的开发任务，甚至在运维工作中独当一面。笔者受此鼓舞，尝试在 Codex 的基础上构建了一个自己的通用型的、可通过即时通讯 (IM) 软件访问的通用型助手 [CodexClaw](https://github.com/Rhapsody0x1/CodexClaw)。在开发过程中笔者有不少观察和由此引发的思考，遂久违地来写一篇博客作为记录。

## 引子

当年，ChatGPT 出色的对话能力从海量语料的训练中涌现，LLM 的逻辑推理能力从语言能力中涌现，编程能力从逻辑推理能力中衍生，最终，一个具有出色代码能力的 LLM，就能通过外部的框架 (Harness)，摇身一变成为一个能处理各种通用任务的 Agent。例如，现在的很多 Agent 能通过 Playwright 控制浏览器，通过 Python 代码做数据分析或者编辑 Word、PPT 文档。再进一步，我们让 Agent 能 24 小时不间断地运行，拥有一些长期记忆，再让它能从我们最常用的聊天软件平台访问，就得到了 OpenClaw。

笔者作为常年上网冲浪 AI 新闻个个不落的赛博生物，自然是没有错过 OpenClaw 爆火的大事件。说实话，看到 OpenClaw 这玩意的第一眼笔者就在怀疑其是否真的有使用价值。本着我~~连母鸡卡都能看完还有什么石赤不下去的~~赤石英雄精神，笔者还是安装并一顿倒腾尝试把好几家不同提供商的模型接入了 OpenClaw，然后很快就发现了问题：

- 你很难知道你发的一条新消息有没有让 OpenClaw 挂掉；
- 你很难知道到底是 LLM 提供商抽风了还是 OpenClaw 抽风了；
- 你不知道 OpenClaw 会不会趁你不注意就给你来个 `sudo rm -rf *`;
- 抛开连接 IM 平台的功能不谈，笔者没有感觉到这玩意比 Claude Code 或者 Codex 更好；
  - 记忆功能事实上也做得很烂；
  - Skill 也不是什么原创的东西，一个相同的 Skill 在后两者上很可能表现得更好；
- OpenClaw 真的非常烧 token，笔者用不起；
  - Peter 能做出这个大概是他有量大管饱的 Claude Max 用，笔者实名羡慕 : (
- 最重要的一点，OpenClaw 作为一个具有**极高权限**的 Agent，它实际工作的代码/逻辑已经没有人类能搞得懂了；
  - 事实上它现在也已经是个完全只能由各路 Agent 来贡献的不可名状之史山了。

总而言之，由于以上种种不靠谱的因素均表明它（正如 Peter 本人所说）只是个 token 富哥的玩具，OpenClaw 在笔者的电脑上仅运行了不到 4 小时就被作者带着配置文件和各种数据扫地出门了。

## 寻思

### 为什么 OpenClaw 能火呢？

虽然从笔者的软件工程专业视角来说，OpenClaw 在各种意义上都烂透了，但既然能火那一定有它的理由。所以俺一寻思大概是有这么几个亮点。

**第一，OpenClaw 把 Agent 接入了 IM。**

笔者从 CoolQ 时代就已经在研究各种 IM 上的非官方机器人了，但事实上这一直是个很小众的领域。即使后来 LLM 出现，运行于 IM 上的聊天机器人也很难说有什么特别实际的用途。但 Agent 作为有**真正使用价值**的工具，其爆火其实是早有先例的——也就是 Manus。相信你也听说过当年 Manus 的“天价邀请码”，“AI 的未来”之类的的新闻。从另一个角度来说，如果能把 Manus 云端 Linux 沙盒环境改成个人 PC 环境，把它的网页端/App 端 Chat 界面换成一个 IM，就得到了 OpenClaw。所以，OpenClaw 的“惊艳”之处是，把一个看上去**“有用”**的 Agent，接入了和**你的日常工作最相关**的 IM 软件上。

**第二，OpenClaw 让 Agent 实现了“主动工作”**

虽然有点取巧又有点标题党，但 OpenClaw 确实是通过一个**定时任务系统**，让 Agent 看上去能“随时待命”并“主动地”处理工作。这很符合大众对”贾维斯“式的通用 AI 助手的想象：能帮我处理邮件，撰写文档，随手处理发来的数据，收集新闻以给出投资建议，审 PR 和 Issue... 总之，即使 OpenClaw 不是一个会被各种服务主动推送消息（比如通过 WebHook）的平台，但它依然可以通过定时任务，也就是某种意义上的**轮询**来主动找活干。

**第三，OpenClaw 降低了很多方面的门槛**

这里倒不是说 OpenClaw 是一个很容易安装的 Agent。其实它作为一个 vibe 出来的项目，很多配置的地方都是需要用户自己有一定了解才能配好的。但是它的**使用门槛**是实打实地降低了。Codex、Claude Code 要求你对着一个终端的 TUI 来执行任务，Claude Desktop 之类的软件依然是比较传统的 LLM 对话框，而 OpenClaw 接入 IM，**IM 反而是普通非技术背景用户最熟悉的 UI**。所以 OpenClaw 能给很多非技术背景的普通用户以很高的亲切感。

而从开发者和研究者的角度来讲，OpenClaw 是一个 **Harness**，很多软件工程师比起 LLM 本身都更了解 Harness，这里不是指 Harness 的概念，是指 Harness 里做的事情，比如接入聊天软件，从各种信息源拉取数据等；对于 LLM 研究者，研究 Harness 所需的各种成本肯定低于训练/微调一个 LLM。所以 OpenClaw 对他们来说看上去也是更“亲切”的。

最后，可能稍微带了点阴谋论，OpenClaw 还降低了 LLM 提供商卖 Token 的门槛。在 OpenClaw 这种产品出现之前，LLM 提供商的 Token 基本都是卖给开发者，现在他们更容易把 Token 卖给普通用户了。

### 所以应该做什么？

从现在的产品角度看，Claude 推出 Cowork，OpenAI 推出 Codex App，国内的各家 LLM 提供商也推出了自己的 Claw，腾讯推出了 WorkBuddy——大厂希望把**高使用门槛的、专业向的、具有很强通用能力的** Code Agent 变成一个能卖给更多普通用户的**低门槛的、平民向的、处理日常任务**的通用 Agent。诸如 KimiClaw，QClaw 之类的产品都有桌面化应用、开箱即用的特点。

那笔者作为一个个人开发者能整点什么呢？

正如之前寻思的，**接入 IM** 是 OpenClaw 的一大亮点，笔者很熟悉 QQ Bot 的开发，也确实感觉有时候在 QQ 里能用用 Agent 会很方便，于是就产生了开发一个自己的 Claw 类似物的想法。这样的 Claw 完全是依照我的想法做出来的，用起来应该也会更安心？

Harness 方面，笔者一直有一个观点：如果想要一个 Agent 产品好用，那么它的基座 LLM 必然需要在这个 Harness 的工具上做专门的训练，以达到最好的效果。所以，为了让这个 Claw 有更好的“任务交付能力”，我决定**直接用 Codex 现成的 Harness**，而不是像 OpenClaw 一样自己构造一套工具和外周服务。至于为什么不选 Claude Code，一方面是因为 Claude Code 闭源，一方面是因为不知道按某 A\ 的脾气会不会来封我的号。而 Codex 在 GitHub 上的维护状况看上去也比很多纯 vibe 的 xxClaw 类似物要好得多。以下引用 [Codex 贡献指南](https://github.com/openai/codex/blob/main/docs/contributing.md)中的部分内容：

> Contributing
>
> **External contributions are by invitation only**
>
> At this time, the Codex team does not accept unsolicited code contributions.
>
> If you would like to propose a new feature or a change in behavior, please open an issue describing the proposal or upvote an existing enhancement request. We prioritize new features based on community feedback, alignment with our roadmap, and consistency across all Codex surfaces (CLI, IDE extensions, web, etc.).
>
> If you encounter a bug, please open a bug report or verify that an existing report already covers the issue. If you would like to help, we encourage you to contribute by sharing analysis, reproduction details, root-cause hypotheses, or a high-level outline of a potential fix directly in the issue thread.
>
> The Codex team may invite an external contributor to submit a pull request when:
>
> - the problem is well understood,
> - the proposed approach aligns with the team’s intended solution, and
> - the issue is deemed high-impact and high-priority.
>
> Pull requests that have not been explicitly invited by a member of the Codex team will be closed without review.
>
> **Why we do not generally accept external code contributions**
>
> In the past, the Codex team accepted external pull requests for bug fixes. While we appreciated the effort and engagement from the community, this model did not scale well.
>
> Many contributions were made without full visibility into the architectural context, system-level constraints, or near-term roadmap considerations that guide Codex development. Others focused on issues that were low priority or affected a very small subset of users. Reviewing and iterating on these PRs often took more time than implementing the fix directly, and diverted attention from higher-priority work.
>
> The most valuable contributions consistently came from community members who demonstrated deep understanding of a problem domain. That expertise is most helpful when shared early -- through detailed bug reports, analysis, and design discussion in issues. Identifying the right solution is typically the hard part; implementing it is comparatively straightforward with the help of Codex itself.
>
> For these reasons, we focus external contributions on discussion, analysis, and feedback, and reserve code changes for cases where a targeted invitation makes sense.

他们没有放任某个菊花头像的 Code Agent 成为项目的主要贡献者，完全胜利 ; )

## 调研

造轮子之前，肯定要先看看有没有人造过类似的。所以先让 Codex 启动几个并行子代理，找找有没有把 Codex 接入 IM 软件的项目。诶！还真有，而且还不少。这个 [Ductor](https://github.com/PleasePrompto/ductor) 是笔者比较喜欢的，笔者也确实试着把 Codex 接到 Telegram 里了，效果还不错，就是 Telegram 比起 QQ 微信还是太不常用了。但是它的很多交互逻辑笔者很喜欢，后续的开发里也参考了它的交互设计。

QQ 非官方机器人三天两头容易被封，用着多少有点难受，感谢 OpenClaw 开源打开了 QQ 官 Bot 的大门，现在个人在沙盒模式下几乎可以无限制地在私聊范围内使用官方 Bot 了。早年 QQ 官方 Bot 的文档也是一坨大的，但依然感谢 OpenClaw 开源，这个 [openclaw-qqbot](https://github.com/tencent-connect/openclaw-qqbot) 就是可以直接抄的通讯协议范本，再也不用~~自己~~让 Agent 去赤那一坨官方文档辣！

至于技术栈，考虑到现在的内存十分滴珍贵，笔者也不需要什么高端的 React 前端页面，只需要尽可能好的性能，所以就直接用 Codex 最擅长写的 Rust 好啦。至此，我们就可以开始设计开发计划，然后交给 Codex 去实现了。

## 开发实现

后续和 QQ 对接、设计用户交互路径之类杂七杂八的事情费了不少心思，而且后来参考 Hermes 试着做了一些记忆、自进化 Skill、计划任务的系统，这些开发细节等以后~~想起来~~有空再聊吧。

## 未来

接下来笔者想聊一聊 Vibe Coding 和开源项目。

前面提到，笔者不喜欢 OpenClaw 的一大原因是：

> 它实际工作的代码/逻辑已经没有人类能搞得懂了。

这大概是所有主要依靠 Vibe Coding 维护，并随意接受 Vibe Coding 贡献的开源项目的必定结局——史山越堆越高，最终无可救药。出了一个 bug 没有人会知道是哪些代码导致的，只能让 Claude 去打一个补丁，但是没人会知道这个补丁在未来会不会引发一个新的 bug。

但是必须承认 Vibe Coding 能让开发者，甚至是普通人以百倍于传统软件工程的速度，交付“看上去可用”的产品。用户只会看到产品本身，只会希望尽快用上产品，而不会去在意产品背后的开发过程。但为了让产品能持续地满足用户的需求，其持续的迭代也是必须的。这个迭代过程在目前，也就是 2026 年 5 月 AI 能力的背景下，**必须要有人来监督和调整**，否则必然在产品达到一定规模时彻底丧失可维护性。

一种有益的实践是，让产品尽可能“小”，代码库的代码行数尽可能地少，在规模足够小的情况下 AI 其实有能力去持续维护这个项目。那么为了阻止产品变“大”，就必须尽可能保证，产品本身满足的正好是**一个**用户的需求，而不是臃肿地包含一大堆特性，用户只用得上其中的少量特性。例如，OpenClaw 支持大量的 LLM 提供商和 IM 平台，但事实上一个用户可能只用得上其中的一两种。那么其他提供商、IM 平台的支持在此处就是累赘的。

所以，笔者粗浅地认为，未来像 OpenClaw 这样高度个性化的、非企业的、非大规模使用的开源软件可能会有更明显的**去中心化**趋势，在中心位置的 OpenClaw 本身仅**可靠地**维护基本的功能，例如它的 Harness，和几个基本的适配器，而其他具有个性化需求的用户可以 fork 该仓库，然后使用 Vibe Coding 来添加**只有自己需要**的特性。如果 AI 的能力足够强，甚至可以通过一段 Prompt 来“开源”这个特性的代码——上游仓库，或者其他 fork 仓库，只需要把这段 Prompt 扔给 Codex 就能在自己的分支上得到相同的特性。

理想情况下，CodexClaw 可能会以这种方式维护吧，至少笔者是绝对不会随便把各种 AI 贡献接受到项目里的 : )

~~但是就目前来看 CodexClaw 已经在奔向史山的路上一去不复返了。~~
