# 糖分打卡 · 每日一点点

单文件的个人习惯打卡 + 任务管理仪表盘。双击 `index.html` 就能用，不需要安装和构建，数据存在浏览器本地，可选 Supabase 手机⇄电脑同步。

## 功能

- **今日**：习惯打卡、补记往日、今日时间线、本周小结、糖分余额
- **看板**：待办 / 进行中 / 已完成三列，支持拖拽换列，标签点一下可改名
- **收集箱**：灵感先存着，一键变成项目 / 任务 / 习惯
- **项目**：进度、子任务、里程碑、项目笔记
- **统计**：月度切换、近 14 天柱状图、打卡热力图、奖励展板与兑换记录
- **数据**：JSON 导入导出、Supabase 云同步（含手机配对链接）、标签管理

糖分规则：打卡 +1，完成一个任务 +2，兑换奖励时扣除。

## 部署到 GitHub Pages

1. 把 `index.html` 传到仓库（`糖分打卡3.html` 改名即可）
2. Settings → Pages → Source 选 `Deploy from a branch`，分支 `main`、目录 `/ (root)`
3. 等 1 分钟，访问 `https://<用户名>.github.io/sugar-check/`

## 可选：云同步

1. Supabase 新建项目，在 SQL Editor 里执行：

```sql
create table if not exists habit_snapshots (
  id text primary key,
  payload jsonb not null,
  updated_at timestamptz default now()
);
alter table habit_snapshots enable row level security;
create policy "anon_rw_snapshots" on habit_snapshots
  for all to anon using (true) with check (true);
```

2. 应用内「数据」页填 Project URL、anon key、表名、同步密钥
3. 点「保存并立即同步」，再点「复制手机配对链接」发到手机打开即可

## 说明

- 本地存储键名 `sugarcheck.v3`，兼容读取 `sugarcheck.v2` 老数据
- 数据默认只在这台浏览器里，换设备或清缓存前先导出 JSON / 开同步
- 个人使用，无多人协作、无推送；云同步是整份快照合并
