# MCP Prompt Server

> 🚀 **Upgrade Notice!**
>
> We have launched a brand new upgraded service based on MCP Prompt Server scenarios: [mypromptmcp.com](https://mypromptmcp.com/).
>
> On this platform, you can freely manage your own prompt templates online, flexibly combine and package them into MCP services, and easily integrate with various AI Agents or tools (such as Cursor, Windsurf, Cline, etc.), enabling cross-platform invocation and efficient collaboration.
>
> Experience more powerful prompt management and distribution now at [mypromptmcp.com](https://mypromptmcp.com/)!
>
> ⚠️ **Important Reminder:**
>
> MCP Prompt Server will continue to work, but we will no longer provide feature upgrades or maintenance. If you want to keep using the latest features, please migrate to [mypromptmcp.com](https://mypromptmcp.com/).

This is a Model Context Protocol (MCP) based server that provides predefined prompt templates based on user task requirements, helping Cline/Cursor/Windsurf and other editors to execute various tasks more efficiently. The server returns predefined prompts as tools for better integration with editors like Cursor and Windsurf.

## Features

- Provides predefined prompt templates for code review, API documentation generation, code refactoring, and more
- Offers all prompt templates as MCP tools rather than MCP prompts format
- Supports dynamic parameter replacement for flexible prompt templates
- Allows developers to freely add and modify prompt templates
- Provides tool APIs for reloading prompts and querying available prompts
- Optimized for editors like Cursor and Windsurf for better integration experience

## Directory Structure

```
prompt-server/
├── package.json         # Project dependencies and scripts
├── src/                # Source code directory
│   ├── index.js         # Server entry file
│   └── prompts/         # Predefined prompt templates directory
│       ├── code_review.yaml
│       ├── api_documentation.yaml
│       ├── code_refactoring.yaml
│       ├── test_case_generator.yaml
│       └── project_architecture.yaml
└── README.md            # Project documentation
```

## Installation and Usage

1. Install dependencies:

```bash
cd prompt-server
npm install
```

2. Start the server:

```bash
npm start
```

The server will run on standard input/output and can be connected by Cursor, Windsurf, or other MCP clients.

## Adding New Prompt Templates

You can create new prompt templates by adding new YAML or JSON files in the `src/prompts` directory. Each template file should contain the following:

```yaml
name: prompt_name                # Unique identifier for calling this prompt
description: prompt description  # Description of the prompt's functionality
arguments:                       # Parameter list (optional)
  - name: arg_name               # Parameter name
    description: arg description # Parameter description
    required: true/false         # Whether required
messages:                        # Prompt message list
  - role: user/assistant         # Message role
    content:
      type: text                 # Content type
      text: |                    # Text content, can include parameter placeholders {{arg_name}}
        Your prompt text here...
```

After adding new files, the server will automatically load them on next startup, or you can use the `reload_prompts` tool to reload all prompts.

## Usage Examples

### Using Code Review Tool in Cursor or Windsurf

```json
{
  "name": "code_review",
  "arguments": {
    "language": "javascript",
    "code": "function add(a, b) { return a + b; }"
  }
}
```

### Using API Documentation Generation Tool in Cursor or Windsurf

```json
{
  "name": "api_documentation",
  "arguments": {
    "language": "python",
    "code": "def process_data(data, options=None):\n    # Process data\n    return result",
    "format": "markdown"
  }
}
```

## Tool APIs

The server provides the following management tools:

- `reload_prompts`: Reload all predefined prompts
- `get_prompt_names`: Get all available prompt names

Additionally, all prompt templates defined in the `src/prompts` directory are provided as tools to clients.

## Editor Integration

### Cursor

In Cursor, you need to edit the MCP configuration file:

1. Find or create Cursor's MCP configuration file (usually located in the `~/.cursor/` directory)
2. Add the following content:

```json
{
  "servers": [
    {
      "name": "Prompt Server",
      "command": ["node", "/path/to/prompt-server/src/index.js"],
      "transport": "stdio",
      "initialization_options": {}
    }
  ]
}
```

Make sure to replace `/path/to/prompt-server` with your actual project path.

3. Save the configuration and restart the editor
4. You should now be able to see all available prompt tools in the tools panel

### Windsurf

In Windsurf, access the MCP configuration through:

1. Navigate to Windsurf - Settings > Advanced Settings, or
2. Use Command Palette > Open Windsurf Settings Page
3. Scroll down to the Cascade section where you'll find options to add a new server
4. Click the "Add Server" button, then select "Add custom server +"
5. Alternatively, you can directly edit the `~/.codeium/windsurf/mcp_config.json` file, adding the following content:

```json
{
  "mcpServers": {
    "prompt-server": {
      "command": "node",
      "args": [
        "/path/to/prompt-server/src/index.js"
      ],
      "transport": "stdio"
    }
  }
}
```

Make sure to replace `/path/to/prompt-server` with your actual project path.

6. After adding the server, click the refresh button
7. You should now be able to see all available prompt tools in the tools panel

## Extension Suggestions

1. Add more professional domain-specific prompt templates
2. Implement prompt version control
3. Add prompt categorization and tagging
4. Implement prompt usage statistics and analysis
5. Add user feedback mechanism
