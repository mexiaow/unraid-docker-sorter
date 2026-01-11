# unraid-docker-sorter

为 Unraid 的 Docker 页面提供一个“紧凑网格拖拽排序器”页面：在小卡片网格里拖拽排序，保存后复用官方 `UserPrefs.php` 写回 Docker 页顺序。

## 安装（URL）

在 Unraid WebGUI：`Plugins -> Install Plugin`，粘贴：

`https://raw.githubusercontent.com/mexiaow/unraid-docker-sorter/main/docker.sorter.plg?version=0.1.17`

## 安装（手动）

1. 将 `docker.sorter.plg` 复制到 Unraid 的 U 盘路径：`/boot/config/plugins/docker.sorter.plg`
2. 在 Unraid 终端执行：`installplg /boot/config/plugins/docker.sorter.plg`
3. 打开 WebGUI：`Tools -> Docker Sorter`

## 使用与验证

1. 在 `Tools -> Docker Sorter` 拖拽任意两个容器改变顺序
2. 点击 `保存到 Docker 页`
3. 刷新 `Docker` 页面，顺序应与网格页一致

## 故障排查

- 拖拽不可用：本页依赖 `cdn.jsdelivr.net` 加载 `SortableJS`，请确认 Unraid 能访问外网或更换可用的 CDN
- 提示缺少 `csrf_token`：Unraid webGUI 对写操作会强制 CSRF；请确认已登录，然后强制刷新（Ctrl+F5），仍不行就重开浏览器或清理该站点缓存
- 仍提示缺少 `csrf_token`：在排序器页面控制台执行 `window.csrf_token` 看是否有值；也可打开 `.../plugins/docker.sorter/include/api.php?action=csrf` 看返回的 `csrf_token`
- 备份失败但仍可保存：备份为“尽力而为”，不影响顺序保存；请把报错信息里的响应内容贴出来以便定位
- 图标不显示：优先使用模板 `Icon`；没有设置图标则直接使用 DockerMan 默认 `question.png`（`/plugins/dynamix.docker.manager/images/question.png`），避免大量 404 刷屏
- 保存显示成功但 Docker 页不变：打开排序器页面下方的调试信息，确认 `user-prefs` 路径与 `mtime` 是否变化；也可访问 `.../plugins/docker.sorter/include/api.php?action=prefsInfo` 查看写入后的实际内容

## 卸载

- 在 Unraid WebGUI：`Plugins -> Installed Plugins -> docker.sorter -> Remove`
- 或删除：`/boot/config/plugins/docker.sorter.plg`，然后重启（插件将不再自动安装）

## 说明

- 保存顺序由插件后端直接写入 DockerMan 的 `user-prefs` 文件（与 Docker 页拖拽保存写入同一位置），并同步调整 Docker autostart 顺序
- 拖拽使用 `SortableJS`（从 `cdn.jsdelivr.net` 加载）
- 网格列数默认 6（可调 5–7）；顺序按“行优先：从左到右、从上到下”
- 卡片显示：图标 + 名称 + 运行状态（绿=运行，红=停止）

在此补充项目说明。
