---
title: EasyClick安卓文档_安卓手机自动化脚本_MCP服务对接
hide_title: false
hide_table_of_contents: false
sidebar_label: MCP服务对接
description: 'EasyClick 云控平台专门用于用于管理脚本、任务、数据的web平台,可以进行远程投屏设备，异地组网，远程操作设备'
keywords:
  - EasyClick
  - 手机自动化脚本
  - 自动化软件
  - 云控平台
  - 抖音云控
  - 快手云控
  - 游戏云控
  - MCP
  - https
  - E6
  - E5
  - E4
  - B8
  - AE
  - www
  - cn
  - 手机自动化
  - 自动化测试
---
# 云控MCP对接

## 什么是MCP
:::tip
- MCP（Model Context Protocol，模型上下文协议）是由Anthropic公司推出的开源协议，旨在标准化大语言模型（LLM）与外部数据源、工具的交互方式，类似于AI领域的“USB-C接口”。
- MCP描述文档: https://baike.baidu.com/item/%E6%A8%A1%E5%9E%8B%E4%B8%8A%E4%B8%8B%E6%96%87%E5%8D%8F%E8%AE%AE/65540618
- 云控遵循MCP协议，可以使用大模型和云控使用自然语言交互，无需编写代码
:::

## 云控MCP支持哪些平台
- [扣子空间](https://www.coze.cn/space)、Dify、vscode、[Trae](https://www.trae.cn/)等
- 凡是遵守MCP调用的平台，都能支持

## 如何使用云控的MCP
- 安装好云控，云控的MCP运行端口是 `8187`，需要在宝塔的安全组以及云服务的安全组等放行这个端口
- 到云控站点的`权限管理-用户管理`中, 找到`云控设定`，点击可以看到弹窗
- 如果没有`MCP密钥`数据，点击生成，然后复制，在点击确定进行保存
## 生成MCP服务配置
- 获取MCP密钥后进行一下更改以下的配置
  - 替换服务器IP为你的云控服务IP
  - 替换MCP密钥为你复制的密钥
```json
{
	"mcpServers": {
		"EcloudMCP_zuhu": {
			"type": "streamable-http",
			"url": "http://服务器IP:8187/mcp",
			"headers": {
				"UserToken": "MCP密钥"
			}
		}
	}
}
```

例如：
```json
{
	"mcpServers": {
		"EcloudMCP_zuhu": {
			"type": "streamable-http",
			"url": "http://192.168.2.12:8187/mcp",
			"headers": {
				"UserToken": "6c2657067fb7459bb7f84b6afab5e5b1"
			}
		}
	}
}
```

## 配置到大模型平台

- 这里以Trae为例，其他模型平台类似
- 到 [Trae](https://www.trae.cn/) 下载并安装打开软件
- 找到右侧AI功能管理
<img src="/mcp/1.png" alt="找到右侧AI功能管理" style={{zoom:'33%'}} />
- 找到`MCP`一栏，点击添加按钮，选择手动添加
<img src="/mcp/2.png" alt="找到`MCP`一栏，点击添加按钮，选择手动添加" style={{zoom:'33%'}} />
- 复制上面生成的MCP 配置信息到 输入框，点击确认
<img src="/mcp/3.png" alt="复制上面生成的MCP 配置信息到 输入框，点击确认" style={{zoom:'33%'}} />
- Trae会自动链接到MCP服务如图所示,会出现支持的功能列表
<img src="/mcp/4.png" alt="Trae会自动链接到MCP服务如图所示" style={{zoom:'33%'}} />
- 点击`智能体`按钮，选择`按钮`
<img src="/mcp/5.png" alt="AI功能管理" style={{zoom:'33%'}} />
- 智能体名称随便写，勾选上 刚才配置的 MCP服务名称后点击创建
<img src="/mcp/6.png" alt="MCP服务名称后点击创建" style={{zoom:'33%'}} />
- 开始和智能体对话，例如输入`执行云控任务，名称为323`，或者`停止云控任务，名称为323`
<img src="/mcp/7.png" alt="开始和智能体对话" style={{zoom:'33%'}} />

## MCP的优势
- MCP一般会结合工作流使用，作为服务的一方，给大模型提供资源和数据
  - 例如配合扣子空间AI生成视频后，直接开启云控任务执行下一步动作，这个只是其中的一个场景
- 在一些简单的自动化任务中，无需编写代码，使用大模型进行语义理解进行完成想要的功能

## 更多待开发需求
- 如果你有更多的关于MCP等想法和需求，请联系沙书记QQ 2557945562，进行探讨
