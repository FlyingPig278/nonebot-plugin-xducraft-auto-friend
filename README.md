# XDUCraft Auto Friend

收到用户的好友申请时，核实申请人是否属于配置的群聊白名单，并在满足群功能与全局开关启用条件时自动通过申请。

如果需要部署包含全部插件的完整机器人，请阅读[整套机器人的安装说明](https://github.com/FlyingPig278/XDUCraft-bot)。

## 功能与行为

OneBot v11 协议的好友申请事件中不包含来源群号。收到好友申请后，机器人按白名单顺序查询申请人是否在已启用的群中。若申请人属于白名单中的群，且该群和全局均已开启本功能，机器人会自动同意申请。若不属于任何已启用的白名单群，或群成员信息查询失败，申请保持未处理状态，不会主动拒绝。

## 安装与加载

在现有 NoneBot 2.4.x 项目环境中，先安装 Core 核心包，再安装本插件：

```bash
pip install "nonebot-plugin-xducraft-core[onebot] @ git+https://github.com/FlyingPig278/nonebot-plugin-xducraft-core.git@v0.3.0"
pip install "nonebot-plugin-xducraft-auto-friend @ git+https://github.com/FlyingPig278/nonebot-plugin-xducraft-auto-friend.git@v0.1.0"
```

在已有项目的 `pyproject.toml` 中的 `plugins` 列表中追加模块，不要覆盖已有配置：

```toml
[tool.nonebot]
plugins = [
    # ... 已有插件 ...
    "xducraft_config",
    "xducraft_auto_friend",
]
```

## 白名单与重启生效说明

允许自动通过好友申请的群号白名单可在 NoneBot 部署目录的 `.env` 中设置，例如 `AUTO_ACCEPT_FRIEND_GROUP_IDS='[123456789, 987654321]'`，也可以在 WebUI 管理面板中修改。

由于白名单在机器人启动时读取，在网页面板修改白名单后必须重启 NoneBot 进程才能加载生效。

## 启用步骤

插件的全局总开关与群功能开关默认均为关闭。自动通过好友申请需按以下顺序完成配置：

首先在 NoneBot 部署目录的 `.env` 或 WebUI 管理面板中设置允许加好友的群号白名单，并重启 NoneBot 进程加载该配置。

接着在 WebUI 管理面板的插件页面开启自动加好友插件，或者参考[不使用网页时启用插件](https://github.com/FlyingPig278/nonebot-plugin-xducraft-core#不使用网页时启用插件)在代码中单次启用。

最后在目标白名单群中开启该功能。若安装了 `nonebot-plugin-xducraft-features` 插件，群管理员可在群内发送 `/功能 on 自动加好友`；若未安装该插件，请在 WebUI 管理面板的群聊页面开启，或者参考上述 Core 链接中的代码启用该群。

## 数据存储

本插件不产生独立的业务数据文件，修改后的白名单配置保存在 Core 配置数据库中。

## 本地开发与测试

```bash
pip install "nonebot-plugin-xducraft-core[onebot] @ git+https://github.com/FlyingPig278/nonebot-plugin-xducraft-core.git@v0.3.0"
pip install -e ".[dev]"
pytest
python -m build
```

## 许可证

本项目采用 [MIT License](LICENSE) 授权开源。
