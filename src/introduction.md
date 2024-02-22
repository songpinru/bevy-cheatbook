{{#include ./include/links-common.md}}


# 关于译者

国内目前没有比较好的Bevy教程，作为学习者，我只能从英文文档学习，由开发者编写的 Unofficial Bevy Cheat Book 是目前能找到的介绍 Bevy 最全面的书籍。所以打算一边学一边翻译，这是我第一次做这种翻译工作，我会尽力翻译，如果有不足的地方，可以联系我或者帮助我，有时间我一定会更新，谢谢大家。

原书使用的是Mdbook和github pages，我依循了这个方式，这两个工具非常方便，可以让我集中精力在翻译上。有想要联系我或者帮我的，可以通过github联系我，[仓库地址](https://github.com/songpinru/bevy-cheatbook).

-------------------------
# 非官方的教程

这是一本针对 [Bevy 游戏引擎][bevy::website]
([GitHub][project::bevy])的参考书.

它旨在以简洁的方式教授 Bevy 概念，帮助您提高工作效率， 并发现您需要的知识。

这本书汇集了许多官方文档没有涉及的社区经验，你不需要浪费时间解决问题，其他人已经搞定了！

虽然它的目标是详尽无遗，但这是一项艰巨的任务。我把时间集中在我认为社区最需要的事情上。

因此，无论是基础还是高级知识点都还有遗漏的部分，但是我相信这本书对你还是很有价值的！


***欢迎！愿这本书对你有好处！***

(别忘了给这本书[GitHub repository][project::cb]
<a class="github-button" href="https://github.com/bevy-cheatbook/bevy-cheatbook" data-icon="octicon-star" aria-label="Star bevy-cheatbook/bevy-cheatbook on GitHub">Star</a>,
也可以[捐赠](https://github.com/sponsors/inodentry) 🙂)

## 如何使用

这本书不必按顺序阅读，每个章节都是一个独立的知识点，请随意跳转。

使用侧边栏跳转到感兴趣的章节或者使用顶部的搜索功能跳转。

[章节概述][chapter::overview] 帮助你大致了解这本书的结构

每个章节都有相关章节的链接，可以帮助你方便的跳转

如果你是新手，从[新手引导][cbtut::guide]开始. 这会帮助你有顺序的了解初级到高级的知识。

[Bevy 内置资源][chapter::builtins] Bevy提供的内置类型与特性。

## 推荐的其他资源

 [official code examples][bevy::examples]，官方示例。

 [bevy-assets][bevyassets], 社区生态资源。

 [Bevy Discord][bevy::discord] 官方聊天室。

 [itch.io][itchio::bevy]或[Bevy Assets][bevyassets::games]上可以找到Bevy制作的一些游戏.

## 这本书是最新的吗?

Bevy 的开发速度非常快，几乎每三个月，每个版本都会带来很多变化，所以保持这本书的更新是一个重大挑战。

为了减轻维护负担，该书包含不同版本的 Bevy 的内容。但是，多个Bevy版本的内容不允许在同一页面上。

在每个页面的顶部，您都会看到它上次更新的版本。 该页面上的所有内容必须与所述的 Bevy 版本相关。

## Support Me

<a href="https://github.com/sponsors/inodentry"><button class="ghsponsors-button">GitHub Sponsors</button></a>
<a href="https://patreon.com/iyesgames"><button class="patreon-button">Patreon</button></a>
<a href="bitcoin:bc1qaf32uqsg6mngw9g4aqc3l2jvuv46qx0zw2438p"><button class="bitcoin-button">Bitcoin</button></a>

If you like this book, please consider sponsoring me. Thank you! ❤️

I'd like to keep improving and maintaining this book, to provide a high-quality
independent learning resource for the Bevy community.

## Support Bevy

<a href="https://github.com/sponsors/cart"><button class="ghsponsors-button">GitHub Sponsors</button></a>

If you like the Bevy Game Engine, you should consider donating to the project.

## License

Copyright © 2021-2023 Ida (IyesGames)

All code in the book is provided under the
[MIT-0 License](https://github.com/bevy-cheatbook/mit-0).
At your option, you may also use it under the regular MIT License.

The text of the book is provided under the
[CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/).

Exception: If used for the purpose of contribution to the "Official Bevy
Project", the entire content of the book may be used under the [MIT-0
License](https://github.com/bevy-cheatbook/mit-0).

"Official Bevy Project" is defined as:
 - Contents of the Git repository hosted at [https://github.com/bevyengine/bevy](https://github.com/bevyengine/bevy)
 - Contents of the Git repository hosted at [https://github.com/bevyengine/bevy-website](https://github.com/bevyengine/bevy-website)
 - Anything publicly visible on the [bevyengine.org](https://bevyengine.org) website

The MIT-0 license applies as soon as your contribution has been accepted upstream.

GitHub Forks and Pull Requests created for the purposes of contributing to
the Official Bevy Project are given the following license exception: the
Attribution requirements of CC BY-NC-SA 4.0 are waived for as long as the
work is pending upstream review (Pull Request Open). If upstream rejects
your contribution, you are given a period of 1 month to comply with the
full terms of the CC BY-NC-SA 4.0 license or delete your work. If upstream
accepts your contribution, the MIT-0 license applies.

## Contributions

Development of this book is hosted on [GitHub][project::cb].

Please file GitHub Issues for any wrong/confusing/misleading information,
as well as suggestions for new content you'd like to be added to the book.

Contributions are accepted, with some limitations.

See the [Contributing][cb::contributing] section for all the details.

## 稳定性警告

Bevy 仍然是一个新的实验性游戏引擎！它公开自 2020 年 8 月！

虽然很活跃，发展速度很快，但Bevy还未成熟。

没有稳定性保证，经常发生重大更改！

通常，适应新版本的变化并不难，但是依然请你了解这一点！