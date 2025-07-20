### Introduction

MCP stands for  Model Context Protocol (MCP)

MCP is an open protocol that standardizes how applications provide context to LLMs. Think of MCP like a USB-C port for AI applications. Just as USB-C provides a standardized way to connect your devices to various peripherals and accessories, MCP provides a standardized way to connect AI models to different data sources and tools.## 

Why MCP?

MCP helps you build agents and complex workflows on top of LLMs. LLMs frequently need to integrate with data and tools, and MCP provides:* A growing list of pre-built integrations that your LLM can directly plug into

* The flexibility to switch between LLM providers and vendors
* Best practices for securing your data within your infrastructure

### Core MCP Concepts

MCP servers can provide three main types of capabilities:1.  **Resources** : File-like data that can be read by clients (like API responses or file contents)

1. **Tools** : Functions that can be called by the LLM (with user approval)
2. **Prompts** : Pre-written templates that help users accomplish specific tasks

### Logging in MCP Servers

When implementing MCP servers, be careful about how you handle logging:**For STDIO-based servers:** Never write to standard output (stdout). This includes:* `print()` statements in Python

* `console.log()` in JavaScript
* `fmt.Println()` in Go
* Similar stdout functions in other languages

Writing to stdout will corrupt the JSON-RPC messages and break your server.**For HTTP-based servers:** Standard output logging is fine since it doesn’t interfere with HTTP responses.###
