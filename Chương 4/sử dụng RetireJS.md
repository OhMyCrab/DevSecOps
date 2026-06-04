tải RetireJS

```
mkdir -p /etc/apt/keyrings
curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
NODE_MAJOR=20
echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" | tee /etc/apt/sources.list.d/nodesource.list
apt update
```

```
apt install nodejs -y
```

```
npm install -g retire@5.0.0
```

`retire --outputformat json --outputpath no-npm-install-retire-output.json`

- `--outputpath`: chỉ định đường dẫn đến tệp đầu ra.

- `--outputformat`: chỉ định định dạng đầu ra. Ở đây là định dạng JSON.

`npm install`

`retire --severity medium --outputformat json --outputpath retire_output.json`: lọc theo medium

`retire --severity critical --outputformat json --outputpath retire_output.json`: lọc theo critical

`cat retire_output.json | jq .`

`echo $?`
