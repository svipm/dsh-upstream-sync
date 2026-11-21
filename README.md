# dsh-upstream-sync

定时将公开上游 [yexi-by/dsh-unrestricted](https://github.com/yexi-by/dsh-unrestricted) 的 `master` 分支镜像同步到私有仓库 [svipm/dsh-unrestricted](https://github.com/svipm/dsh-unrestricted)（github.com 上私库不可见是正常的）。

## 为什么需要这个仓库

GitHub Actions 在私有仓库上受账号 Actions 额度或账单限制时无法运行，而公开仓库的 Actions 是免费的。因此本仓库只存放工作流，通过 Deploy Key（保存在 Actions Secret `DEPLOY_KEY` 中）向私有仓库执行 git push。私有代码本身不会出现在本仓库。

## 运行方式

- 自动：每 6 小时一次（UTC 00:17 / 06:17 / 12:17 / 18:17）
- 手动：Actions → Sync dsh-unrestricted from upstream → Run workflow

## 防删库设计

- **上游身份锁**（`upstream-lock.json`）：同步前先用 GitHub API 校验上游的数字 repo id（不可变）。上游若被删除后有人重新注册同名仓库，id 必然不同，同步会中止并报警，而不是把冒名内容覆盖进镜像。
- **拉取失败即停止**：上游 fetch 失败（删除/改名/网络问题）时工作流直接失败并发送 Actions 失败通知，私有镜像保持最后一次成功同步的状态不动。
- **keepalive**：每月 1 日自动提交一次时间戳，防止公开仓库的定时任务因 60 天无活动被 GitHub 自动禁用。

## 注意

工作流会把私有仓库的 `master` 强制对齐到上游（含 tags），私有仓库里的任何本地改动都会被覆盖。需要保留本地改动时，先在私有仓库开分支。
