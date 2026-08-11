
<img width="800" height="488" alt="image" src="https://github.com/user-attachments/assets/90c8ac91-fb4e-4211-85ae-69d8f5f898e6" />

# fn-bing-wallpaper

飞牛 fnOS 应用：**每日自动轮换必应 4K 壁纸**

- 每日自动轮换必应 4K 壁纸
- 旧图自动清理
- 开机自启补刷
- 让飞牛桌面每天都有新风景

**版本**: 1.1.0（修复 Host 头路由与凭证完整性问题，支持全新安装即用）

## 安装

1. 下载 `fn-bing-wallpaper.fpk`
2. 飞牛 OS → 应用中心 → **手动安装** → 选择该文件
3. 安装完成后启动即可

## 兼容性

- 平台: x86
- 飞牛 OS >= 1.1.8

## 其他

- 作者: [TXH](https://github.com/txhtxh11)
- 博客: <https://txhtxh11.github.io>

---

## 抓取你的专属「API 通行证」

要让脚本全自动伪装成你来更换壁纸，必须先拿到你的身份密钥。

1. 登录飞牛网页桌面，按下键盘 **F12** 打开开发者工具。
2. 切换到 **Network（网络）** 面板，点击过滤器中的 **Fetch/XHR**，然后点击左上角的「🚫」图标清空当前记录。
3. 在飞牛桌面的「个人设定 → 壁纸」里，手动上传一张任意的本地图片并设置为壁纸。
4. 网络面板中会立刻弹出一个名为 **uploadWallpaper** 的请求，点击它。
5. 在右侧的 **Request Headers（请求标头）** 中，找到并复制以下两项机密数据，先保存到记事本：
   - **Authorization**（通常是一串以 `trim` 开头的长字符）
   - **Cookie**（包含 `fnos-token=...` 和 `SID=...` 的长字符串）

---

## 安装注意事项（实测踩坑记录）

### 1. Cookie 有长度限制
飞牛应用中心安装向导的字段存入数据库时**单字段上限 1024 字符**。
从浏览器复制的完整 Cookie 往往超长（含 `memos_refresh`、`remember-me` 等无用项），会导致安装报错：
`服务异常无法安装` / `ERROR: value too long for type character varying(1024)`。

**解决**：只保留必要项即可，精简后的 Cookie 大约 300 字符：

```
device_id=xxx; fnos-long-token=xxx; os-access-code=xxx; language=zh-CN; fnos-token=xxx
```

（`fnos-token` 的值就是 Authorization 中 `trim` 后面那段。）

### 2. 凭证有效期约 24 小时
`fnos-token` 每天过期，过期后上传返回 `401 trim token fail`。
更新方法：重新按上方「API 通行证」教程抓取，然后在应用设置中更新 Authorization 与 Cookie（或重装时填入）。

### 3. 定时任务
应用会写入 crontab（每日 00:05 + 可选开机补刷），日志在
`/vol1/@appdata/fn-bing-wallpaper/cron.log`。
若重复安装过多次可能出现重复的 cron 条目，可手动清理：
`crontab -e` 删除重复的 `fn-bing-wallpaper` 行。
