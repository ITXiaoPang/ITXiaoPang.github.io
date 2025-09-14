---
layout: post
title: 从pyenv和poetry迁移到uv
abbrlink: 7b70d9f1ca2b4b8eb1b6ad5c454cb1bc
tags:
  - uv
  - python
  - pyenv
  - poetry
categories:
  - IT
  - 开发
  - Python
  - 包管理
  - uv
date: 1751802289305
updated: 1757670404302
---

# 格式转换

```shell
uvx migrate-to-uv
```

## python版本，虚拟环境管理

| 操作              | uv                       | poetry                       | pyenv                               | 备注                                   |
| --------------- | ------------------------ | ---------------------------- | ----------------------------------- | ------------------------------------ |
| 更新工具自身          | uv self update           | poetry self update           | pyenv update                        |                                      |
| 查看所有Python版本    | uv python list           | poetry python list           | pyenv versions                      |                                      |
| 安装特定版本Python    | uv python install 3.13.5 | poetry python install 3.13.5 | pyenv install 3.13.5                |                                      |
| 创建Python虚拟环境    | uv venv --python 3.13.5  | poetry env use 3.13.5        | pyenv virtualenv 3.13.5 `venv_name` | uv默认不安装pip，如需要增加参数--seed。poetry会自动创建 |
| 在当前目录锁定Python版本 | uv python pin 3.13.5     | 不需要                          | pyenv local  `venv_name`            | 目录下生成.python-version，poetry不生成       |
| 激活python虚拟环境    | uv run `command`         | poetry env use 3.13.5        | pyenv shell `venv_name`             | pyenv 用过local后，cd到目录自动激活             |
| 查看当前Python版本    | uv run python --version  | poetry env info              | pyenv version                       | 也可以poetry run python -V              |

## python包管理

| 操作       | uv                                              | poetry                                        | 备注                       |
| -------- | ----------------------------------------------- | --------------------------------------------- | ------------------------ |
| 创建应用项目   | uv init `proj_name` --python 3.13.5             | poetry new --flat `proj_name` --python 3.13.5 |                          |
| 创建打包项目   | uv init --package `proj_name` --python 3.13.5   | poetry new `proj_name` --python 3.13.5        | 支持执行该命令，代码放到src文件夹中      |
| 创建库项目    | uv init --lib `proj_name` --python 3.13.5       |                                               | 库为其他项目提供函数和对象，始终是一个打包的项目 |
| 安装包      | uv add `包1` `包2`                                | poetry add `包1` `包2`                          | 如果没有虚拟环境，都会自动创建一个        |
| 安装扩展包    | uv add sqlalchemy --extra pymysql --extra mysql | poetry add sqlalchemy -E pymysql -E mysql     |                          |
| 删除包      | uv remove `包1` `包2`                             | poetry remove `包1` `包2`                       | 删除不用的依赖                  |
| 安装特定版本的包 | uv add `包1==x.xx.x` `包2==x.xx.x`                | uv add `包1==x.xx.x` `包2==x.xx.x`              |                          |
| 导入依赖     | uv add -r requirements.txt                      | poetry add \$(cat requirements.txt)           |                          |
| 同步包      | uv sync --no-install-project                    | poetry sync --no-root                         |                          |

## uv run

| 命令               | 检查 uv.lock 与 pyproject.toml 一致性 | 更新 uv.lock | 同步虚拟环境 | 适用场景              |
| ---------------- | ------------------------------- | ---------- | ------ | ----------------- |
| uv run           | 是                               | 是          | 是      | 开发环境，动态更新依赖       |
| uv run --locked  | 是（若不一致则报错）                      | 否          | 是      | CI/CD 或生产环境，严格可重现 |
| uv run --frozen  | 否                               | 否          | 是      | 快速开发，信任现有锁文件      |
| uv run --no-sync | 否                               | 否          | 否      | 性能敏感，已手动同步环境      |

## 官方网站

- PEP518:<https://peps.python.org/pep-0518/>
- uv官方文档:<https://docs.astral.sh/uv/>
- pyenv用法介绍:<https://github.com/pyenv/pyenv>
- pyenv官方安装工具:<https://github.com/pyenv/pyenv-installer>
- poetry官方文档:<https://python-poetry.org/docs/>
