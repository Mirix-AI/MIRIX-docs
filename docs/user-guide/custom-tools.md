# Custom Tools

Create and manage custom tools in MIRIX to extend your agent's capabilities with your own Python functions. They will be run in a sandbox environment.

## Basic Tool Creation

### Simple Function Tool

```python
from mirix import Mirix

# Initialize memory agent
memory_agent = Mirix(api_key="your-google-api-key")

# Create a simple calculation tool
result = memory_agent.insert_tool(
    name="calculate_sum",
    source_code="def calculate_sum(a: int, b: int) -> int:\n    return a + b",
    description="Calculate the sum of two numbers",
    args_info={"a": "First number", "b": "Second number"},
    returns_info="The sum of a and b",
    tags=["math", "utility"] # "tags" is optional and not important
)
```

### API Integration Tool

```python
# Create a weather API tool
weather_tool_code = """def get_weather(city: str, api_key: str) -> str:
    import requests
    import json
    
    url = f"http://api.openweathermap.org/data/2.5/weather"
    params = {
        'q': city,
        'appid': api_key,
        'units': 'metric'
    }
    
    try:
        response = requests.get(url, params=params)
        response.raise_for_status()
        data = response.json()
        
        temp = data['main']['temp']
        description = data['weather'][0]['description']
        humidity = data['main']['humidity']
        
        return f"Weather in {city}: {temp}°C, {description}, humidity: {humidity}%"
    except Exception as e:
        return f"Error getting weather data: {str(e)}"
"""

result = memory_agent.insert_tool(
    name="get_weather",
    source_code=weather_tool_code,
    description="Get current weather information for a specified city",
    args_info={
        "city": "Name of the city to get weather for",
        "api_key": "OpenWeatherMap API key"
    },
    returns_info="Weather information string with temperature, description, and humidity",
    tags=["weather", "api", "external"] # "tags" is optional and not important
)
```

## Tool Management Parameters

### Method Signature

```python
def insert_tool(
    self, 
    name: str, 
    source_code: str, 
    description: str, 
    args_info: Optional[Dict[str, str]] = None, 
    returns_info: Optional[str] = None, 
    tags: Optional[List[str]] = None, 
    apply_to_agents: Union[List[str], str] = 'all'
) -> Dict[str, Any]
```

#### Parameter Details

| Parameter | Type | Description | Example |
|-----------|------|-------------|---------|
| `name` | `str` | Function name identifier | `"calculate_sum"` |
| `source_code` | `str` | Python function code (without docstring) | `"def func(x): return x * 2"` |
| `description` | `str` | What the tool does | `"Calculate the sum of two numbers"` |
| `args_info` | `Dict[str, str]` | Argument descriptions | `{"a": "First number", "b": "Second number"}` |
| `returns_info` | `str` | Return value description | `"The sum of a and b"` |
| `tags` | `List[str]` | Categorization tags | `["math", "utility"]` |
| `apply_to_agents` | `Union[List[str], str]` | Which agents get the tool | `'all'` or `["chat_agent", "semantic_memory_agent"]` |

### Available Agent Names

When specifying `apply_to_agents`, you can use any of these agent names:

- `reflexion_agent` - Handles self-reflection and metacognitive processes
- `core_memory_agent` - Manages core persistent memories
- `episodic_memory_agent` - Handles episodic and experiential memories
- `procedural_memory_agent` - Manages procedural knowledge and skills
- `semantic_memory_agent` - Handles semantic knowledge and facts
- `meta_memory_agent` - Manages meta-information about memories
- `resource_memory_agent` - Handles resource and file associations
- `knowledge_vault_agent` - Manages structured knowledge storage
- `chat_agent` - Handles conversational interactions
- `background_agent` - Processes background tasks and operations
