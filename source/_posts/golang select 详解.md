title: mise 使用详解
date: 2026-06-11 16:55:00
tags: mise

---

### 安装

```bash
brew install mise
```



### 常用命令

|       命令        |          作用          |
| :---------------: | :--------------------: |
|      mise ls      |     查看已安装工具     |
| mise ls --current | 查看当前目录生效的版本 |
|  mise ls-remote   |   查询远程可安装版本   |
|   mise install    |  安装配置文件中的工具  |
|     mise use      |   安装并配置工具版本   |
|    mise use -g    |    设置全局默认版本    |
|  mise uninstall   |      卸载指定版本      |
|   mise upgrade    |        升级工具        |
|    mise where     |    查看工具安装目录    |
|    mise which     |  查看命令实际执行路径  |
|    mise doctor    |     检查 mise 环境     |
|    mise config    |        查看配置        |
|     mise exec     |  使用指定环境执行命令  |
|     mise run      |      执行项目任务      |

