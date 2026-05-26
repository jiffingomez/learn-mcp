# Tools

## Return a list of tools defined by the MCP server
```python
results = await self.session().list_tools()
return results.tools
```

## Call a particular tool and return the result
```python
results = await self.session().call_tool(tool_name, tool_input)
return results
```

## testing code:
```python
# For testing
async def main():
    async with MCPClient(
        # If using Python without UV, update command to 'python' and remove "run" from args.
        command="uv",
        args=["run", "mcp_server.py"],
    ) as _client:
        results = await _client.list_tools()
        print(results)
```

## Run the client
```python
uv run mcp_client.py 
```

# Resources

## Read a resource, parse the contents and return it
```python
result = await self.session().read_resource(AnyUrl(uri))
resource = result.contents[0]

if isinstance(resource, types.TextResourceContents):
    if resource.mimeType == "application/json":
        return json.loads(resource.text)
    return resource.text
```

Also import the `json` & `pydantic` module at the top of the file:
```python
import json
from pydantic import AnyUrl
```

# Prompt

## Return a list of prompts defined by the MCP server
```python
results = await self.session().list_prompts()
return results.prompts
```

## Get a particular prompt defined by the MCP server
```python
results = await self.session().get_prompt(prompt_name, args)
return results.messages
```