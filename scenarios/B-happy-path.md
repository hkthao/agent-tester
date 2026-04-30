# Scenario B — Happy path end-to-end

**Mục tiêu**: thay thế việc tester click thủ công cho luồng mua hàng.
**Target**: https://www.saucedemo.com/

## Prompt copy-paste vào Claude Code

```
Thực hiện luồng mua hàng end-to-end trên https://www.saucedemo.com/:

1. Đăng nhập với standard_user / secret_sauce
2. Thêm 2 sản phẩm bất kỳ vào cart
3. Vào trang cart, kiểm tra số lượng item = 2
4. Bấm Checkout
5. Điền form: First Name "Test", Last Name "User", Postal Code "70000"
6. Bấm Continue, review tổng tiền
7. Bấm Finish, verify trang "Thank you for your order"

Sau mỗi bước, chụp screenshot. Cuối cùng báo cáo:
- Tổng thời gian chạy
- Có bước nào UI lag, confusing, hoặc khác thường không
- Tổng tiền ở bước review có khớp với cộng dồn 2 sản phẩm không
```

## Điều cần lưu ý khi demo

- Đây là luồng "boring" mà QA thường chạy mỗi build → nhấn mạnh giá trị tự động hoá.
- Nếu Claude phát hiện tổng tiền sai (saucedemo không cộng tax đúng) → bonus point.
