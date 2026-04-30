# Scenario A — Validation errors

**Mục tiêu**: chứng minh AI test được các rule validation cơ bản ở trang login.
**Target**: https://www.saucedemo.com/

## Prompt copy-paste vào Claude Code

```
Mở https://www.saucedemo.com/ và thực hiện 4 test case ở trang đăng nhập.
Sau mỗi case, chụp screenshot và ghi lại thông báo lỗi (nếu có):

1. Bỏ trống cả Username và Password → bấm Login
2. Nhập Username "standard_user", bỏ trống Password → bấm Login
3. Nhập Username "locked_out_user", Password "secret_sauce" → bấm Login
4. Nhập Username "standard_user", Password "secret_sauce" → bấm Login
   (case này phải vào được trang Products)

Tóm tắt kết quả thành bảng: case | hành vi mong đợi | hành vi thực tế | pass/fail.
```

## Điều cần lưu ý khi demo

- Case 3 (locked_out_user) là hidden gem — nhiều tester không biết saucedemo có user này. AI sẽ phát hiện thông báo "Sorry, this user has been locked out."
- Case 4 verify được redirect tới `/inventory.html` — AI phải gọi `state` để confirm URL đổi.
