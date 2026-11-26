# MEMORY.md - Long-Term Memory

## 关于用户
- gush596（GitHub），邮箱 gush14@foxmail.com，时区 Asia/Shanghai
- 在做学术论文（深度学习 / SNN 方向），中文交流，工作节奏快，指令简洁
- 偏好：不确定就问，别猜他的意图；改完要及时提交推送 git

## 当前项目：PRIME 论文
- 仓库：`github.com/gush596/mm2_paper`（**公开仓**），本地 `mm2_paper/`
- 内容：ANN-to-SNN 论文（多阈值神经元 + RMPA + FP16 二进制解析），ICLR 2027 版（iclr_*）+ ICML 2026 版（icml_*）
- **ICML 版是"初步修改版"**，所有改动相对初始版本必须用颜色标注：`\added` 蓝 / `\moved` 橙 / `\deleted` 红删除线 / `\movednote` / `\deletednote` / `addedblock` / `movedblock` / `movedparagraph` / `addedparagraph`（定义在 icml_main.tex 42-50 行）
- 改写英语时尽量复用论文现有句式和用词

### 论文结构要点（2026-11-26 状态）
- Eq.(6)：$V=2m_{rest}=m_{rest}+m_{rest}$，后附说明"硬件形态决定自加还是乘 2"
- 3.2 有 D>1 段：先讲"为适配主流二进制硬件高效实现额外引入 D"→ D 定义 → D>1 也可用脉冲发放实现（Eq.(10) $V^\ell=2^D m^\ell_{rest}$、Eq.(11) $S=\max\{...\}_2$ 下标 2 表转二进制）→ T 间放大 $2^n$ 直到 $V>\theta$ 过渡 → D>1 适配二进制硬件、也适用未来神经拟态芯片
- 3.3 开头：命题在前（Proposition 1 误差界），"The detailed derivation is given below."，推导（Rate Coding / Integration Coding）在下；末尾补充"D=1 时误差界仍是 $\theta/2^{T+1}$"
- math_commands.tex 的 `\eqref` 已改为标准 "(5)" 样式（原来是 equation~\ref{} 会渲染成 "Eq. equation 5"）

## 环境与工具备忘
- GitHub 操作走 SSH key `ssh/github_ed25519`（GIT_SSH_COMMAND 方式）；**不输出私钥内容、不操作 ~/.ssh**
- 机器无 pdflatex、无 root apt；编译验证用免 root 的 **tectonic**（在 `tools/tectonic`，首次编译需联网拉 bundle，较慢）
- GitHub HTTPS 大文件下载易截断（curl 会停在几 MB），**wget -c 续传更可靠**
- 论文验证公式编号用 `pdftotext`（含 -layout / 分页 dump；两栏版面提取顺序会乱，要以源码顺序为准）

## 下次会话快速恢复手册（拿到记忆文件后照此执行）
1. **论文仓库**：`mm2_paper/` 本地已克隆；若没了重新拉：`GIT_SSH_COMMAND="ssh -i ssh/github_ed25519 -o IdentitiesOnly=yes" git clone git@github.com:gush596/mm2_paper.git`
2. **Git SSH key（用户定的规则：每次会话都重新生成，不做持久备份）**：开始干活前先生成新密钥对：`ssh-keygen -t ed25519 -C gush14@foxmail.com -f ssh/github_ed25519 -N ""`（如已有旧密钥，先把 ssh/ 下旧文件移到回收站再生成），然后把**公钥**（.pub）发给用户让他添加到 GitHub Settings → SSH keys（旧公钥可顺手删除）。之后所有 git 命令加 `GIT_SSH_COMMAND="ssh -i $PWD/ssh/github_ed25519 -o IdentitiesOnly=yes"`。
   - **红线：私钥内容永远不在聊天/仓库/任何外发渠道出现**，只给公钥。
3. **LaTeX 环境**：`tools/tectonic`（免 root，linux x86_64，仓库里有备份）。编译：`cd mm2_paper && ../tools/tectonic -X compile icml_main.tex`（编译产物直接覆盖 icml_main.pdf）。bundle 缓存在 `~/.cache/tectonic`，在则编译快；丢了会自动重新联网下载（慢）。
   - 若 tectonic 二进制丢失：从仓库 `tools/tectonic` 复制，或 `wget -c` 从 GitHub releases 拉 `tectonic-0.17.0-x86_64-unknown-linux-musl.tar.gz`（curl 大文件易截断，用 wget）。
4. **论文修改规范**：改 icml_* 文件；所有改动用颜色宏标注（见上）；英语复用原句式；改完 commit + push；拿不准的位置直接问用户。

## 待办 / 开放问题
- 2026-11-26：用户要求"记忆也要上传"，但 mm2_paper 是公开仓（记忆含个人信息不宜放进去），且没找到现成的记忆仓库——等用户指定/创建私人仓库后推送
- Overleaf 那个邀请链接（6b0694c1…）需登录才能访问，若还要同步 Overleaf 内容需要只读链接
- 记忆已获用户确认放入公开仓 mm2_paper（MEMORY.md + memory/）；**每次更新记忆文件后要同步复制到 mm2_paper 并推送**
