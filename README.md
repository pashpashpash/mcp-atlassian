# MCP Atlassian

Model Context Protocol (MCP) server for Atlassian Cloud products (Confluence and Jira). This integration is designed specifically for Atlassian Cloud instances and does not support Atlassian Server or Data Center deployments.

### Feature Demo
![Demo](https://github.com/user-attachments/assets/995d96a8-4cf3-4a03-abe1-a9f6aea27ac0)

### Resources

- `confluence://{space_key}`: Access Confluence spaces and pages
- `confluence://{space_key}/pages/{title}`: Access specific Confluence pages
- `jira://{project_key}`: Access Jira project and its issues
- `jira://{project_key}/issues/{issue_key}`: Access specific Jira issues

### Tools

#### Confluence Tools

1. `confluence_search`
   - Search Confluence content using CQL
   - Inputs:
     - `query` (string): CQL query string
     - `limit` (number, optional): Results limit (1-50, default: 10)
   - Returns: Array of search results with page_id, title, space, url, last_modified, type, and excerpt

2. `confluence_get_page`
   - Get content of a specific Confluence page
   - Inputs:
     - `page_id` (string): Confluence page ID
     - `include_metadata` (boolean, optional): Include page metadata (default: true)
   - Returns: Page content and optional metadata

3. `confluence_get_comments`
   - Get comments for a specific Confluence page
   - Input: 
     - `page_id` (string): Confluence page ID
   - Returns: Array of comments with author, creation date, and content

#### Jira Tools

1. `jira_get_issue`
   - Get details of a specific Jira issue
   - Inputs:
     - `issue_key` (string): Jira issue key (e.g., 'PROJ-123')
     - `expand` (string, optional): Fields to expand
   - Returns: Issue details including content and metadata

2. `jira_search`
   - Search Jira issues using JQL
   - Inputs:
     - `jql` (string): JQL query string
     - `fields` (string, optional): Comma-separated fields (default: "*all")
     - `limit` (number, optional): Results limit (1-50, default: 10)
   - Returns: Array of matching issues with metadata

3. `jira_get_project_issues`
   - Get all issues for a specific Jira project
   - Inputs:
     - `project_key` (string): Project key
     - `limit` (number, optional): Results limit (1-50, default: 10)
   - Returns: Array of project issues with metadata

## Installation

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/pashpashpash/mcp-atlassian.git
   cd mcp-atlassian
   ```

2. **Install Dependencies**:
   ```bash
   npm install
   ```

3. **Build the Project**:
   ```bash
   npm run build
   ```

## Configuration

The MCP Atlassian integration supports using either Confluence, Jira, or both services. You only need to provide the environment variables for the service(s) you want to use.

### Usage with Claude Desktop

1. Get API tokens from: https://id.atlassian.com/manage-profile/security/api-tokens

2. Add to your `claude_desktop_config.json` with only the services you need:

For Confluence only:
```json
{
  "mcpServers": {
    "mcp-atlassian": {
      "command": "node",
      "args": ["path/to/build/index.js"],
      "env": {
        "CONFLUENCE_URL": "https://your-domain.atlassian.net/wiki",
        "CONFLUENCE_USERNAME": "your.email@domain.com",
        "CONFLUENCE_API_TOKEN": "your_api_token"
      }
    }
  }
}
```

For Jira only:
```json
{
  "mcpServers": {
    "mcp-atlassian": {
      "command": "node",
      "args": ["path/to/build/index.js"],
      "env": {
        "JIRA_URL": "https://your-domain.atlassian.net",
        "JIRA_USERNAME": "your.email@domain.com",
        "JIRA_API_TOKEN": "your_api_token"
      }
    }
  }
}
```

For both services:
```json
{
  "mcpServers": {
    "mcp-atlassian": {
      "command": "node",
      "args": ["path/to/build/index.js"],
      "env": {
        "CONFLUENCE_URL": "https://your-domain.atlassian.net/wiki",
        "CONFLUENCE_USERNAME": "your.email@domain.com",
        "CONFLUENCE_API_TOKEN": "your_api_token",
        "JIRA_URL": "https://your-domain.atlassian.net",
        "JIRA_USERNAME": "your.email@domain.com",
        "JIRA_API_TOKEN": "your_api_token"
      }
    }
  }
}
```

## Debugging

You can use the MCP inspector to debug the server:

```bash
cd path/to/mcp-atlassian
npx @modelcontextprotocol/inspector node build/index.js
```

View logs with:
```bash
tail -n 20 -f ~/Library/Logs/Claude/mcp*.log
```

## Security

- Never share API tokens
- Keep .env files secure and private
- See [SECURITY.md](SECURITY.md) for best practices

## License

Licensed under MIT - see [LICENSE](LICENSE) file. This is not an official Atlassian product.

---
Note: This is a fork of the [original mcp-atlassian repository](https://github.com/sooperset/mcp-atlassian).
