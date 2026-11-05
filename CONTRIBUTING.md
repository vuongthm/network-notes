# Đóng góp nội dung

Repo này chứa ghi chép kiến thức theo module. Thêm bài ở `vi/new-note.md` hoặc trong nhóm nội dung sẵn có; bản Anh đặt ở `en/new-note.md`. Ảnh/diagram nằm trong `media/` và được tham chiếu bằng đường dẫn tương đối.

```markdown
---
id: network-notes:new-note
title: Tiêu đề ghi chép
description: Người đọc sẽ hiểu điều gì?
date: 2026-01-01
status: draft
kind: note
level: beginner
---

## Nội dung

Giải thích, ví dụ có thể kiểm chứng và nguồn tham khảo.
```

Dùng topic của `module.json`. Khi góp bài theo lộ trình, thêm đúng `learning.path`, `learning.section`, `learning.step` trong định nghĩa lộ trình của module; bài độc lập bỏ trường `learning`. Có thể góp bản dịch, sửa lỗi hoặc câu hỏi tự ôn dưới dạng fence `quiz` với đáp án và giải thích.

## Gửi thay đổi

1. Tạo nhánh hoặc fork, sửa đúng nội dung và ngôn ngữ liên quan.
2. Giữ ID/slug cũ khi sửa bài; giữ chung ID hoặc `translationKey` giữa các bản dịch.
3. Kiểm tra cú pháp, code/ví dụ và nguồn tham khảo. Chỉ dùng media có quyền sử dụng.
4. Giữ bài mới ở trạng thái draft để review. Ghi rõ nội dung và cách kiểm tra trong PR.

Không đưa credential, nội dung cá nhân chưa được phép công khai, dependencies hoặc cache vào PR. Các thiết lập vận hành do maintainer quản lý; không cần yêu cầu khóa cá nhân để gửi đóng góp.
