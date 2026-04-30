# Scenario C — Bug hunt

**Mục tiêu**: AI tự tìm bug mà không được brief trước. Wow factor cao nhất.
**Target**: https://www.saucedemo.com/ (login với user có bug)

## Prompt copy-paste vào Claude Code

```
Đăng nhập https://www.saucedemo.com/ với problem_user / secret_sauce.
Sau khi vào, khám phá toàn bộ trang Products + Cart + Checkout flow trong tối đa 3 phút.

Giới hạn:
- Chỉ thao tác trong domain saucedemo.com
- KHÔNG đăng ký, KHÔNG submit payment thật
- KHÔNG vào URL ngoài trừ khi link nội bộ

Nhiệm vụ: tìm CÀNG NHIỀU bug càng tốt. Kiểm tra:
- Hình ảnh sản phẩm có đúng không
- Tên/giá sản phẩm có đúng không
- Form input có chấp nhận giá trị bất thường (ký tự đặc biệt, để trống...)
- Click Add to Cart / Remove có hoạt động đúng không
- Sort dropdown có sort đúng không
- Bất kỳ hành vi lạ nào khác

Báo cáo từng bug tìm được theo format:
- Bug #N: [mô tả]
- Steps to reproduce
- Expected vs Actual
- Screenshot
```

## Điều cần lưu ý khi demo

- `problem_user` là user được saucedemo cố ý gắn nhiều bug:
  - Tất cả ảnh sản phẩm là cùng 1 con chó
  - Sort dropdown không hoạt động
  - Add to Cart trên 1 số sản phẩm bị lỗi (button đổi text sai)
  - Form checkout: Last Name không submit được → lỗi validation
- AI thường tìm được 3-5 bug trong 3 phút → demo rất ấn tượng.
- Có thể dùng `visual_user` thay nếu muốn focus vào lỗi visual / CSS.

## Backup — nếu Claude không tìm ra bug rõ

Nhắc thêm: "Thử sort dropdown các option khác nhau và kiểm tra thứ tự có đúng không"
