** Thiết lập Git ban đầu

`git config`: lệnh cấu hình Git

- `git config --global user.email "student@practical-devsecops.com"` - cấu hình địa chỉ email sẽ được gắn với các commit Git.

- `git config --global user.name "student"` - cấu hình tên tác giả mặc định cho các commit Git.

- `git config --list` - xem toàn bộ cấu hình Git hiện tại.

`git clone`: lệnh tải (clone) repository từ GitLab/GitHub về máy local.

- `git clone git@gitlab-ce-8t8ba53v:root/django-nv.git`

`git status`: lệnh xem trạng thái hiện tại của repository.

`git add`: lệnh đưa file hoặc thay đổi vào staging area để chuẩn bị commit.

- `git add myfile README.md`

`git commit`: lệnh lưu thay đổi từ staging area vào local repository.

- `git commit -m "Fix command injection"` - `-m` dùng để chỉ định commit message.

`git push`: lệnh đẩy commit từ local repository lên remote repository.

`git pull`: lệnh tải và hợp nhất thay đổi từ remote repository về local repository.

`git reset`: lệnh bỏ file khỏi staging area.

- `git reset HEAD myfile` - file vẫn tồn tại và các thay đổi vẫn còn, chỉ bị bỏ khỏi staging area.

** Làm việc với Branch

`git branch`: lệnh quản lý branch (tạo, liệt kê, xóa branch).

- `git branch new-branch` - tạo branch mới tên `new-branch`.

`git branch --list`: lệnh liệt kê các branch.

`git branch --show-current`: lệnh xem branch hiện tại.

`git branch -D branch-name`: lệnh xóa branch local.

`git checkout`: lệnh chuyển branch hoặc khôi phục file.

- `git checkout new-branch`
  Chuyển sang branch đã tồn tại.

- `git checkout -b new-branch` - tạo branch mới và chuyển sang branch đó.

`git switch`: lệnh chuyển branch (cách mới được Git khuyến nghị).

- `git switch new-branch` - chuyển sang branch đã tồn tại.

- `git switch -c new-branch` - tạo branch mới và chuyển sang branch đó.

`git switch main`: lệnh chuyển về branch main.

- `git push origin new-branch` - đẩy branch local `new-branch` lên remote repository.

- `git pull origin main` - kéo cập nhật từ remote branch `main` về local branch hiện tại.
