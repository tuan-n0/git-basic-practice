# BÁO CÁO THỰC HÀNH — CHƯƠNG 2: GIT CƠ BẢN

- Họ tên: Ngô Anh Tuấn
- Mã sinh viên: dtc245200515
- Repository: https://github.com/tuan-n0/git-basic-practice

---

## BÀI 3 — Bảng so sánh 3 loại reset

Thử nghiệm trên file `temp.txt` được commit tạm rồi reset lần lượt bằng 3 cách.

| Loại reset | Repository (commit) | Staging Area | Working Directory | Trạng thái `temp.txt` sau lệnh |
|---|---|---|---|---|
| `--soft` | Hủy commit | Giữ nguyên | Giữ nguyên | Còn trên ổ đĩa, đang ở trạng thái staged (màu xanh) |
| `--mixed` | Hủy commit | Bị dọn sạch | Giữ nguyên | Còn trên ổ đĩa, trở về untracked (màu đỏ) |
| `--hard` | Hủy commit | Bị dọn sạch | Bị ghi đè | Bị xóa hẳn khỏi ổ đĩa |

### Nhận xét thực tế khi chạy

- Sau `--soft`: `git status` hiển thị `temp.txt` trong mục "Changes to be committed". Commit lại được ngay mà không cần `git add`.
- Sau `--mixed`: `git status` hiển thị `temp.txt` trong mục "Untracked files". Muốn commit lại bắt buộc phải `git add` trước, vì staging area đã bị dọn sạch.
- Sau `--hard`: `git status` báo "working tree clean", lệnh `ls -l` không còn thấy `temp.txt`. File mất hoàn toàn, không khôi phục được bằng Ctrl+Z.

Mức độ ảnh hưởng tăng dần theo ba tầng của Git: `--soft` chỉ tác động tầng Repository, `--mixed` tác động thêm Staging Area, `--hard` tác động cả ba tầng kể cả Working Directory.

---

## BÀI 1 — Câu hỏi tư duy: vai trò của Staging Area

Staging Area (Index) là tầng trung gian giữa thư mục làm việc và repository, cho phép người dùng **chọn lọc chính xác những thay đổi nào sẽ đi vào commit**.

Bài 1 là ví dụ trực tiếp: thư mục có 3 file thay đổi nhưng chỉ cần ghi 2 file vào lịch sử. Nhờ staging area, chỉ `index.html` và `style.css` được đưa vào commit, còn `notes.txt` vẫn đứng ngoài.

Nếu không có staging area, việc commit sẽ bất tiện ở ba điểm:

1. **Không lọc được nội dung commit.** Git sẽ buộc phải ghi toàn bộ mọi thay đổi trong thư mục vào commit. Các file ghi chú cá nhân, file cấu hình chứa mật khẩu hay file thử nghiệm dở dang đều bị đẩy vào lịch sử dự án.

2. **Không tách được commit theo chủ đề.** Khi sửa hai việc không liên quan cùng lúc, người dùng không thể chia thành hai commit riêng. Lịch sử trở nên lộn xộn, sau này rất khó dò lại xem thay đổi nào thuộc về việc gì.

3. **Không rà soát được trước khi chốt.** Có staging area thì chạy `git diff --staged` để xem chính xác phần sắp commit. Không có nó, người dùng chỉ biết kết quả sau khi commit đã ghi vào lịch sử.

Có thể hình dung: Working Directory là bàn làm việc, Staging Area là chiếc khay xếp riêng những gì muốn gửi đi, còn Repository là kho lưu trữ vĩnh viễn.

---

## BÀI 3 — Câu hỏi tư duy: `git reset --hard` sau khi đã push

**Không an toàn, cần tránh tuyệt đối.**

**Lý do 1 — Lịch sử hai bên bị lệch.** `reset --hard` chỉ xóa commit ở repository local, trên GitHub commit đó vẫn tồn tại. Khi push, Git từ chối vì phát hiện local đang thiếu commit so với remote. Muốn đẩy được phải dùng `git push --force`, mà lệnh này ghi đè lên lịch sử chung của cả nhóm.

**Lý do 2 — Đồng đội mất code.** Nếu có thành viên đã pull commit vừa bị xóa và làm việc tiếp trên nền đó, sau khi force-push lịch sử của họ trở nên mồ côi. Lần pull kế tiếp sẽ phát sinh xung đột phức tạp, trường hợp xấu có thể mất phần công việc đã làm.

**Lý do 3 — Vi phạm nguyên tắc của Git.** Git có nguyên tắc: không viết lại lịch sử đã được công khai. Commit đã push là tài sản chung của nhóm, không còn thuộc quyền định đoạt của riêng một người.

**Cách xử lý đúng: dùng `git revert`.** Lệnh này không xóa commit cũ mà tạo ra một commit mới có nội dung ngược lại để triệt tiêu thay đổi sai, tương tự việc ghi một bút toán điều chỉnh trong sổ kế toán thay vì tẩy xóa dòng cũ. Lịch sử được giữ nguyên vẹn, không cần force-push, không ảnh hưởng tới đồng đội.

| Tiêu chí | `git reset --hard` | `git revert` |
|---|---|---|
| Lịch sử cũ | Bị xóa | Được giữ nguyên |
| Cần force-push | Có | Không |
| Ảnh hưởng đồng đội | Nghiêm trọng | Không |
| Dùng cho commit đã push | Không nên | Nên dùng |

---

## BÀI 5 — Câu hỏi: `git pull` là tổ hợp của 2 lệnh nào?

`git pull` là tổ hợp của **`git fetch`** và **`git merge`**.

- **`git fetch`** tải toàn bộ commit mới từ remote về repository local và cập nhật nhánh theo dõi `origin/main`. Bước này **chưa** đụng gì tới thư mục làm việc, nên hoàn toàn an toàn.
- **`git merge origin/main`** hợp nhất những commit vừa tải về vào nhánh đang làm việc, lúc này nội dung file trong thư mục mới thực sự thay đổi.

Vì vậy `git pull` tương đương với việc chạy lần lượt:
