# litellm

Get Lite LLM secrets:
```bash
kgse litellm-secrets -o yaml | \
  yq '.data |= with_entries(.value |= @base64d)'
```

