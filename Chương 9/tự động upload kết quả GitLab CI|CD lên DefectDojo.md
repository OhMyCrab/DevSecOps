Trước khi push file upload-results.py lên repo, chúng ta cần thiết lập git command.

`git config --global user.email "student@practical-devsecops.com"`

`git config --global user.name "student"`

`git clone git@gitlab-ce-8t8ba53v:root/django-nv.git`

`cd django-nv`

`curl https://gitlab.practical-devsecops.training/-/snippets/28/raw -o upload-results.py`

`git add upload-results.py`

`git commit -m "Add upload-results.py file"`

`git push origin main`

Nếu gặp mã lỗi 500, cần kiểm tra xem môi trường đã được thiết lập đúng cách chưa. Có thể kiểm tra biến cụ thể trong mục `Engagements` > `Environments`.

