# Kong AI Gateway Observability

Sample Stack for Kong AI Gateway Observability Tools.

## Starting the Stack

Boot the stack with Docker Compose:

```sh
docker compose up
```

or sometimes:

```sh
docker-compose up
```

## Usage

Navigate to http://localhost:5601/app/dashboards#/view/aa8e4cb0-9566-11ef-beb2-c361d8db17a8
to see the Kibana dashboard for the Kong AI Gateway metrics.

Now you need to make some AI traffic!

The best way to utilise this demo is with an inference library such as:

* [LangChain](https://python.langchain.com/docs/tutorials/llm_chain/)

### LangChain

Try executing some Python function tool using LangChain:

```python
from langchain_openai import ChatOpenAI
from langchain_core.tools import tool

kong_url = "http://127.0.0.1:8000"
kong_route = "gpt4o"

@tool
def multiply(first_int: int, second_int: int) -> int:
    """Multiply two integers together."""
    return first_int * second_int

llm = ChatOpenAI(
    base_url=f'{kong_url}/{kong_route}',
    api_key="department-1-api-key"
)

llm_with_tools = llm.bind_tools([multiply])

chain = llm_with_tools | (lambda x: x.tool_calls[0]["args"]) | multiply
response = chain.invoke("What's four times 23?")
print(f"$ ToolsAnswer:> {response}")
```
