<img width="965" height="1024" alt="image" src="https://github.com/user-attachments/assets/f6a09257-efe2-4f4b-be31-11f1b8d0ce81" />

```
export API_KEY=$(curl -s -XPOST \
  -H 'content-type: application/json' \
  https://dojo-8t8ba53v.lab.practical-devsecops.training/api/v2/api-token-auth/ \
  -d '{"username": "root", "password": "pdso-training"}' | jq -r '.token')
```

`echo $API_KEY`

<img width="1024" height="571" alt="image" src="https://github.com/user-attachments/assets/af384bfb-9e52-476b-8ba0-c66ce4476249" />
