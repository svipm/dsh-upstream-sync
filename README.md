# dsh-upstream-sync

定时将公开上游 [yexi-by/dsh-unrestricted](https://github.com/yexi-by/dsh-unrestricted) 的 `master` 分支镜像同步到私有仓库 [svipm/dsh-unrestricted](https://github.com/svipm/dsh-unrestricted)。

## 为什么需要这个仓库

GitHub Actions 在私有仓库上受账号 Actions 额度或账单限制时无法运行，而公开仓库的 Actions 是免费的。因此本仓库只存放一个工作流，通过 Deploy Key（保存在 Actions Secret `DEPLOY_KEY` 中）向私有仓库执行 git push。私有代码本身不会出现在本仓库。

## 运行方式

- 自动：每 6 小时一次（UTC 00:17 / 06:17 / 12:17 / 18:17）
- 手动：Actions → Sync dsh-unrestricted from upstream → Run workflow

## 注意

工作流会把私有仓库的 `master` 强制对齐到上游，私有仓库里的任何本地改动都会被覆盖。
