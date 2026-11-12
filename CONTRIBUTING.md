# Viết ghi chép trong network-notes

Các bài khung thuộc Pre Security đã được chuyển về `security-notes/pre-security/` để bộ khung nằm gọn một chỗ. Repo này dành cho ghi chép độc lập hoặc chuyên sâu; một path vẫn có thể tham chiếu bài ở đây bằng ID.

Bạn có thể viết bài ở bất kỳ thứ tự nào. Mỗi module có `vi/`, `en/`, `media/`; một bài có thể gắn nhiều chủ đề bằng `topics` và được nhiều lộ trình tham chiếu.

## Module hiện có

- `fundamentals` — ID `fundamentals`.
- `protocols-and-devices` — ID `protocols-and-devices`.
- `how-the-web-works` — ID `how-the-web-works`.

## Tạo bài nháp trong module

```bash
pnpm content:new article network-notes new-note --module fundamentals
pnpm content:new article network-notes new-note --module fundamentals --lang en
```

Ngày mặc định là ngày hiện tại theo giờ Việt Nam, không tiếp nối lịch của các khung nháp. Bản dịch kế thừa định danh, chủ đề, ngày và thứ tự. Khi đăng, điền mô tả/nội dung, sửa ngày thực tế rồi đổi `status: published`. Các file đã có khung có thể mở trực tiếp để viết, không cần tạo lại.

## Phân loại và lộ trình

- `module`: ID module trong manifest, độc lập với đường dẫn thư mục.
- `topics`: có thể gồm nhiều chủ đề đã đăng ký như `security`, `soc`, `networking`, `linux`, `cloud`, `ai`.
- `order`: thứ tự mặc định trong module, không phải ngày đăng.
- `id`, `translationKey`, `slug`: giữ ổn định, kể cả khi chuyển thư mục/repo; bản dịch dùng chung định danh.
- `learning-path.json → sections[].steps[].article`: chọn ID bài và thứ tự học riêng cho từng lộ trình. Không chép bài, không cần khai báo `learning` trong Markdown.
- `outline: true`: chủ động cho phép tên bản nháp hiện trong đề cương; thân bài/mô tả/media vẫn chưa công khai.

SOC L1, Pre Security và Cyber Security 101 đã chọn từng bài theo ID. Thêm bài mới vào module không tự đổi đề cương. Khi tự chọn chế độ `sections[].module`, path sẽ nhận thêm các bài của module.

Hướng dẫn đầy đủ: [CONTENT.md của website](https://github.com/vuongthm/vuongthm.github.io/blob/main/CONTENT.md).
