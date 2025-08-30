MCP is an open protocol that standardizes how applications provide context to LLMs. 

Think of MCP like a USB-C port for AI applications. Just as USB-C provides a standardized way to connect your devices to various peripherals and accessories, MCP provides a standardized way to connect AI models to different data sources and tools.

# Motivation
Even the best models are constrained by their isolation from data, and every new data source requires its own custom implementation, making truly connected systems difficult to scale. 

MCP provides a universal, open standard for connecting AI systems with data sources, replacing fragmented integrations with a single protocol. 

**Result**: Simple and reliable way to give AI systems to the data they need

# Introduction
MCP is an open standard that enables developers to build ==secure, two-way connections== between their data sources and AI-powered tools.

**Options**: 
1. Expose data to MCP servers
2. Build AI applications(MCP Clients) that connect to these servers

## Concepts
MCP servers can provide three main types of capabilities:
1. **Resources**: File-like data that can be read by clients (like API responses or file contents)
2. **Tools**: Functions that can be called by the LLM (with user approval)
3. **Prompts**: Pre-written templates that help users accomplish specific tasks







## References
1. https://modelcontextprotocol.io/
2. 