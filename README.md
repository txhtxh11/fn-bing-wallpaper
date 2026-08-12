# fn-bing-wallpaper

飞牛 fnOS 应用：**每日必应 4K 壁纸 · 7天轮换 · 自动设为桌面壁纸**

## 特性

- 每日 00:05 自动下载当日必应 4K 壁纸
- 保存到安装向导指定的**自定义目录**（默认 `/vol1/1000/config/bing-wallpaper`）
- 自动保留最近 **7 张**循环轮换，旧图自动清理
- 自动上传到飞牛壁纸库并**自动设为桌面壁纸**
- **无需任何登录凭证**：直写飞牛壁纸库，永不失效（不再有 token 过期问题）
- 幂等防重复：同一天只入库一次，重启补刷/重复运行不会产生重复壁纸
- 开机自启补刷（可选，默认开启）

**版本**: 1.0.2

## 安装

1. 下载 `fn-bing-wallpaper-weekly-1.0.2.fpk`
2. 飞牛 OS → 应用中心 → **手动安装** → 选择该文件
3. 向导中填写壁纸保存目录（凭证类字段已全部移除）
4. 安装完成后自动执行一次，立即生效

> 若此前安装过旧版（1.1.x），请先卸载，避免两个定时任务重复上传。

## 兼容性

- 平台: x86
- 飞牛 OS >= 1.1.8

## 工作原理（无需凭证）

应用以 root 运行，通过直写飞牛系统实现（无需登录会话）：

1. 下载必应当日壁纸到自定义目录
2. 拷贝到飞牛壁纸库 `/usr/trim/var/trim_sac/wallpaper/<uid>/`（ffmpeg 生成缩略图）
3. 写入壁纸库记录（`trim_sac.wallpaper` 表）
4. 更新桌面壁纸设置（`trim_sac.user_preference.single_photo_wp_id`，等价前端「设为壁纸」操作）

日志：`/vol1/@appdata/fn-bing-wallpaper-weekly/cron.log`

## 其他

- 作者: [TXH](https://github.com/txhtxh11)
- 博客: <https://txhtxh11.github.io>
