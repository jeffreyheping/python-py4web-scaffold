# python-py4web-scaffold

py4web 框架的 scaffold 应用脚手架，基于上游 py4web 仓库的 `apps/_scaffold` 目录提取，支持与上游同步。

## 项目结构

```
python-py4web-scaffold/
├── README.md
└── scaffold/              # 通过 git subtree 管理的 py4web scaffold
    ├── AGENTS.md
    ├── __init__.py
    ├── common.py
    ├── controllers.py
    ├── models.py
    ├── settings.py
    ├── tasks.py
    ├── templates/
    ├── static/
    └── translations/
```

## 分支结构

- **main**: 主分支，包含 scaffold 子目录（通过 subtree 管理）
- **scaffold**: 独立的 scaffold 分支，保持与上游同步

## 与上游同步

### 方式一：更新 scaffold 分支（推荐）

```bash
# 1. 切换到 scaffold 分支
git checkout scaffold

# 2. 添加 py4web 远程（如果还没有）
git remote add py4web https://github.com/web2py/py4web.git

# 3. 从上游拉取更新
git fetch py4web

# 4. 合并上游的 apps/_scaffold 目录变更
git merge py4web/master --strategy=ours
git read-tree --prefix=apps/_scaffold -u py4web/master:apps/_scaffold
git commit -m "sync: update scaffold from py4web"

# 5. 移动文件到根目录
git mv apps/_scaffold/* .
rm -rf apps
git add -A
git commit -m "move scaffold files to root"

# 6. 推送到远程
git push -f origin scaffold
```

### 方式二：直接从上游提取并更新 subtree

```bash
# 从上游提取最新的 _scaffold
cd /tmp
rm -rf py4web-temp
git clone --depth=1 https://github.com/web2py/py4web.git py4web-temp
cd py4web-temp
git-filter-repo --path=apps/_scaffold --force
mv apps/_scaffold/* .
rm -rf apps

# 推送到 scaffold 分支
git remote add origin https://github.com/jeffreyheping/python-py4web-scaffold.git
git push -f origin HEAD:scaffold

# 更新主分支的 subtree
cd /path/to/your/repo
git fetch origin
git subtree pull --prefix=scaffold origin scaffold --squash
```

### 方式三：直接在主分支更新 subtree

```bash
# 更新 scaffold subtree
git fetch origin scaffold
git subtree pull --prefix=scaffold origin scaffold --squash
```

## 来源

本项目的 `scaffold` 目录提取自 [py4web](https://github.com/web2py/py4web) 仓库的 `apps/_scaffold` 目录。

## 使用方法

1. 克隆本仓库
2. 进入 scaffold 目录
3. 根据需要修改配置
4. 集成到您的 py4web 应用中

## 更多信息

- [py4web 官方文档](https://py4web.com/)
- [py4web GitHub 仓库](https://github.com/web2py/py4web)

## 许可证

继承 py4web 的 BSD-3-Clause 许可证。
