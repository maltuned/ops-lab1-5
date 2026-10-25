# 实验 1 记录：第 5 组

现场检查时将打开本文件，按其中的链接核对。证据一律以链接或文本形式给出，不要只贴截图。将 `<填写>` 替换为本组的内容。main 受保护之后，修改本文件也须通过 PR。

## 1. 基本信息

| 项                | 内容                                   |
| ----------------- | -------------------------------------- |
| 小组              | 第 5 组                                |
| 仓库地址          | https://github.com/maltuned/ops-lab1-5 |
| 代码载体          | 统一示例                               |
| runner 与语言版本 | `ubuntu-24.04`，go 1.26                |
| 合并方式          | squash                                 |

## 2. 任务 0：本地基线

| 问题                                                                                                 | 观察结果                                                                                                                                                                                                                                                  |
| ---------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 第一次 `make package` 得到的 BUILD_INFO（粘贴）                                                      | name=enroll-conflict<br>version=1.0.0<br>build_no=1<br>run_id=local<br>commit=7b63b41c13f061439997b0e6e4eaa2af922a7aa1<br>go=go1.27.2                                                                                                                     |
| 修改一行、不提交，再次打包：BUILD_INFO 是否变化？二进制文件的 sha256 是否变化？（贴出两次的 sha256） | 没有变化<br>7d7e3287d1b32d90e67219040c1a3e1f22a769cb33bc734b1fb7792672db7c8d<br>e603b4acfda53e34aaf64ab8cc258f9f43e86aff953c817380e819e79570247a                                                                                                          |
| 两次 `go version -m` 输出中的 vcs 行有何不同？                                                       | vcs 类型、revision 和 time 都相同，只有 modified 不同，修改前是 `false`，修改后是 `true`                                                                                                                                                                  |
| 仅凭 BUILD_INFO，能否回答该包是什么、在哪台机器上、何时、用哪份代码构建？哪一项信息会出错？          | 只能确定包名和版本号，不能确定包是什么<br>`github.run_id` 只能标识一次工作流，不能确定是哪台机器<br>不能确定构建时间， `BUILD_INFO` 没有记录相关信息<br>不能确定是用哪份代码构建，如果 Git 仓库有未提交的修改，就算 commit 相同，用来构建的代码也可能不同 |

## 3. 证据

| 项                                             | 链接或文本                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ---------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 规则集里启用的规则                             | 规则名：Restrict deletions、 Require a pull request before merging、Require status checks to pass、Block force pushes<br>审批人数：1<br>必须的检查：ci                                                                                                                                                                                                                                                                                                                                                                                                |
| 组长直接 `git push origin main` 被拒的终端输出 | remote: error: GH013: Repository rule violations found for refs/heads/main.<br>remote: Review all repository rules at https://github.com/maltuned/ops-lab1-5/rules?ref=refs%2Fheads%2Fmain<br>remote:<br>remote: - Changes must be made through a pull request.<br>remote:<br>remote: - Required status check "ci" is expected.<br>remote:<br>To github.com:maltuned/ops-lab1-5.git<br> ! [remote rejected] main -> main (push declined due to repository rule violations)<br>error: failed to push some refs to 'github.com:maltuned/ops-lab1-5.git' |
| 第一轮：失败的 PR 运行                         | https://github.com/maltuned/ops-lab1-5/actions/runs/39603298415                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| 第一轮：修复后通过的 PR 运行                   | https://github.com/maltuned/ops-lab1-5/actions/runs/39604046874                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| 第一轮：合入 main 后的运行（含制品）           | https://github.com/maltuned/ops-lab1-5/actions/runs/39604694598                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| 第二轮：找到的缺陷，以及现有测试为何无法发现它 | 在 `Conflict` 中特判所有只有一节次的课程全都互不冲突，测试目前只涉及多节次课程的测试                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| 第二轮：仅含缺陷时仍然通过的运行               | https://github.com/maltuned/ops-lab1-5/actions/runs/39608694172                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| 第二轮：补测试后失败的运行                     | https://github.com/maltuned/ops-lab1-5/actions/runs/39609381983                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| 第二轮：撤销缺陷后通过的运行                   | https://github.com/maltuned/ops-lab1-5/actions/runs/39610030268                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| 未合入分支上的 tag 被 release gate 拒绝的运行  | https://github.com/maltuned/ops-lab1-5/actions/runs/39617429494                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| Release v1.0.0                                 | https://github.com/maltuned/ops-lab1-5/releases/tag/v1.0.0                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| Release v1.0.1                                 | https://github.com/maltuned/ops-lab1-5/releases/tag/v1.0.1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |

## 4. 追溯与运行

对 v1.0.0 和 v1.0.1 各运行一次 `scripts/trace.sh`，粘贴输出，并填写下表。

| 项                                                 | v1.0.0                                                           | v1.0.1                                                           |
| -------------------------------------------------- | ---------------------------------------------------------------- | ---------------------------------------------------------------- |
| 版本号                                             | 1.0.0                                                            | 1.0.1                                                            |
| 构建号                                             | 1                                                                | 3                                                                |
| 提交号（SHA）                                      | 42fd737d994272f65e93b89af2fbe8d1fa2031df                         | 6e114885ce57c786ce7fd89b1e6561fd182a5d08                         |
| 引入该提交的 PR                                    | #7 https://github.com/maltuned/ops-lab1-5/pull/7                 | https://github.com/maltuned/ops-lab1-5/pull/11                   |
| 构建它的 Actions 运行（`trace.sh` 给出的链接）     | https://github.com/maltuned/ops-lab1-5/actions/runs/39615211859  | https://github.com/maltuned/ops-lab1-5/actions/runs/39745379494  |
| 制品 sha256（发布当时，取自 Release 里的 .sha256） | 9b2698a08ded233b238c54a6bd7009b3d074d222d48939e0e5edd7aec0009d9e | 66764a8add7cd42d90b7009e12570ca3bb4b27a9bdcf898ea40db056e04ceca1 |

仅凭构建号能否唯一确定是哪次运行？v1.0.0 的构建号在 `ci` 和 `release` 的运行列表中各对应哪次运行？
不能，构建号是针对一个 workflow 的运行次数，不能确定是哪个 workflow。v1.0.0 的构建号为 1，不对应 ci 的 [构建号 #1 运行](https://github.com/maltuned/ops-lab1-5/actions/runs/39382612527)，而对应 release 的 [构建号 #1 运行](https://github.com/maltuned/ops-lab1-5/actions/runs/39615211859)。

发布 v1.0.1 之后重新下载 v1.0.0 的附件，计算得到的 sha256：9b2698a08ded233b238c54a6bd7009b3d074d222d48939e0e5edd7aec0009d9e。与上表中发布当时的值是否一致？一致

`trace.sh` 输出（v1.0.0）：

```text
== 1. 校验和
enroll-conflict_1.0.0_linux_amd64.tar.gz: OK
== 2. 制品里的 BUILD_INFO
name=enroll-conflict
version=1.0.0
build_no=1
run_id=39615211859
commit=42fd737d994272f65e93b89af2fbe8d1fa2031df
go=go1.26.9
== 3. 提交 42fd737d994272f65e93b89af2fbe8d1fa2031df
作者: Yuanzhe Cai <kiyyaforever007@gmail.com>
时间: 2026-10-21T22:11:28+08:00
标题: Finish Release Workflow (Task 5) (#7)
主干: 该提交在 origin/main 的历史上
指向它的 tag: v1.0.0
版本号: 1.0.0    构建号: 1
提交页面（含关联的 PR）: https://github.com/maltuned/ops-lab1-5/commit/42fd737d994272f65e93b89af2fbe8d1fa2031df
构建它的运行: https://github.com/maltuned/ops-lab1-5/actions/runs/39615211859
```

`trace.sh` 输出（v1.0.1）：

```text
== 1. 校验和
enroll-conflict_1.0.1_linux_amd64.tar.gz: OK
== 2. 制品里的 BUILD_INFO
name=enroll-conflict
version=1.0.1
build_no=3
run_id=39745379494
commit=6e114885ce57c786ce7fd89b1e6561fd182a5d08
go=go1.26.9
== 3. 提交 6e114885ce57c786ce7fd89b1e6561fd182a5d08
作者: HongYi Yu <2746576912@qq.com>
时间: 2026-10-22T01:28:26+08:00
标题: Task 6: test version endpoint build metadata (#11)
主干: 该提交在 origin/main 的历史上
指向它的 tag: v1.0.1
版本号: 1.0.1    构建号: 3
提交页面（含关联的 PR）: https://github.com/maltuned/ops-lab1-5/commit/6e114885ce57c786ce7fd89b1e6561fd182a5d08
构建它的运行: https://github.com/maltuned/ops-lab1-5/actions/runs/39745379494
```

任务 6 第 4 步，两次 `/version` 的输出：

```text
{"version":"1.0.1","commit":"6e114885ce57c786ce7fd89b1e6561fd182a5d08","build_no":"3","go":"go1.26.9"}
{"version":"1.0.0","commit":"42fd737d994272f65e93b89af2fbe8d1fa2031df","build_no":"1","go":"go1.26.9"}
```

## 5. 数据

时间均从 GitHub 页面读取，并注明时区。

第一轮 gate 演练的时间线：

| 事件                         | 时间                      | 链接                                                                                   |
| ---------------------------- | ------------------------- | -------------------------------------------------------------------------------------- |
| 发起 PR                      | Oct 21, 2026, 20:35 GMT+8 | https://github.com/maltuned/ops-lab1-5/pull/5                                          |
| 第一次 `ci` 失败（运行结束） | Oct 21, 2026, 20:54 GMT+8 | https://github.com/maltuned/ops-lab1-5/actions/runs/39603298415/job/119232989674       |
| 推送修复提交                 | Oct 21, 2026, 20:59 GMT+8 | 48f02a742432b365f3acdb31a81559252f1eac1e                                               |
| `ci` 通过（运行结束）        | Oct 21, 2026, 20:59 GMT+8 | https://github.com/maltuned/ops-lab1-5/actions/runs/39604046874/job/119235573810       |
| 合入 main                    | Oct 21, 2026, 21:03 GMT+8 | https://github.com/maltuned/ops-lab1-5/commit/75279083c7a6ed7c3b3e32120e4ddd249219972f |

提交到发布的间隔，至少记录三个合入的 PR：

| PR  | 第一个提交的时间          | 合入时间                  | 对应 Release 的发布时间   | 提交到发布的间隔 |
| --- | ------------------------- | ------------------------- | ------------------------- | ---------------- |
| #5  | Oct 21, 2026, 20:28 GMT+8 | Oct 21, 2026, 21:03 GMT+8 | Oct 21, 2026, 22:15 GMT+8 | 1:47             |
| #8  | Oct 21, 2026, 21:29 GMT+8 | Oct 21, 2026, 21:59 GMT+8 | Oct 21, 2026, 22:15 GMT+8 | 0:46             |
| #11 | Oct 21, 2026, 23:31 GMT+8 | Oct 22, 2026, 01:28 GMT+8 | Oct 22, 2026, 16:07 GMT+8 | 16:36            |

`ci` 工作流的运行时长（从 Actions 页面读取），至少 5 次，全部列出，不要只给出平均值：

| 运行 | 事件（PR / push） | 时长 | 备注 |
| ---- | ----------------- | ---- | ---- |
| #14  | PR                | 12s  | -    |
| #17  | PR                | 17s  | -    |
| #23  | push              | 14s  | -    |
| #29  | PR                | 13s  | -    |
| #30  | push              | 18s  | -    |

## 6. 分析问题

每题先列出所用数据（可引用上面的表），再用一段话（4～8 句）写出结论，并说明结论的成立条件。

**问题 1：gate 的收益与成本。** 两轮演练中，gate 拦住了什么，没有拦住什么？用第 5 节的时间线计算：第一轮因失败多花了多少时间，其中等待 `ci` 的时间和人工修改的时间各占多少？一次 `ci` 大约需要多久，在“提交到发布的间隔”中占多大比例？第二轮补充测试之后，还有哪类问题是测试本身无法发现的？

第一轮演练中 gate 拦住了不能通过 test 的修改；第二轮演练中， gate 没有拦住 test 没有覆盖的错误。第一轮第一次 `ci` 失败时间是 20:54，`ci` 通过时间是 20:59，因失败多花了 5 分钟；推送修复提交的时间是 20:59，因此等待 `ci` 的时间不到一分钟，人工修改时间是 5 分钟。一次修改时间在 10 秒到 20 秒之间，几乎不占“提交到发布的间隔”。补充测试之后，测试仍然只能发现程序本身行为的问题，构建、发布的问题是代码测试无法发现的。

**问题 2：PR 中的构建与 main 上的构建。** 在 PR 触发的运行日志中，`github.sha` 与 `pull_request.head.sha` 是否相同？它们能否在 main 的历史中找到？答案与合并方式有关，请写明本组使用的方式。合并之后，main 上这次改动的提交号是哪个？据此说明，为什么待发布的制品只能从 main 上的提交或 tag 构建。

以 [PR #5](https://github.com/maltuned/ops-lab1-5/pull/5) 为例，`github.sha` 131d26e1071a8b1e6f7df2624c7ec67648abd139 是 GitHub workflow 自动创建的一个 merge commit 的 sha，`github.event.pull_request.head.sha` 48f02a742432b365f3acdb31a81559252f1eac1e 是 PR 的分支的 sha，两者不同。本组使用 squash merge，两者都不能在 main 的历史中找到。合并之后，main 上的提交是 75279083c7a6ed7c3b3e32120e4ddd249219972f。除了 main 上的提交之外，其他地方的提交都是临时的，随时可能失去引用，所以只能在 main 上创建发布。

**问题 3：可重复构建。** 在本地同一个提交上按下表构建，记录哈希值。④、⑤ 需临时去掉 Makefile 中 tar 的 `--mtime=@0`，完成后用 `git checkout -- Makefile` 恢复，不要提交。

| 构建    | 条件                                                                                | 二进制 sha256                                                                                                                        | tar.gz sha256                                                                                                                        |
| ------- | ----------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| ①       | `make package`，`VERSION`、`BUILD_NO`、`RUN_ID`、`COMMIT` 均指定固定值              | 26d7259467c6d05fd6b2c49968a2f6ce23025baecb773968c339d55dda6993ad                                                                     | 372c3cc8ac95ca753e0b2f94761b9d42d4b0a671c13bce89620743323a55521b                                                                     |
| ②       | 与 ① 完全相同，再构建一次                                                           | 26d7259467c6d05fd6b2c49968a2f6ce23025baecb773968c339d55dda6993ad                                                                     | 372c3cc8ac95ca753e0b2f94761b9d42d4b0a671c13bce89620743323a55521b                                                                     |
| ③       | 仅将 `BUILD_NO` 换成另一个值                                                        | 874272751ced9e0b8e470609ef4297a91db14660b9b1d0067bcab220209bc208                                                                     | 4f023b58e6d496f5b248c3bd64cd7e68831f3f48990d217b0bf6904baac92bb1                                                                     |
| ④       | 去掉 `--mtime=@0`，其余同 ①                                                         | d040d51bd215029b2cf338cab8d6c7e47e4fb265a77a905238c489dd6b64f4b6                                                                     | 4854517c7de4a4e8ba8d0e99cd0fffc53ee99b94ec1e3d91ecbfc8eea8a792bf                                                                     |
| ⑤       | 同 ④，间隔几秒后再构建一次                                                          | d040d51bd215029b2cf338cab8d6c7e47e4fb265a77a905238c489dd6b64f4b6                                                                     | 64f33c637057b0d2502b59d7dc87b71973a0e5d8cda02ea63d907e16eebd08ef                                                                     |
| Actions | 对 main 上的一次 push 运行执行 Re-run all jobs，比较两次尝试中 package 步骤打印的值 | a594f57a07a517a75924d558c4e29726ec208447af118b5faeb85c512173d5aa<br>a594f57a07a517a75924d558c4e29726ec208447af118b5faeb85c512173d5aa | 20896486dc86569c7066cb79f79b8c3b2a518d7be6bedce23dcba04db2ab7924<br>20896486dc86569c7066cb79f79b8c3b2a518d7be6bedce23dcba04db2ab7924 |

哪些相同、哪些不同，原因分别是什么？④ 的二进制文件与 ① 是否相同？如果不同，请结合任务 0 中看到的 vcs 信息分析。据此说明制品中应当包含什么、不应包含什么。另外，`release` 在同一个提交上再次编译，而没有直接发布 `ci` 产出的主干制品：为什么这样做？这与 L-05 第 11 页的“一次构建、多处部署”是否冲突，代价是什么？

1、2 的两个 sha256 都相同，因为构建参数和打包内容都没有改变。1、3 的两个 sha256 都不同，因为 `BUILD_NO` 会同时写入二进制和 `BUILD_INFO`，二进制和打包都会变化。1、4 的两个 sha256 都不同，二进制不同是因为此时 Git 中有未提交的更改，二进制中写入的 vcs 信息与工作区干净时不同；打包不同是因为二进制不同，`tar` 的参数也不同。4、5 的打包二进制不同，因为去掉 `--mtime=@0` 之后，不同构建间文件的 mtime 会变化，导致打包发生变化。Actions 中的 sha256 都相同，因为在同一个提交下重复运行 jobs，相当于这里的 1、2 两种情况，产物没有变化。因此，制品中应当包含与打包环境相关的信息，不应当包含在同一个打包环境下会变化的信息。`release` 产物会写入具体的版本信息，因此不能复用 `ci` 的产物。不冲突，`release` 与 `ci` 间的差异与具体功能无关，仍可以视为几乎没有变化，且方便判断是开发中的版本还是发布版本，代价是发布时需要多一次构建流程。

**问题 4：发布与回退。** 假设 v1.0.1 发布后发现缺陷，应当如何处理？为什么不应删除 v1.0.1 的 tag、再用同一版本号重新发布？任务 6 中将服务从 v1.0.1 切换回 v1.0.0 时，使用的是哪个文件，是否需要重新构建？请用两次 `/version` 的输出说明。如果改为从源码重新构建 v1.0.0，得到的文件与原附件是否相同（参考问题 3）？最后，如果有人删除 v1.0.0 的附件，换上一个新文件和对应的新 `.sha256`，`trace.sh` 能否发现？本组在第 4 节记录的值能否发现？可以查阅 GitHub 的 Immutable releases 设置的用途。

应当即时回退到 v1.0.0，，修复缺陷后发布 v1.0.2。如果修改 v1.0.1 tag 对应的提交，不同环境下显示为同一个版本号的产物实际上有可能是不一样的，会导致不可变失效。使用的是 v1.0.0 `release` 构建的产物，不需要重新构建，两次 `/version` 输出表明了它们构建时的环境信息，可以确认回退后运行的文件和 v1.0.0 发布的相同。根据问题 3 的构建 1 和 2，重新构建得到的文件与原附件相同。如果有人同时替换了 v1.0.0 的两个附件，`trace.sh` 不会执行重复构建来确保产物不变，所以这样做可以欺骗 `trace.sh`。第 4 节记录了 v1.0.0 附件最初的 sha256，因此可以发现被篡改。

## 7. 选做记录

| 选做                    | 完成情况与证据                                                                                                         |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| O1 容器镜像             | ghcr.io/maltuned/ops-lab1-5:v1.0.2 sha256:ad0464028ec3b0c374b19cb95184778347af5105a7991e341623cafcee9d7aa9 linux/amd64 |
| O2 缓存对反馈时间的影响 | 未做                                                                                                                   |
| O3 迁移到小组项目       | 未做                                                                                                                   |

## 8. 成员贡献

每位成员一行，只填写 GitHub 用户名，无需填写姓名；行数应与小组人数一致。

| GitHub 用户名 | 发起并合入的 PR 编号 | 审批过的 PR 编号 |
| ------------- | -------------------- | ---------------- |
| maltuned      | 1 3 4 6 12-18        | 5 7 9-11         |
| Kiyalan       | 7 9 10               | 4 6 8 12 18      |
| Ryosenbei     | 5 8                  | 1 3 13 15-17     |
| Asteroidyyy   | 11                   | 1 3 18           |
