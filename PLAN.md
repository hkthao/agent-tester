# MVP Plan: Browser Use MCP + Claude Code CLI

> Mục tiêu: Dev setup xong, demo cho team tester thấy AI tự điều khiển browser để test web app.

---

## Architecture

```
Claude Code CLI  →  Browser Use MCP (local)  →  Chrome / Playwright
(reasoning, auth    (expose raw browser tools   (click, navigate,
 có sẵn hết)         qua stdin/stdout)            screenshot...)
```

Browser Use MCP chạy ở mode `--mcp` (local stdin/stdout) — chỉ là "tay chân" expose primitives (click, type, navigate, screenshot...). Mọi reasoning do Claude Code lo. **Không cần** `ANTHROPIC_API_KEY` hay `BROWSER_USE_API_KEY` (key cloud chỉ cần khi dùng Browser Use Cloud, không phải mode này).

---

## Phase 0 — Prerequisites (~15 phút)

Kiểm tra trước:

```bash
python3 --version   # cần 3.11+
claude --version    # Claude Code CLI đã cài chưa
```

> Lưu ý: trên macOS dùng `python3` (không có alias `python`).

---

## Phase 1 — Cài Browser Use + đăng ký MCP (~20 phút)

macOS Homebrew Python chặn `pip install` global (PEP 668), nên cài trong virtualenv là sạch nhất:

```bash
cd /path/to/agent-tester
python3 -m venv .venv
.venv/bin/pip install --upgrade pip
.venv/bin/pip install browser-use playwright
.venv/bin/playwright install chromium
```

Verify CLI:

```bash
.venv/bin/browser-use --help    # phải thấy flag --mcp
```

File `.mcp.json` đã có sẵn trong repo, dùng relative path tới venv:

```json
{
  "mcpServers": {
    "browser-use": {
      "command": ".venv/bin/browser-use",
      "args": ["--mcp"]
    }
  }
}
```

> Relative path resolve theo thư mục chứa `.mcp.json`. Người clone mới chỉ cần tạo `.venv` ở đúng vị trí là chạy được, không cần sửa config.

> Claude Code tự pick up file này khi chạy trong cùng thư mục.

---

## Phase 2 — Smoke test (~10 phút)

```bash
cd your-project
claude
```

Trong Claude Code, kiểm tra MCP đã load chưa:

```
/mcp
```

Nếu thấy `browser-use` trong list → thành công. Test thử:

```
Mở google.com và cho tôi biết tiêu đề trang
```

Chrome tự mở, thao tác, trả kết quả về terminal là xong.

---

## Phase 3 — Chuẩn bị 3 demo scenario (~30 phút)

### Scenario A — Validation errors
*Dễ thấy nhất, ấn tượng nhất với tester.*

```
Vào trang [staging URL]/login. Thử đăng nhập với:
1. Email sai format "abc" → kiểm tra có báo lỗi không
2. Bỏ trống password → kiểm tra thông báo lỗi
3. Đăng nhập đúng → kiểm tra redirect về dashboard
Chụp screenshot mỗi bước và tóm tắt kết quả.
```

### Scenario B — Happy path end-to-end
*Thay thế việc tester click thủ công mỗi lần có build mới.*

```
Thực hiện luồng [tính năng chính] từ đầu đến cuối.
Ghi lại thời gian hoàn thành và bất kỳ điểm nào
UI bị lag hoặc confusing.
```

### Scenario C — Bug hunt
*Wow factor cao nhất — AI tự nghĩ ra edge case.*

```
Khám phá trang [URL] trong 2 phút. Giới hạn scope:
chỉ thao tác trong domain [domain], KHÔNG submit form
liên quan tới payment / xóa dữ liệu thật. Thử các thao
tác edge case: nhập ký tự đặc biệt, để trống form,
click liên tục... Báo cáo bất kỳ lỗi hoặc hành vi
bất thường.
```

---

## Phase 4 — Demo cho team (~30 phút)

Thứ tự demo hiệu quả nhất:

1. Mở Claude Code terminal — team thấy giao diện quen
2. Chạy Scenario A — team thấy Chrome tự thao tác
3. Cho 1 tester tự gõ task bằng tiếng Việt → Claude tự hiểu và chạy
4. Chạy Scenario C — "bug hunt" thường gây wow
5. Hỏi feedback: thao tác nào team hay làm thủ công nhất?

---

## Checklist tổng

- [ ] Python 3.11+ đã cài (`python3 --version`)
- [ ] Claude Code CLI đã cài (`claude --version`)
- [ ] `pip3 install browser-use playwright` xong
- [ ] `playwright install chromium` xong
- [ ] `browser-use --help` thấy flag `--mcp`
- [ ] `.mcp.json` đã tạo trong project folder (dùng `command: browser-use`, `args: ["--mcp"]`)
- [ ] `/mcp` trong Claude Code thấy `browser-use`
- [ ] Smoke test google.com thành công
- [ ] Staging URL có VPN / credentials sẵn sàng (nếu cần)
- [ ] 3 scenario đã thay `[staging URL]` bằng URL thật
- [ ] Demo chạy thử một mình trước khi show team

---

## Ghi chú thay đổi so với plan v1

- `python` → `python3` (macOS không có alias `python`)
- `pip` → `pip3`
- `.mcp.json`: `python -m browser_use` → `browser-use --mcp` (CLI có flag `--mcp` chính thức cho stdin/stdout MCP mode, theo [docs](https://docs.browser-use.com/open-source/browser-use-cli))
- Scenario C bổ sung scope guard để tránh AI lạc sang thao tác nguy hiểm
- Checklist thêm bước verify `browser-use --help` và staging credentials
