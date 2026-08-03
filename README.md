# 🍑 桃·华夏图志

以地图为卷、写一果之千年。本仓库是一套「桃文化历史」主题的交互式地图页面合集，使用 OpenLayers / Leaflet 实现蚂蚁线动画路线，讲述桃自横断山区起源、东传京华、南下江南、西渡丝路直至盛唐的千年旅程。

## 📁 项目结构

| 文件 | 内容 | 在线路径 |
|---|---|---|
| `index.html` | 首页地图：横断山区 → 新疆绿洲（桃之起源） | `/index.html` |
| `tao2.html` | 桃东传 → 北京 | `/tao2.html` |
| `tao3.html` | 桃南下 → 江南水乡 | `/tao3.html` |
| `tao-world.html` | 桃沿丝绸之路西传、走向世界 | `/tao-world.html` |
| `mawei.html` | 香消玉殒 · 马嵬遗恨 | `/mawei.html` |
| `tang.html` | 盛世极峰 · 盛唐牡丹 | `/tang.html` |

> 所有页面相互独立，各自通过 `域名/文件名` 直接访问，无导航首页。

## 🧭 部署架构

```
用户浏览器
    │
    ▼
https://saturday-earwig-unlisted.ngrok-free.dev/   ← ngrok 免费隧道（https 域名）
    │
    ▼
Droplet 服务器 167.71.192.92 : 80   ← nginx 静态站点
    │
    ▼
/var/www/html（本仓库 git clone）
```

- **GitHub 仓库**：`Palpi-tate/Map`（公开，分支 `main`）—— 唯一代码源
- **服务器**：DigitalOcean Droplet（Ubuntu 24.04，`/var/www/html` 是 git clone）
- **域名**：ngrok 免费隧道域名（重启/断线后地址可能变化）

## 🔄 工作流程

### 日常更新页面（改现有 HTML）

1. **本地改文件**：编辑 `C:\Users\Administrator\Desktop\Map_repo\` 下的 HTML（这是本地同步副本）
2. **提交推送**：
   ```bash
   cd C:\Users\Administrator\Desktop\Map_repo
   git add -A
   git commit -m "更新说明"
   git push origin main
   ```
3. **服务器同步**：打开 Droplet 的 **Web Console**（root），执行：
   ```bash
   cd /var/www/html
   git pull origin main
   ```
4. **验证**：浏览器访问对应页面即可看到更新（注意 ngrok 首次访问需点 "Visit Site"）。

### 新增页面

1. 把新 HTML 放入 `Map_repo` 目录
2. 按上面"提交推送 + 服务器同步"两步走
3. 建议用英文短名（如 `xxx.html`），避免中文文件名产生 URL 乱码

### 首次部署（新服务器）

```bash
# 服务器上安装 nginx 后
apt install -y nginx git
git clone https://github.com/Palpi-tate/Map.git /var/www/html
# 确认 nginx 站点根目录指向 /var/www/html
```

## 📌 注意事项

- **旧中文文件名链接已废弃**：早期版本使用 `传播到世界.html` 等中文文件名，现已改为英文短名，旧链接会返回 404。
- **ngrok 拦截页**：免费版首次访问会显示 "Visit Site" 确认页，点一下即可，之后不再出现。
- **服务器直连 GitHub**：若本地网络无法直连 GitHub，已为 git 配置系统代理 `127.0.0.1:7897`。
- **外网 SSH(22) 关闭**：无法直接 SSH，统一通过 DigitalOcean 的 **Web Console** 操作服务器。
- **App Platform**：账号下没有已部署的 App Platform 应用，服务全部运行在 Droplet + ngrok 上。

## 📬 如何访问（访问流程）

### 方式一：通过 ngrok 域名（推荐，https，可分享给任何人）

**第 1 步**：打开浏览器，输入以下完整地址（域名 + 页面路径）：

| 页面 | 完整访问地址 |
|---|---|
| 首页（桃之起源） | `https://saturday-earwig-unlisted.ngrok-free.dev/index.html` |
| 桃·东传北京 | `https://saturday-earwig-unlisted.ngrok-free.dev/tao2.html` |
| 桃·南下江南 | `https://saturday-earwig-unlisted.ngrok-free.dev/tao3.html` |
| 桃·西传世界 | `https://saturday-earwig-unlisted.ngrok-free.dev/tao-world.html` |
| 马嵬遗恨 | `https://saturday-earwig-unlisted.ngrok-free.dev/mawei.html` |
| 盛唐牡丹 | `https://saturday-earwig-unlisted.ngrok-free.dev/tang.html` |

**第 2 步**：首次访问会看到 ngrok 的 "Visit Site" 确认页
- 点击 **Visit Site** 按钮即可进入真实页面
- 同一个浏览器之后再次访问不会再出现该确认页

**第 3 步**：进入后即为全屏交互地图页面，直接用鼠标/手指拖动查看即可。

> 💡 手机、电脑、任何设备都能打开，无需安装任何软件；复制上面的完整链接发给别人就能直接访问。

### 方式二：通过服务器公网 IP（备选，无 https 证书）

浏览器直接访问服务器 IP 的 80 端口：

- `http://167.71.192.92/index.html`
- 或 `http://167.71.192.92/tao2.html` 等

> ⚠️ 注意：IP 方式为 http（无加密），仅作备选；日常分享请使用 ngrok 的 https 域名。

### 访问流程速览

```
浏览器输入地址 → (仅首次) ngrok 确认页点 "Visit Site" → 看到地图页面
```

- 地址 = 域名 + `/` + 文件名（如 `/tao2.html`）
- 想访问哪张图，就把对应的文件名换成上表的路径即可
- 域名若失效（ngrok 免费版重启后地址可能变化），请按"工作流程"重新获取域名并同步服务器
