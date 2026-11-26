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
- 机器无 pdflatex、无 root apt；编译验证用免 root 的 **tectonic**（在 `.openclaw/tmp/paper_edit/tectonic`，首次编译需联网拉 bundle，较慢）
- GitHub HTTPS 大文件下载易截断（curl 会停在几 MB），**wget -c 续传更可靠**
- 论文验证公式编号用 `pdftotext`（含 -layout / 分页 dump；两栏版面提取顺序会乱，要以源码顺序为准）

## 待办 / 开放问题
- 2026-11-26：用户要求"记忆也要上传"，但 mm2_paper 是公开仓（记忆含个人信息不宜放进去），且没找到现成的记忆仓库——等用户指定/创建私人仓库后推送
- Overleaf 那个邀请链接（6b0694c1…）需登录才能访问，若还要同步 Overleaf 内容需要只读链接
- 记忆已获用户确认放入公开仓 mm2_paper（MEMORY.md + memory/）；**每次更新记忆文件后要同步复制到 mm2_paper 并推送**
