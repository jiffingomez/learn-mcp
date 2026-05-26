# Tools

## Write a tool to read a doc
```python
@mcp.tool(
    name="read_doc_contents",
    description="This tool reads the contents of a file and return it as string."
)
def read_doc_contents(doc_id: str = Field(description="The document ID.")):
    if doc_id not in docs:
        raise ValueError(f"The document ID {doc_id} does not exist.")
    return docs[doc_id]
```
Also import the `Field` class from `pydantic` at the top of the file:
```python
from pydantic import Field
```

## Write a tool to edit a doc
```python
@mcp.tool(
    name="edit_doc_contents",
    description="This tool edits the contents of a file with a string."
)
def edit_doc_contents(
        doc_id: str = Field(description="The document ID."),
        old_str:str = Field(description="The text to replace. Must match exactly, including white spaces."),
        new_str:str = Field(description="The new text to insert in place of the old text.")
):
    if doc_id not in docs:
        raise ValueError(f"The document ID {doc_id} does not exist.")
    docs[doc_id] = docs[doc_id].replace(old_str, new_str)
```

## to run the inspector
```commandline
mcp dev mcp_server.py
```

# Resources

## Write a resource to return all doc id's
```python
@mcp.resource(
    uri="docs://documents",
    mime_type="application/json"
)
def list_docs()-> list[str]:
    return list(docs.keys())
```

## Write a resource to return the contents of a particular doc
```python
@mcp.resource(
    uri="docs://documents/{doc_id}",
    mime_type="plain/text"
)
def fetch_doc(doc_id: str):
    if doc_id not in docs:
        raise ValueError(f"The document ID {doc_id} does not exist.")
    return docs[doc_id]
```

# Prompt

## Write a prompt to rewrite a doc in markdown format
```python
@mcp.prompt(
    name="format",
    description="This tool formats the contents of a file in markdown format.",
)
def format_doc(doc_id: str = Field(description="The document ID."))->list[base.Message]:
    if doc_id not in docs:
        raise ValueError(f"The document ID {doc_id} does not exist.")

    prompt = f"""
    Your goal is to reformat a document to be written with markdown syntax.

    The id of the document you need to reformat is:
    <document_id>
    {doc_id}
    </document_id>

    Add in headers, bullet points, tables, etc as necessary. Feel free to add in structure.
    Use the 'edit_document' tool to edit the document. After the document has been reformatted...
    """
    return [base.UserMessage(prompt)]
```

## also import the `base` module at the top of the file:
```python
from mcp.server.fastmcp.prompts import base
```