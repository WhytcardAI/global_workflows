# MCP Analysis Workflow

## 1. Discovery Phase

- [ ] List all available MCP servers
- [ ] Check MCP configuration files
- [ ] Verify MCP endpoints accessibility

## 2. Status Check

- [ ] Ping each MCP server
- [ ] Verify authentication tokens
- [ ] Check connection timeouts
- [ ] Validate SSL certificates

## 3. Capability Analysis

- [ ] List available tools per MCP
- [ ] Document supported methods
- [ ] Check rate limits
- [ ] Verify permissions

## 4. Debug Checklist

- [ ] Enable verbose logging
- [ ] Check MCP logs location
- [ ] Verify environment variables
- [ ] Test with minimal payload
- [ ] Check network connectivity
- [ ] Validate JSON schema

## 5. Common Issues & Solutions

| Issue              | Diagnostic Command     | Solution            |
| ------------------ | ---------------------- | ------------------- |
| Connection refused | `ping mcp-server`      | Check firewall/port |
| Auth failed        | Check token expiry     | Refresh credentials |
| Timeout            | Increase timeout value | Check server load   |
| Invalid response   | Validate JSON          | Check API version   |

## 6. Debug Commands

```bash
# Check MCP status
curl -X GET http://localhost:PORT/health

# List available tools
curl -X POST http://localhost:PORT/tools/list

# Test tool execution
curl -X POST http://localhost:PORT/tools/call -d '{"name":"tool_name","arguments":{}}'

# View logs
tail -f ~/.codeium/logs/mcp.log
```

## 7. Reporting Template

```json
{
  "mcp_name": "",
  "status": "active|inactive|error",
  "tools_count": 0,
  "last_check": "",
  "errors": [],
  "response_time_ms": 0
}
```
