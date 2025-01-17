# MicroRabbit

MicroRabbit is a lightweight, asynchronous Python framework for working with RabbitMQ. It simplifies the process of setting up RabbitMQ consumers and publishers, making it easy to build microservices and distributed systems.

## Features

- Asynchronous message handling using `asyncio`
- Simple decorator-based message routing
- Plugin system for modular code organization
- Easy-to-use client configuration
- Built-in logging support

## Installation

```bash
pip install git+https://@github.com/TonnoBelloSnello/microrabbit.git
```

## Quick Start
Here's a simple example of how to use MicroRabbit:

```python
import asyncio
import logging
from microrabbit import Client

client = Client(
    host="amqp://guest:guest@localhost/",
    plugins="./plugins"
)

log = logging.getLogger(__file__)
logging.basicConfig(level=logging.INFO)

@Client.on_message("queue_name")
async def test(data: dict) -> dict:
    log.info(f"Received message {data}")
    return {"connected": True}

@client.on_ready
async def on_ready():
    log.info("[*] Waiting for messages. To exit press CTRL+C")

if __name__ == "__main__":
    asyncio.run(client.run())
```

## Usage
### Client Configuration
Create a `Client` instance with the following parameters:

- `host`: RabbitMQ server URL
- `plugins`: Path to the plugins folder (optional)
```python
from microrabbit import Client

client = Client(
    host="amqp://guest:guest@localhost/",
    plugins="./plugins"
)
```

### Message Handling
Use the `@Client.on_message` decorator to define a message handler. The decorator takes the queue name as an argument.
```python
from microrabbit import Client

@Client.on_message("queue_name")
async def handler(data: dict):
    # Process the message
    return response_data # Serializeable data
```


### Ready Event
Use the `@client.on_ready` decorator to define a function that runs when the client is ready:
```python
from microrabbit import Client

client = Client(
    host="amqp://guest:guest@localhost/",
    plugins="./plugins"
)

@client.on_ready
async def on_ready():
    print("Client is ready")
```

### Running the Client
Run the client using `asyncio.run(client.run())`:
```python
import asyncio
from microrabbit import Client

client = Client(
    host="amqp://guest:guest@localhost/",
    plugins="./plugins"
)

if __name__ == "__main__":
    asyncio.run(client.run())
```

## Plugins
MicroRabbit supports a plugin system. 
Place your plugin files in the specified plugins folder, and they will be automatically 
loaded by the client.

### Plugin Example
```python
# ./plugins/test_plugin.py
from microrabbit import Client

@Client.on_message("test_queue")
async def test_handler(data: dict):
    print(f"Received message: {data}")
    return {"status": "ok"}
```

## Contributing
Contributions are welcome! For feature requests, bug reports, or questions, please open an issue. 
If you would like to contribute code, please submit a pull request.

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.