# Bài 4: Quản lý tệp tin bỏ qua (.gitignore) và Sửa lịch sử (Amend)

## 1. Mục tiêu

Thực hành xử lý trường hợp vô tình commit file `credentials.txt` chứa thông tin nhạy cảm. File được giữ nguyên trên máy tính nhưng được gỡ khỏi danh sách theo dõi của Git, sau đó cấu hình `.gitignore` và sửa commit gần nhất bằng `git commit --amend`.

## 2. Quy trình thực hiện

### Bước 1. Commit nhầm `credentials.txt`

Ban đầu, file `credentials.txt` được tạo và commit nhầm vào repository.

Trạng thái sau khi commit nhầm:

```text

```

### Bước 2. Tạo `.gitignore`

Nội dung `.gitignore`:

```gitignore
# File chứa thông tin nhạy cảm
credentials.txt

# File rác hệ thống
*.log
.DS_Store
Thumbs.db
```

Quy tắc `credentials.txt` giúp Git bỏ qua file này trong các lần thay đổi sau.

### Bước 3. Gỡ file khỏi vùng theo dõi nhưng không xóa file vật lý

Sử dụng đúng lệnh theo yêu cầu:

```bash
git rm --cached credentials.txt
```

Kết quả lệnh:

```text
rm 'credentials.txt'
```

Lệnh `git rm --cached` chỉ xóa file khỏi Git index, **không xóa file `credentials.txt` khỏi thư mục làm việc**.

Sau khi thực hiện lệnh, trạng thái:

```text
D  credentials.txt
?? .gitignore
```

Trong đó `D credentials.txt` ở vùng staged thể hiện Git sẽ bỏ file khỏi phiên bản được theo dõi, còn file vật lý vẫn tồn tại và được `.gitignore` bỏ qua sau khi commit.

### Bước 4. Commit và sửa thông điệp bằng Amend

Thêm `.gitignore` vào vùng staged:

```bash
git add .gitignore
```

Sau đó sử dụng:

```bash
git commit --amend -m "Cấu hình .gitignore và loại credentials khỏi theo dõi"
```

Kết quả commit:

```text
[main b639fc5] Cấu hình .gitignore và loại credentials khỏi theo dõi
 Date: Sun Oct 4 13:06:17 2026 +0000
 1 file changed, 7 insertions(+)
 create mode 100644 .gitignore
```

### Bước 5. Kiểm tra trạng thái

Lệnh:

```bash
git status
```

Kết quả cuối cùng:

```text
(working tree clean)
```

Repository sạch, không còn `credentials.txt` ở trạng thái Staged hoặc Modified.

### Bước 6. Kiểm tra commit gần nhất

Lệnh:

```bash
git log -n 1
```

Kết quả rút gọn:

```text
b639fc5 Cấu hình .gitignore và loại credentials khỏi theo dõi
```

Thông điệp commit gần nhất đã được sửa thành:

```text
Cấu hình .gitignore và loại credentials khỏi theo dõi
```

## 3. Kiểm tra file vật lý

File `credentials.txt` vẫn tồn tại trong thư mục làm việc cục bộ và không bị xóa bằng lệnh của hệ điều hành.

Kiểm tra bằng:

```bash
Test-Path credentials.txt
```

Kết quả:

```text
True
```

## 4. Kết luận

Bài thực hành đã đáp ứng các yêu cầu:

- Đã tạo `.gitignore` để bỏ qua `credentials.txt` và một số file rác.
- Đã sử dụng `git rm --cached credentials.txt` để gỡ file khỏi Git nhưng giữ file vật lý.
- Không sử dụng lệnh xóa file vật lý.
- Đã sử dụng `git commit --amend` để sửa commit gần nhất.
- `git status` cuối cùng sạch.
- `git log -n 1` hiển thị thông điệp commit đã được sửa.
