---
title: API Keys
parent: Secure Chain
nav_order: 6
---

# API Keys

When you log in to the Secure Chain website, you will see a button in the top right-hand corner to manage API Keys, which can be used by your own services or codebases as follows.

## API Key Authentication
Used by microservices for service-to-service communication:

```bash
# Create an API Key (requires login into Secure Chain web)
# Then use API Key in other microservices
curl "https://securechain.dev/api/some_endpoint" -H "X-API-Key: sk_your_api_key_here"
```

```python
import requests

# Use API Key in Python
api_key = "sk_your_api_key_here"
headers = {"X-API-Key": api_key}

response = requests.get("https://securechain.dev/api/some_endpoint", headers=headers)
print(response.json())
```
