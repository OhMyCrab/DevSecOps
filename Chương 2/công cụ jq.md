** Hiển thị JSON
 
 `jq '.'`: lấy toàn bộ dữ liệu JSON đầu vào và hiển thị lại dưới dạng được định dạng (pretty-print) để dễ đọc hơn.

- `echo '{"cloudprovider":{"name":"AWS","url":"www.aws.com"}} ' | jq '.'`

- `curl https://randomuser.me/api/ | jq '.'`

- `jq '.' learnjq.json`

`jq '.field'`: dùng để truy cập một thuộc tính trong JSON.

- `jq '.details'` - lấy trường details.

`jq '.[]'`: dùng để duyệt từng phần tử của một mảng (array).

** 2 hàm jq hay dùng

`jq 'length'`: lệnh dùng để đếm: 

- Số phần tử trong mảng

- Số thuộc tính trong đối tượng

- Độ dài chuỗi

`jq 'select()'`: lệnh dùng để lọc dữ liệu theo điều kiện.
