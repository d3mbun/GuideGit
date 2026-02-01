# Hướng dẫn sử dụng Git & GitHub

Lưu lại các hướng dẫn lệnh Git và quy trình làm việc với GitHub.

---

## 📑 Mục lục

1. [Cài đặt và cấu hình ban đầu](#1-cài-đặt-và-cấu-hình-ban-đầu)
   - [1.1. Cài đặt Git](#11-cài-đặt-git)
   - [1.2. Cấu hình thông tin cá nhân](#12-cấu-hình-thông-tin-cá-nhân)
   - [1.3. Cấu hình SSH Key](#13-cấu-hình-ssh-key)
2. [Khởi tạo Repository mới](#2-khởi-tạo-repository-mới)
3. [Push code lên GitHub](#3-push-code-lên-github)
4. [Clone Repository về máy](#4-clone-repository-về-máy)
5. [Pull code từ Repository](#5-pull-code-từ-repository)
6. [Quy trình làm việc hàng ngày](#6-quy-trình-làm-việc-hàng-ngày)
   - [6.1. Bắt đầu phiên làm việc mới](#61-bắt-đầu-phiên-làm-việc-mới)
   - [6.2. Trong quá trình làm việc](#62-trong-quá-trình-làm-việc)
   - [6.3. Kết thúc phiên làm việc](#63-kết-thúc-phiên-làm-việc)
7. [Làm việc với Git LFS (Large File Storage)](#7-làm-việc-với-git-lfs-large-file-storage)
   - [7.1. Cài đặt và cấu hình LFS](#71-cài-đặt-và-cấu-hình-lfs)
   - [7.2. Push file lớn với LFS](#72-push-file-lớn-với-lfs)
   - [7.3. Pull file LFS](#73-pull-file-lfs)
8. [Cấu hình .gitignore](#8-cấu-hình-gitignore)
   - [8.1. Tạo file .gitignore](#81-tạo-file-gitignore)
   - [8.2. Cú pháp .gitignore](#82-cú-pháp-gitignore)
   - [8.3. Các mẫu .gitignore thường dùng](#83-các-mẫu-gitignore-thường-dùng)
9. [Các lệnh Git hữu ích khác](#9-các-lệnh-git-hữu-ích-khác)
10. [Xử lý sự cố thường gặp](#10-xử-lý-sự-cố-thường-gặp)
11. [Xử lý tình huống up sai – Xóa commit gần nhất](#11-xử-lý-tình-huống-up-sai--xóa-commit-gần-nhất)
---

## 1. Cài đặt và cấu hình ban đầu

### 1.1. Cài đặt Git

**Windows:**
- Tải Git từ: <https://git-scm.com/install/windows>
- Chạy file cài đặt và làm theo hướng dẫn


**Linux (Ubuntu/Debian):**
```bash
sudo apt-get update
sudo apt-get install git
```

Kiểm tra phiên bản đã cài đặt:
```bash
git --version
```

### 1.2. Cấu hình thông tin cá nhân

Thiết lập tên và email (sẽ hiển thị trong các commit):

```bash
git config --global user.name "d3mbun"
git config --global user.email d3mbun@gmail.com
```

Cấu hình line ending (khuyến nghị - Git sẽ không tự ý đổi line ending. Nhưng để repo nhất quán, đặc biệt khi người khác clone về máy Windows, thì **.gitattributes** mới là thứ “bắt buộc chuẩn hóa” trong repo):
```bash
git config --global core.autocrlf false
```

Kiểm tra cấu hình:
```bash
git config --list
```

### 1.3. Cấu hình SSH Key

**Bước 1:** Kiểm tra SSH key đã tồn tại chưa
```bash
ls -al ~/.ssh
```

**Bước 2:** Tạo SSH key mới (nếu chưa có)
```bash
ssh-keygen -t rsa -b 4096 -C "d3mbun@gmail.com"
```
- Nhấn Enter để chấp nhận vị trí lưu mặc định
- Nhập passphrase (hoặc để trống nếu không muốn)

**Bước 3:** Kích hoạt ssh-agent và thêm key
```bash
# Kích hoạt ssh-agent
eval "$(ssh-agent -s)"

# Thêm SSH private key vào ssh-agent
ssh-add ~/.ssh/id_rsa
```

**Bước 4:** Lấy SSH public key
```bash
cat ~/.ssh/id_rsa.pub
```

**Bước 5:** Thêm SSH key vào GitHub
1. Copy nội dung SSH key vừa hiển thị
2. Vào GitHub → Đăng nhập theo email d3mbun@gmail.com → Settings → SSH and GPG keys
3. Click "New SSH key"
4. Dán key vào và lưu

**Bước 6:** Kiểm tra kết nối
```bash
ssh -T git@github.com
```

[⬆️ Quay lại mục lục](#-mục-lục)

---

## 2. Khởi tạo Repository mới

### Tạo repository trên GitHub trước
1. Vào GitHub và tạo repository mới
2. Có thể chọn thêm README, .gitignore, .gitattributes hoặc license

### Khởi tạo Git trong project local

Chuột phải thư mục project → **Git Bash Here** :

```bash
# Khởi tạo Git repository
git init

# Kết nối với remote repository
git remote add origin git@github.com:d3mbun/TLBBWinnerR_Server.git
git remote add origin git@github.com:d3mbun/TLBBWinnerR_Client.git

# Kiểm tra remote đã được thêm
git remote -v
```

### Tạo file .gitkeep cho thư mục rỗng

Nếu có thư mục rỗng cần push lên Git:
```bash
touch folder_name/.gitkeep
```

[⬆️ Quay lại mục lục](#-mục-lục)

---

## 3. Push code lên GitHub

### Quy trình push lần đầu

```bash
# Thêm tất cả file vào staging area
git add .

# Hoặc thêm file cụ thể
git add file1.txt file2.lua

# Kiểm tra trạng thái
git status

# Commit với message mô tả
git commit -m "Initial commit"

# Push lên branch main/master
git push origin master 
# hoặc
git push origin main

```

### Push các lần sau

```bash
# Thêm file đã thay đổi
git add .

# Commit
git commit -m "Mô tả ngắn gọn về thay đổi"

# Push
git push origin master
```

### Push branch mới

```bash
# Tạo và chuyển sang branch mới
git checkout -b feature-new-branch

# Push branch mới lên remote
git push -u origin feature-new-branch
```

[⬆️ Quay lại mục lục](#-mục-lục)

---

## 4. Clone Repository về máy

### Clone với HTTPS

```bash
git clone https://github.com/username/repository.git
```

### Clone với SSH (khuyến nghị)

```bash
git clone git@github.com:username/repository.git
```

### Clone vào thư mục cụ thể

```bash
git clone git@github.com:username/repository.git my-folder-name
```

### Clone một branch cụ thể

```bash
git clone -b branch-name git@github.com:username/repository.git
```

[⬆️ Quay lại mục lục](#-mục-lục)

---

## 5. Pull code từ Repository


### Pull khi chưa có repository local

Mở Git Bash tại thư mục muốn tải code:

```bash
# Khởi tạo Git
git init

# Thêm remote
git remote add origin git@github.com:d3mbun/TLBBWinnerR_Server.git

# Pull code về
git pull origin master
```
### Pull từ repository có sẵn

```bash
# Pull từ branch hiện tại
git pull

# Pull từ branch cụ thể
git pull origin master
```

### Pull và rebase

```bash
git pull --rebase origin master
```

[⬆️ Quay lại mục lục](#-mục-lục)

---

## 6. Quy trình làm việc hàng ngày

### 6.1. Bắt đầu phiên làm việc mới

```bash
# 1. Mở Git Bash here tại thư mục project

# 2. Kiểm tra branch hiện tại
git branch

# 3. Pull code mới nhất từ remote (Luôn pull trước khi làm bất cứ thay đổi nào)
git pull origin master

# 4. Kiểm tra trạng thái
git status
# Kết quả mong muốn:
On branch master
Your branch is up to date with 'origin/master'.
nothing to commit, working tree clean
# Nếu thấy:
- modified: → có file đã bị chỉnh sửa trên máy
- untracked files: → có file mới chưa add
Thì KHÔNG nên code tiếp nếu chưa biết rõ các thay đổi này từ đâu

# 5. Tạo branch mới cho tính năng (nếu cần)
git checkout -b feature/ten-tinh-nang
```

### 6.2. Trong quá trình làm việc

```bash
# Kiểm tra thay đổi
git status

# Xem chi tiết thay đổi
git diff

# Xem lịch sử commit
git log
git log --oneline
git log --graph --oneline --all

# Stash thay đổi tạm thời (nếu cần chuyển branch)
git stash
git stash pop  # Lấy lại thay đổi
```

### 6.3. Kết thúc phiên làm việc

```bash
# 1. Thêm file vào staging
git add .

# 2. Commit với message rõ ràng
git commit -m "feat: Thêm chức năng đăng nhập"

# 3. Push code lên remote
git push origin master
```

### Convention cho commit message

```bash
feat: Thêm tính năng mới
fix: Sửa lỗi
docs: Cập nhật tài liệu
style: Thay đổi format, không ảnh hưởng code
refactor: Refactor code
test: Thêm test
chore: Cập nhật build, config
```

[⬆️ Quay lại mục lục](#-mục-lục)

---

## 7. Làm việc với Git LFS (Large File Storage)

Git LFS được sử dụng để quản lý các file lớn (video, audio, assets game, v.v.)

### 7.1. Cài đặt và cấu hình LFS

**Cài đặt Git LFS:**
- Tải từ: <https://git-lfs.com/>
  ```

**Khởi tạo LFS trong repository:**
```bash
git lfs install
```

Kết quả: `Git LFS initialized.`

### 7.2. Push file lớn với LFS

```bash
# Track file theo extension
git lfs track "*.axp"
git lfs track "*.psd"
git lfs track "*.mp4"

# Hoặc track file cụ thể
git lfs track "Data/Scene.axp"

# Thêm file .gitattributes vào Git
git add .gitattributes

# Thêm file cần push
git add CLIENT/Data/Sound.axp

# Commit
git commit -m "Add large files with LFS"

# Push
git push origin main
```

### 7.3. Pull file LFS

```bash
# Cài đặt LFS (nếu chưa có)
git lfs install

# Nếu chưa có .gitattributes, cần track lại
git lfs track "*.axp"
git add .gitattributes
git commit -m "Fix LFS tracking"

# Fetch tất cả file LFS
git lfs fetch --all

# Checkout file LFS
git lfs checkout

# Hoặc pull bình thường (sẽ tự động pull LFS)
git pull origin master
```

### Kiểm tra file LFS

```bash
# Xem các file đang được track bởi LFS
git lfs ls-files

# Xem thông tin LFS
git lfs env
```

[⬆️ Quay lại mục lục](#-mục-lục)

---

## 8. Cấu hình .gitignore

### 8.1. Tạo file .gitignore

```bash
touch .gitignore
```

Sau đó mở file và thêm các pattern cần ignore.

### 8.2. Cú pháp .gitignore

| Ký tự | Ý nghĩa                          |
|-------|----------------------------------|
| `#`   | Dùng để comment                  |
| `*`   | Đại diện cho mọi ký tự           |
| `/`   | Đại diện cho thư mục gốc repo    |
| `!`   | Loại trừ (bỏ qua tất cả trừ file này) |
| `**`  | Match với nhiều cấp thư mục      |


### 8.3. Các mẫu .gitignore thường dùng

```gitignore
# ===========================
# Thư mục build và dependencies
# ===========================
/build/
/dist/
/node_modules/
/__pycache__/
/vendor/
/.venv/

# ===========================
# File tạm, log, backup
# ===========================
*.log
*.tmp
*.bak
*.swp
*~

# ===========================
# File hệ điều hành
# ===========================
.DS_Store       # macOS
Thumbs.db       # Windows
desktop.ini     # Windows

# ===========================
# File cấu hình và bí mật
# ===========================
.env
.env.local
*.secret
*.key
config.local.php
secrets.yml

# ===========================
# IDE và Editor
# ===========================
.vscode/
.idea/
*.sublime-project
*.sublime-workspace

# ===========================
# Ignore tất cả trừ file cụ thể
# ===========================
*.txt
!README.txt

# ===========================
# Ví dụ phức tạp
# ===========================
# Ignore tất cả file .a
*.a

# Nhưng track file lib.a
!lib.a

# Chỉ ignore file TODO ở thư mục gốc
/TODO

# Ignore tất cả file trong thư mục build
build/

# Ignore doc/notes.txt nhưng không ignore doc/server/arch.txt
doc/*.txt

# Ignore tất cả file .pdf trong doc/ và subdirectories
doc/**/*.pdf
```

### Áp dụng .gitignore cho file đã được track

```bash
# Xóa file khỏi Git nhưng giữ trong local
git rm --cached filename

# Hoặc xóa toàn bộ cache
git rm -r --cached .
git add .
git commit -m "Update .gitignore"
```

[⬆️ Quay lại mục lục](#-mục-lục)

---

## 9. Các lệnh Git hữu ích khác

### Quản lý Branch

```bash
# Xem tất cả branch
git branch -a

# Tạo branch mới
git branch branch-name

# Chuyển sang branch khác
git checkout branch-name

# Tạo và chuyển sang branch mới
git checkout -b branch-name

# Xóa branch local
git branch -d branch-name

# Xóa branch remote
git push origin --delete branch-name

# Merge branch
git checkout main
git merge feature-branch
```

### Xem lịch sử và thay đổi

```bash
# Xem lịch sử commit
git log
git log --oneline --graph --all

# Xem thay đổi chưa stage
git diff

# Xem thay đổi đã stage
git diff --staged

# Xem thay đổi của một commit
git show commit-hash
```

### Hoàn tác thay đổi

```bash
# Hoàn tác thay đổi chưa stage
git checkout -- filename

# Hoàn tác thay đổi đã stage
git reset HEAD filename

# Hoàn tác commit gần nhất (giữ thay đổi)
git reset --soft HEAD~1

# Hoàn tác commit gần nhất (xóa thay đổi)
git reset --hard HEAD~1

# Revert một commit (tạo commit mới)
git revert commit-hash
```

### Làm việc với Remote

```bash
# Xem các remote
git remote -v

# Thêm remote mới
git remote add name url

# Đổi tên remote
git remote rename old-name new-name

# Xóa remote
git remote remove name

# Cập nhật URL của remote
git remote set-url origin new-url
```

### Stash - Lưu thay đổi tạm thời

```bash
# Lưu thay đổi tạm thời
git stash

# Lưu với message
git stash save "Work in progress"

# Xem danh sách stash
git stash list

# Áp dụng stash gần nhất
git stash pop

# Áp dụng stash cụ thể
git stash apply stash@{0}

# Xóa stash
git stash drop stash@{0}

# Xóa tất cả stash
git stash clear
```

### Tag

```bash
# Tạo tag
git tag v1.0.0

# Tạo tag với message
git tag -a v1.0.0 -m "Release version 1.0.0"

# Xem danh sách tag
git tag

# Push tag lên remote
git push origin v1.0.0

# Push tất cả tag
git push origin --tags

# Xóa tag
git tag -d v1.0.0
git push origin :refs/tags/v1.0.0
```

[⬆️ Quay lại mục lục](#-mục-lục)

---

## 10. Xử lý sự cố thường gặp

### Conflict khi merge/pull

```bash
# 1. Pull code và gặp conflict
git pull origin main

# 2. Xem file bị conflict
git status

# 3. Mở file và tìm các marker:
# <<<<<<< HEAD
# Your changes
# =======
# Their changes
# >>>>>>> branch-name

# 4. Sửa file, giữ lại code đúng

# 5. Đánh dấu conflict đã được giải quyết
git add conflicted-file

# 6. Commit
git commit -m "Resolve merge conflict"
```

### Commit nhầm file

```bash
# Xóa file khỏi commit gần nhất (giữ file local)
git reset HEAD~1 --soft
git reset HEAD filename
git commit -m "Correct commit"

# Sửa message của commit gần nhất
git commit --amend -m "New message"

# Thêm file vào commit gần nhất
git add forgotten-file
git commit --amend --no-edit
```

### Push bị từ chối

```bash
# Khi remote có commit mới
git pull --rebase origin main
git push origin main

# Force push (NGUY HIỂM - chỉ dùng khi chắc chắn)
git push -f origin main
```

### Xóa file đã commit nhầm

```bash
# Xóa file khỏi Git nhưng giữ file local
git rm --cached sensitive-file
git commit -m "Remove sensitive file"
git push origin main
```

### Recovery commit đã xóa

```bash
# Xem lịch sử tất cả thao tác
git reflog

# Recovery commit
git checkout commit-hash
git checkout -b recovery-branch
```

### Đồng bộ fork với upstream

```bash
# Thêm upstream remote
git remote add upstream git@github.com:original/repo.git

# Fetch từ upstream
git fetch upstream

# Merge vào branch hiện tại
git merge upstream/main

# Hoặc rebase
git rebase upstream/main
```

[⬆️ Quay lại mục lục](#-mục-lục)

---

## 11. Xử lý tình huống up sai – Xóa commit gần nhất

Phần này cực kỳ quan trọng khi dev game TLBB, vì **up nhầm asset, map, axp** có thể gây lỗi cho cả team.

---

### Trường hợp 1️⃣: Đã commit nhưng CHƯA push lên GitHub

👉 Có thể xóa commit gần nhất **an toàn trên máy local**.

```bash
git reset --soft HEAD~1
```

- Xóa commit
- Giữ nguyên file đã chỉnh sửa (để sửa lại và commit lại)

Hoặc xóa sạch cả commit + thay đổi:
```bash
git reset --hard HEAD~1
```

⚠️ **Cẩn thận**: `--hard` sẽ mất toàn bộ thay đổi.

---

### Trường hợp 2️⃣: Đã push commit sai lên GitHub (CHƯA có ai pull)

```bash
git reset --hard HEAD~1
git push origin master --force
```

⚠️ Chỉ dùng khi:
- Repo ít người
- Chắc chắn **chưa ai pull commit đó**

---

### Trường hợp 3️⃣: Đã push và có người khác pull rồi (khuyến nghị)

👉 **KHÔNG reset force**.

Tạo commit mới để rollback:
```bash
git revert HEAD
```

- An toàn
- Có lịch sử rõ ràng
- Phù hợp team đông dev

---

### Trường hợp 4️⃣: Lỡ tay sửa lung tung, muốn quay về trạng thái remote

```bash
git fetch origin
git reset --hard origin/master
```

👉 Máy local sẽ **giống hệt repo trên GitHub**.


[⬆️ Quay lại mục lục](#-mục-lục)

---
## 📚 Tài nguyên tham khảo

- [Git Documentation](https://git-scm.com/doc)
- [GitHub Guides](https://guides.github.com/)
- [Git LFS](https://git-lfs.com/)
- [Gitignore.io](https://www.toptal.com/developers/gitignore) - Tạo file .gitignore tự động
- [Make README](https://www.makeareadme.com/) - Tạo file README.md một cách trực quan
---

## 📝 Ghi chú

- Thay `main` bằng `master` nếu repository của bạn sử dụng branch master
- Thay `username/repository` bằng tên thực tế của repository
- Nên commit thường xuyên với message rõ ràng
- Luôn pull trước khi push để tránh conflict

---

**Tác giả:** d3mbun  
**Email:** d3mbun@gmail.com  
**Cập nhật lần cuối:** Tháng 1/2026