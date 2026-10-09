# litellm

Create new user:
```bash
MASTER=<LITELLM_MASTER_KEY>          # from litellm-secrets / the scratchpad credentials file
B=https://litellm.nvidia-droplet.shubhamtatvamasi.com

curl -s $B/user/new \
  -H "Authorization: Bearer $MASTER" -H 'Content-Type: application/json' \
  -d '{"user_id":"shubham","user_email":"shubham.tatvamasi@portainer.io","user_role":"proxy_admin","auto_create_key":false,"send_invite_email":false}'
```

Create an invitation for the user:
```
curl -s $B/invitation/new \
  -H "Authorization: Bearer $MASTER" -H 'Content-Type: application/json' \
  -d '{"user_id":"shubham"}'
# → {"id":"b01ce98c-154a-4c51-aeee-209e3a8a223f","user_id":"shubham","is_accepted":false,"expires_at":"2026-10-16T…"}
```


https://litellm.nvidia-droplet.shubhamtatvamasi.com/ui?invitation_id=<id>


Get Lite LLM secrets:
```bash
kgse litellm-secrets -o yaml | \
  yq '.data |= with_entries(.value |= @base64d)'
```

