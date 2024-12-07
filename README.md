# EasyRemote

EasyRemote is a Python framework that makes it easy to execute functions remotely through a public VPS server. It's designed for scenarios where you want to expose local compute resources (like AI models) through a public endpoint.

## Features

- Simple decorator-based API
- Support for sync/async functions
- Support for streaming/generator functions
- Automatic reconnection handling
- Integration with Flask/FastAPI
- Type safety and IDE support

## Installation

```bash
pip install easyremote
```

## Quick Start

### On your local compute node:

```python
from easyremote import ComputeNode

node = ComputeNode("your-vps:8080")

@node.register
def process_data(data: dict) -> dict:
    return {"result": data["value"] * 2}

if __name__ == "__main__":
    node.serve()
```

### On your VPS (with Flask):

```python
from flask import Flask
from easyremote import Server, remote

app = Flask(__name__)
server = Server(port=8080)

@remote
def process_data(data: dict) -> dict:
    pass  # This will be executed on compute node

@app.route('/process', methods=['POST'])
def process():
    result = process_data(request.json)
    return jsonify(result)

if __name__ == '__main__':
    server.start_background()
    app.run()
```

## Advanced Features

### Async Functions

```python
@node.register
async def process_video(video_data: bytes) -> bytes:
    result = await async_process(video_data)
    return result
```

### Streaming

```python
@node.register
def stream_process(data: bytes):
    for chunk in process_chunks(data):
        yield chunk
```

### Node Selection

```python
@remote(node_id="gpu-1")
def train_model(data: dict):
    pass
```

## Documentation

For full documentation, visit [docs link].

## License

MIT License
