# ERPNext 安装复盘

## 安装要求

本次 ERPNext v16 在 Ubuntu 24.04 上的关键依赖是：

- Python 3.14。
- Node 24。
- Yarn 1.22.x。
- pkg-config。
- MariaDB。
- Redis。
- Git。[web:701][web:741][web:747]

## 本次做对的事

- 使用独立的 `frappe` 用户安装，而不是长期用 root 操作。
- 及时通过 `bench init` 暴露依赖缺失问题。
- 最终确认了可用的版本组合：Python 3.14.6、Node 24、Yarn 1.22.22、MariaDB 10.11.14、Redis 7.0.15。
- 通过 `sudo mysql` 验证 MariaDB root 可访问。
- 成功初始化了 `frappe-bench`。[web:702][web:882][web:893][web:917]

## 本次踩坑的事

- 只检查了 Node/npm，没有一次性检查完整依赖。
- 忽略了 Frappe v16 对 Python 3.14 的要求。
- 没有提前确认 Yarn 版本和路径，导致 `yarn install --check-files` 失败。
- Node 版本停留在 20.x，未达到 v16 需要的 24。
- `bench new-site` 时把 `Yang` 误当成数据库超级用户，导致 `Access denied`。
- 对 MariaDB root 的认证方式不清晰，后面才确认 root 可用并切换到 `mysql_native_password`。[web:741][web:827][web:891][web:893][web:945]

## 下次优化建议

1. 先统一做版本检查：
   - `python3.14 --version`
   - `node --version`
   - `npm --version`
   - `yarn --version`
   - `pkg-config --version`
   - `mariadb --version`
   - `redis-server --version`
   - `git --version`。[web:701][web:741]

2. 使用 nvm 管理 Node，并切到 Node 24。[web:843][web:844]

3. 使用 Yarn classic 1.22.22，避免旧的 `cmdtest`/`yarn` 冲突。[web:810][web:818]

4. 安装并显式使用 Python 3.14，而不是系统默认 3.12。[web:741][web:828]

5. `bench new-site` 时，数据库超级用户应填真实可登录的 MariaDB root 或专用管理员账号，不要猜用户名。[web:891][web:893]

## 最终结果

- Bench 初始化成功。
- MariaDB root 本机登录正常。
- 版本链路已经对齐，可继续创建站点并安装 ERPNext。[web:882][web:917]
