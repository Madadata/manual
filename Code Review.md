# Code review 规则

## 为什么要有 code review

Code review 是提高团队团队代码质量的有效方法。因为个人疏忽，设计缺陷，以及其他的 unknown unknowns（你不知道你其实不知道的东西）的存在，通过同事之间互相 code review 都可以避免。

Code review 不是互相挑刺，也绝对不是互相评价技术水平的地方。大家应该充分认识到，code review 的唯一目的就是共同提高代码和文档的质量。因此，提交 code review 和读 code review 的人对代码质量负有同样责任。

Code review 还是一个项目管理的工具。我们鼓励大家勤发 code review，把每个具体模块拆成小功能，相互独立，然后分为多个 code review 分次合并到 master 上，既减轻了看 code review 的人的工作量（没有人喜欢看100个文件的 code review），又可以让你的功能更模块化（否则单个 code review 过不了 CI server 的测试），又可以帮助作者时刻注意自己的项目进展，还可以让看过的 review 的人也同步了解你的进展，一举多得。


## 我们的 code review 规则

我们约定：

1. 发 code review 要「精简」和「勤」，也就是我们不鼓励超过三天的工作量一次性提交，原因见上
2. 发 code review 必须要指定 reviewer，不可以自己放了之后，没有人看过就直接 merge（production hot fix 除外，但此时必须周知所有人）
3. 指定 reviewer 的时候应该包括比较了解这个项目的人，但我们鼓励所有人都参与
4. 指定的 reviewer 有责任看完 code review，并且给出评论，提交者必须把评论解决之后，等待留言通过（比如 LGTM），才可以 merge——须知 reviewer 和 code review 作者对代码质量负有同样责任
5. 为了更高效的协作，code review 的作者有责任节约 reviewer 的时间，比如在发起的 code review 中不可以有忘记删除的代码、评论，不可以没有通过 linter 的测试，不可以有一些明显不应该出现的语句（比如 `System.out` ），不应该在 CI server 测试没有通过的时候就邀请大家——经常犯这条的人需要请对方吃冰激凌
6. 对于 code review 里面的评论，作者必须一一回应、修改；如果因为某些原因需要推迟到随后的 pull request 再修改的，必须在 Github 中创建一个 issue，描述清楚内容，把对应评论的链接附在 issue 中，然后把 issue 的链接也贴到评论的回复里面——这样做的目的是为了防止作者之后忘掉修改（it happens to everyone）

