# Bài 2: Quản lý nhánh và Giải quyết xung đột (Merge Conflict)

## 1. Mục tiêu

* Tạo mới và di chuyển giữa các nhánh Git cục bộ.
* Tạo ra Merge Conflict giữa hai nhánh.
* Xử lý Merge Conflict thủ công trong file `README.md`.
* Hiểu các ký hiệu `<<<<<<<`, `=======`, `>>>>>>>`.
* Hoàn thành Merge Commit.
* Kiểm tra lịch sử commit bằng lệnh `git log --graph --oneline`.
* Hiểu cơ chế gộp 3 vùng (3-Way Merge).

---

## 2. Bối cảnh

Trong bài thực hành, nhánh `feature-update` được tạo ra từ nhánh chính `main` để phát triển một nội dung mới.

Sau đó, nhánh `main` cũng được cập nhật trên cùng file `README.md`.

Hai nhánh cùng thay đổi một vùng nội dung trong `README.md`, vì vậy khi thực hiện Merge, Git không thể tự động lựa chọn nội dung và xảy ra Merge Conflict.

Conflict được xử lý thủ công trước khi hoàn thành Merge.

---

## 3. Khởi tạo Repository

Khởi tạo Git Repository:

```bash
git init
```

Kiểm tra trạng thái:

```bash
git status
```

Tạo commit đầu tiên cho `README.md`:

```bash
git add README.md
git commit -m "Initial README"
```

Commit ban đầu:

```text
a1fd162 Initial README
```

---

## 4. Tạo nhánh feature-update

Tạo nhánh mới và chuyển sang nhánh `feature-update`:

```bash
git switch -c feature-update
```

Sau đó cập nhật nội dung file `README.md` trên nhánh `feature-update`.

Commit thay đổi:

```bash
git add README.md
git commit -m "Update README on feature branch"
```

Commit trên nhánh `feature-update`:

```text
f301c7d Update README on feature branch
```

---

## 5. Cập nhật nhánh main

Chuyển về nhánh chính:

```bash
git switch main
```

Sau đó cập nhật cùng vùng nội dung trong file `README.md`.

Commit thay đổi trên `main`:

```bash
git add README.md
git commit -m "Update README on main branch"
```

Commit trên nhánh `main`:

```text
cf5fb71 Update README on main branch
```

Tại thời điểm này, lịch sử có hai nhánh phát triển riêng:

```text
* cf5fb71 (main) Update README on main branch
| * f301c7d (feature-update) Update README on feature branch
|/
* a1fd162 Initial README
```

---

## 6. Tạo Merge Conflict

Đang ở nhánh `main`, thực hiện Merge:

```bash
git merge feature-update
```

Git phát hiện hai nhánh cùng thay đổi nội dung trong `README.md` và thông báo:

```text
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

Kiểm tra trạng thái:

```bash
git status
```

Kết quả:

```text
On branch main
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   README.md
```

Điều này cho thấy file `README.md` đã xảy ra Merge Conflict.

---

## 7. Xử lý Merge Conflict thủ công

Git đánh dấu vùng xung đột trong `README.md` bằng các ký hiệu:

```text
<<<<<<< HEAD
Nội dung trên nhánh main
=======
Nội dung trên nhánh feature-update
>>>>>>> feature-update
```

Ý nghĩa:

* `<<<<<<< HEAD`: bắt đầu nội dung hiện tại trên nhánh `main`.
* `=======`: phân cách giữa hai phiên bản nội dung.
* `>>>>>>> feature-update`: kết thúc nội dung đến từ nhánh `feature-update`.

Tiến hành mở file `README.md` và xử lý thủ công.

Sau khi giải quyết, các ký hiệu:

```text
<<<<<<<
=======
>>>>>>>
```

được xóa khỏi file.

Nội dung cuối cùng được giữ lại sau khi giải quyết Conflict.

---

## 8. Đánh dấu Conflict đã được giải quyết

Sau khi chỉnh sửa `README.md`, chạy:

```bash
git add README.md
```

Git xác nhận:

```text
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)
```

Điều này cho biết Conflict đã được giải quyết và file đã được đưa vào Staging Area.

---

## 9. Hoàn thành Merge Commit

Tạo Merge Commit:

```bash
git commit -m "Merge feature-update and resolve conflict"
```

Git tạo Merge Commit:

```text
0e9b965 Merge feature-update and resolve conflict
```

---

## 10. Kiểm tra lịch sử commit dạng đồ thị

Sử dụng lệnh:

```bash
git log --graph --oneline --all
```

Kết quả thực tế:

```text
*   0e9b965 (HEAD -> main) Merge feature-update and resolve conflict
|\
| * f301c7d (feature-update) Update README on feature branch
* | cf5fb71 Update README on main branch
|/
* a1fd162 Initial README
```

### Phân tích kết quả

* `a1fd162`: Commit ban đầu của Repository.
* `cf5fb71`: Commit cập nhật `README.md` trên nhánh `main`.
* `f301c7d`: Commit cập nhật `README.md` trên nhánh `feature-update`.
* `0e9b965`: Merge Commit sau khi giải quyết Conflict.
* Nhánh `feature-update` được tách ra từ commit `a1fd162`.
* Nhánh `main` và `feature-update` phát triển độc lập.
* Hai nhánh được hợp nhất tại Merge Commit `0e9b965`.
* Merge Commit có hai nhánh tổ tiên, thể hiện qua:

```text
|\
|/
```

---

## 11. Kiểm tra trạng thái Repository

Sau khi hoàn thành Merge, chạy:

```bash
git status
```

Kết quả:

```text
On branch main
nothing to commit, working tree clean
```

Điều này chứng minh:

* Merge đã hoàn thành.
* Không còn Conflict.
* Không còn thay đổi chưa được commit.
* Working Tree sạch.

---

## 12. Cơ chế 3-Way Merge

Git sử dụng cơ chế 3-Way Merge để hợp nhất hai nhánh.

Ba phiên bản được sử dụng gồm:

1. Commit chung gần nhất của hai nhánh.
2. Phiên bản hiện tại của nhánh `main`.
3. Phiên bản của nhánh `feature-update`.

Trong bài thực hành:

```text
              cf5fb71 (main)
             /
a1fd162 ----<
             \
              f301c7d (feature-update)
```

Sau khi Merge:

```text
              cf5fb71 (main)
             /        \
a1fd162 ----<          0e9b965 (Merge Commit)
             \        /
              f301c7d
```

Do hai nhánh cùng thay đổi một vùng trong `README.md`, Git không thể tự động quyết định nội dung cuối cùng và yêu cầu người dùng xử lý Conflict thủ công.

---

## 13. Kết quả đạt được

Bài thực hành đã hoàn thành các yêu cầu:

* [x] Tạo nhánh `feature-update`.
* [x] Di chuyển giữa `main` và `feature-update`.
* [x] Cập nhật cùng file `README.md` trên hai nhánh.
* [x] Tạo Merge Conflict.
* [x] Xử lý Conflict thủ công.
* [x] Xóa các ký hiệu `<<<<<<<`, `=======`, `>>>>>>>`.
* [x] Sử dụng `git add README.md` để đánh dấu Conflict đã được giải quyết.
* [x] Tạo Merge Commit.
* [x] Kiểm tra lịch sử bằng `git log --graph --oneline --all`.
* [x] Kiểm tra trạng thái cuối cùng bằng `git status`.

---

## 14. Minh chứng

### 14.1. Minh chứng Merge Conflict

Kết quả khi chạy:

```bash
git merge feature-update
```

```text
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

**Ảnh minh chứng:** Chèn ảnh chụp màn hình kết quả Merge Conflict tại đây.

---

### 14.2. Minh chứng xử lý Conflict

Sau khi chỉnh sửa thủ công `README.md`, chạy:

```bash
git add README.md
```

Git thông báo:

```text
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)
```

**Ảnh minh chứng:** Chèn ảnh chụp màn hình quá trình xử lý Conflict tại đây.

---

### 14.3. Minh chứng lịch sử commit

Lệnh kiểm tra:

```bash
git log --graph --oneline --all
```

Kết quả:

```text
*   0e9b965 (HEAD -> main) Merge feature-update and resolve conflict
|\
| * f301c7d (feature-update) Update README on feature branch
* | cf5fb71 Update README on main branch
|/
* a1fd162 Initial README
```

**Ảnh minh chứng:** Chèn ảnh chụp màn hình kết quả `git log --graph --oneline --all` tại đây.

---

### 14.4. Minh chứng trạng thái cuối cùng

Lệnh:

```bash
git status
```

Kết quả:

```text
On branch main
nothing to commit, working tree clean
```

**Ảnh minh chứng:** Chèn ảnh chụp màn hình kết quả `git status` tại đây.

---

## 15. Kết luận

Qua bài thực hành, đã thực hiện thành công việc quản lý nhánh và giải quyết Merge Conflict trong Git.

Quá trình thực hành đã minh họa việc hai nhánh cùng thay đổi một vùng nội dung trong `README.md`, dẫn đến Conflict. Conflict được xử lý thủ công bằng cách chỉnh sửa nội dung và loại bỏ các ký hiệu đánh dấu Conflict.

Sau đó, hai nhánh được hợp nhất thành công bằng Merge Commit:

```text
0e9b965 Merge feature-update and resolve conflict
```

Lịch sử Git sau cùng thể hiện rõ hai nhánh và Merge Commit bằng:

```bash
git log --graph --oneline --all
```

Repository ở trạng thái sạch:

```text
nothing to commit, working tree clean
```
