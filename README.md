[README.md](https://github.com/user-attachments/files/28435939/README.md)
# 随想录 — 个人随笔网站部署指南

> 极简个人随笔网站，支持本地存储与云端同步（Supabase / JSONBin.io 双方案），一键部署到互联网。

---

## 文件清单

| 文件 | 说明 |
|------|------|
| `index.html` | 主站点文件（GitHub Pages 默认首页） |
| `thoughts.html` | 基础版站点（不含 JSONBin.io） |
| `import_data.json` | 初始随笔数据集（34 篇，可选导入） |
| `README.md` | 本部署指南 |

---

## 快速部署（三选一）

### 方式一：GitHub Pages（最推荐，完全免费）

1. 在 [GitHub](https://github.com) 创建新仓库（如 `thoughts`）
2. 将 `index.html` 上传到仓库根目录
3. 进入 `Settings` → `Pages` → Source 选 `main` 分支 → 点 `Save`
4. 等待 1-2 分钟，访问 `https://你的用户名.github.io/thoughts/`

### 方式二：Vercel

1. 访问 [vercel.com](https://vercel.com)，用 GitHub 账号登录
2. 点击 `Add New` → `Project` → 导入 GitHub 仓库
3. 无需任何配置，直接点 `Deploy`
4. 获得 `https://xxx.vercel.app` 域名，支持绑定自定义域名

### 方式三：Netlify

1. 访问 [app.netlify.com](https://app.netlify.com)
2. 将整个 `output` 文件夹拖拽到部署区域
3. 自动上线，获得 `https://xxx.netlify.app` 域名

---

## 云端存储配置

部署到互联网后，数据默认保存在浏览器本地。如需多设备同步，请配置云端后端。

### 方案 A：Supabase（功能完整）

**优点**：细粒度 CRUD、实时同步、免费额度 500MB 数据库  
**缺点**：需注册账号、建表、配置 RLS 策略

**步骤**：

1. 注册 [supabase.com](https://supabase.com)（支持 GitHub 登录）
2. 创建新项目，记住数据库密码
3. 进入 `SQL Editor`，执行以下 SQL：

```sql
CREATE TABLE thoughts (
  id TEXT PRIMARY KEY,
  title TEXT,
  content TEXT,
  timestamp BIGINT
);

ALTER TABLE thoughts ENABLE ROW LEVEL SECURITY;

CREATE POLICY "anon_all" ON thoughts
  FOR ALL USING (true) WITH CHECK (true);
```

4. 进入 `Settings` → `API`，复制 `Project URL` 和 `anon/public` key
5. 打开网站 → 点击齿轮图标 → 选择 **Supabase** 标签 → 填入 URL 和 Key → 保存

### 方案 B：JSONBin.io（极简易用）

**优点**：无需建表、一个 Bin 即后端、API 极简  
**缺点**：整存整取（无细粒度 CRUD）、免费版 10K 请求/月

**步骤**：

1. 注册 [jsonbin.io](https://jsonbin.io)（支持 Google/GitHub 登录）
2. 点击 `Create Bin` → 填入初始数据 `[]`（空数组即可）→ 命名为 `thoughts`
3. 复制 Bin ID（URL 中 `/b/` 后的字符串，如 `67a1b2c3e41b4d34e4a1b2c3`）
4. （可选）进入 `API Keys` 创建 Access Key 以保护 Bin
5. 打开网站 → 点击齿轮图标 → 选择 **JSONBin.io** 标签 → 填入 Bin ID → 保存

---

## 方案对比

| 维度 | GitHub Pages | Vercel | Netlify | Supabase | JSONBin.io |
|------|:---:|:---:|:---:|:---:|:---:|
| **费用** | 免费 | 免费 | 免费 | 免费（500MB）| 免费（10K/月）|
| **部署难度** | 极简 | 极简 | 极简 | 中等 | 简单 |
| **自定义域名** | 支持 | 支持 | 支持 | — | — |
| **HTTPS** | 自动 | 自动 | 自动 | — | — |
| **数据同步** | — | — | — | 实时、细粒度 | 整存整取 |
| **需注册账号** | GitHub | GitHub | 任意 | Supabase | JSONBin.io |
| **建表/配置** | 无 | 无 | 无 | 需 SQL | 点点按钮 |

---

## 使用说明

- **写随笔**：点击右上角「写随笔」按钮
- **查看随笔**：首页按时间倒序展示所有随笔
- **汇总页面**：点击「汇总」标签，查看统计和热力图
- **云同步**：导航栏齿轮图标，配置 Supabase 或 JSONBin.io
- **导入数据**：同步栏「导入本地文件」，支持 JSON 格式
- **导出数据**：同步栏「导出全部数据」，下载 JSON 备份

---

## 技术栈

- 纯前端：HTML + CSS + 原生 JavaScript
- 本地存储：localStorage
- 云端后端：Supabase REST API / JSONBin.io v3 API
- 零依赖：无框架、无构建工具
