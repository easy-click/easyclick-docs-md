---
title: EasyClick Android — MCP integration
hide_title: false
hide_table_of_contents: false
sidebar_label: MCP integration
description: 'EasyClick cloud control — web platform for scripts, tasks, and data; remote mirroring, cross-site networking, remote device control'
keywords:
 - EasyClick
 - mobile automation scripts
 - automation software
 - cloud control platform
 - Douyin cloud control
 - Kuaishou cloud control
 - game cloud control
 - MCP
 - https
 - E6
 - E5
 - E4
 - B8
 - AE
 - www
 - cn
 - mobile automation
 - automation testing
---
# Cloud control MCP

## What is MCP
:::tip
- MCP (Model Context Protocol) is an open protocol from Anthropic that standardizes how LLMs talk to external data and tools — like a “USB-C for AI”
- Overview: https://baike.baidu.com/item/%E6%A8%A1%E5%9E%8B%E4%B8%8A%E4%B8%8B%E6%96%87%E5%8D%8F%E8%AE%AE/65540618
- Cloud control speaks MCP, so you can drive it with natural language via an LLM — no custom code required
:::

## Supported platforms
- [Coze](https://www.coze.cn/space), Dify, VS Code, [Trae](https://www.trae.cn/), and more
- Any platform that follows MCP can connect

## How to use cloud MCP
- Install cloud control. MCP listens on port **`8187`** — open it in BT Panel security and your cloud provider security group
- Go to **Permission → User management → Cloud settings**
- If there is no **MCP key**, generate one, copy it, then confirm to save
## MCP server config
- After you have the key, edit the config below:
 - Replace the server IP with your cloud control IP
 - Replace the MCP key with the key you copied
```json
{
	"mcpServers": {
		"EcloudMCP_zuhu": {
			"type": "streamable-http",
			"url": "http://SERVER_IP:8187/mcp",
			"headers": {
				"UserToken": "MCP_KEY"
			}
		}
	}
}
```

Example:
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

## Configure in an LLM platform

- Example uses Trae; other platforms are similar
- Download and open [Trae](https://www.trae.cn/)
- Open AI feature management on the right
<img src="/mcp/1.png" alt="AI feature management" style={{zoom:'33%'}} />
- Open **MCP** → Add → Manual
<img src="/mcp/2.png" alt="MCP → Add → Manual" style={{zoom:'33%'}} />
- Paste the MCP config above and confirm
<img src="/mcp/3.png" alt="Paste MCP config" style={{zoom:'33%'}} />
- Trae connects and lists supported tools
<img src="/mcp/4.png" alt="MCP connected" style={{zoom:'33%'}} />
- Click **Agent** → create
<img src="/mcp/5.png" alt="Create agent" style={{zoom:'33%'}} />
- Name the agent, check the MCP server you configured, then create
<img src="/mcp/6.png" alt="Select MCP server" style={{zoom:'33%'}} />
- Chat with the agent, e.g. `Run cloud task named 323` or `Stop cloud task named 323`
<img src="/mcp/7.png" alt="Chat with agent" style={{zoom:'33%'}} />

## Why MCP helps
- Often used in workflows as a tool/data provider for the LLM
 - Example: Coze generates a video, then starts a cloud task for the next step — one of many scenarios
- For simple automation, skip coding and let the model interpret intent

## Feature requests
- For more MCP ideas or needs, contact Secretary Sha (QQ 2557945562)
