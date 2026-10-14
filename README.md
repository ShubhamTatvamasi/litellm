# litellm

### Setup new user:

Setup domain and key:
```bash
MASTER=$(kubectl -n litellm get secret litellm-secrets -o jsonpath='{.data.masterkey}' | base64 -d)
B=https://litellm.droplet.shubhamtatvamasi.com
```

Create the user (skip if it already exists; you get a 409)
```bash
curl -s "$B/user/new" \
  -H "Authorization: Bearer $MASTER" -H 'Content-Type: application/json' \
  -d '{"user_id":"shubham","user_email":"shubham.tatvamasi@portainer.io","user_role":"proxy_admin","auto_create_key":false,"send_invite_email":false}'
```

Make a 10-minute key owned by that user (the master key's own user doesn't exist, so it can't create invitations)
```bash
TMP=$(curl -s "$B/key/generate" \
  -H "Authorization: Bearer $MASTER" -H 'Content-Type: application/json' \
  -d '{"user_id":"shubham","duration":"10m","key_alias":"invite-bootstrap"}' | jq -r .key)
```

Create the invitation with that key and print the link
```bash
ID=$(curl -s "$B/invitation/new" \
  -H "Authorization: Bearer $TMP" -H 'Content-Type: application/json' \
  -d '{"user_id":"shubham"}' | jq -r .id)
echo "$B/ui?invitation_id=$ID"
```

Delete the temporary key
```bash
curl -s "$B/key/delete" \
  -H "Authorization: Bearer $MASTER" -H 'Content-Type: application/json' \
  -d "{\"keys\":[\"$TMP\"]}"
```

https://litellm.nvidia-dynamo.shubhamtatvamasi.com/ui?invitation_id=<id>

Get Lite LLM secrets:
```bash
kubectl -n litellm get secret litellm-secrets -o yaml | \
  yq '.data |= with_entries(.value |= @base64d)'
```

