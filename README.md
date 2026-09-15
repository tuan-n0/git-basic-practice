# BÀI THỰC HÀNH CHƯƠNG 2 — GIT CƠ BẢN

- **Họ tên:** Ngô Anh Tuấn
- **Mã sinh viên:** dtc245200515

---

## 📂 DANH MỤC BÀI NỘP

| Bài | Nội dung | Nơi nộp |
|---|---|---|
| 1 | Khởi tạo repository và commit đầu tiên | Repository này |
| 2 | Commit nhiều lần và xem lịch sử | Repository này |
| 3 | Di chuyển giữa các phiên bản (checkout/reset) | Repository này + [BAO-CAO.md](BAO-CAO.md) |
| 4 | Tạo project trên GitHub và kết nối remote | Repository này |
| 5 | Đồng bộ remote (clone/pull/push) | Repository này |
| 6 | **Tổng hợp — trang giới thiệu cá nhân** | 👉 **https://github.com/tuan-n0/gioi-thieu-ca-nhan** |

> ⚠️ **Bài 6 nằm ở repository riêng** theo đúng yêu cầu của đề (project mới, quy trình làm lại từ đầu):
> **https://github.com/tuan-n0/gioi-thieu-ca-nhan**

📄 Phần trả lời **câu hỏi tư duy** và **bảng so sánh 3 loại reset** nằm trong file [BAO-CAO.md](BAO-CAO.md).

---

## Lịch sử commit của Bài 1 → 5

```
38c5ea8  Add report file via GitHub web      (Bài 5 - tạo trên giao diện web)
650322c  Add about file                      (Bài 5 - push từ local)
6b65387  Update style and notes              (Bài 2)
426d4f9  Add javascript file                 (Bài 2)
41d9f09  Update html content                 (Bài 2)
753cb67  Initial commit: add html and css    (Bài 1)
```

## Lịch sử commit của Bài 6

```
49bea4a  Revert "Broken CSS: wrong colors"   (bước 6 - sửa lỗi bằng git revert)
8e44750  Broken CSS: wrong colors            (bước 6 - commit sai cố ý)
63d6713  Style introduction section          (bước 5)
ac5d977  Add introduction section            (bước 4)
630d1d5  Initial structure                   (bước 2)
```

**Ghi chú Bài 6:** commit `Broken CSS: wrong colors` là commit sai **cố ý** theo yêu cầu bước 6.
Do commit này đã được push lên GitHub nên không dùng `git reset --hard` (sẽ phải force-push
và ghi đè lịch sử chung), mà dùng `git revert` để tạo commit mới triệt tiêu thay đổi sai.
Nhờ vậy toàn bộ commit hợp lệ trước đó được giữ nguyên.
