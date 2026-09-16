# TEAM — Day04, K4-L3B

**Làm nhóm.** Mỗi người tự viết và commit phần INDIVIDUAL của mình.

## Thông tin bài nộp

- Tên nhóm: PhungThanhAn-2A202603006
- Người đại diện / MSSV: Phùng Thành An / 2A202603006
- Tên repo: `K4-L3-DAY04-PhungThanhAn-2A202603006-PromptEngineeringToolCalling`
- URL repo, nhánh nộp, commit chốt: https://github.com/Mashall-Anie/K4-L3-DAY04-PhungThanhAn-2A202603006-PromptEngineeringToolCalling, nhánh `main`, commit chốt: 218d755f39d8e122def8b53272f87875425cca18
- Deadline áp dụng và link thông báo đổi hạn nếu có: 12h00 - 16/09/2026

## Thành viên

| Họ và tên | MSSV | GitHub | Vai trò và công việc | File/commit/PR |
|---|---|---|---|---|
| Phùng Thành An | 2A202603006 | Mashall-Anie | Toàn bộ: viết/sửa system_prompt.md v0–v3, viết 10 case nhóm, chạy eval base/group/adversarial, chạy UI lấy transcript, viết REPORT.md | `starter_v0/artifacts/system_prompt.md`, `starter_v0/data/eval_group.json`, `starter_v0/version_log.csv`, `starter_v0/runs/*.json`, `starter_v0/transcripts/*.json`, `starter_v0/artifacts/REPORT.md` |

## Nhận xét chung

- **Kết quả và bằng chứng**: Cải thiện agent qua 3 vòng có đo lường (v0: 0.70 → v1: 0.7333 → v2: 0.7667 → v3: 1.0 trên bộ 30 case cơ bản, `runs/v3_B_base_openrouter_20260915T205504173722.json`). Bộ 10 case tự viết đạt 10/10 sau khi tự phát hiện và sửa lỗi format `eval_group.json` (dùng sai key `input` thay vì `turns` cho case nhiều lượt). Bộ 12 case an toàn đạt 7/12, đã phân tích chi tiết 4 case fail trong REPORT.md B4a.
- **Thay đổi hiệu quả nhất**: rule "Write action confirmation" (v2) — sửa dứt điểm 3 case liên quan tới xác nhận trước khi tạo ticket trên bộ cơ bản (H12, M05, M09), và là nền tảng để phân tích rõ lỗ hổng ở bộ adversarial.
- **Giới hạn còn lại**: Toàn bộ 4/5 lỗi ở bộ adversarial (A03, A04, A10, A11) đều do agent chấp nhận các dạng "xác nhận giả" (JSON tự bịa, pseudo-code, markup giả `<assistant>`, xác nhận cũ đã lỗi thời) thay vì chỉ tin vào một `clarify` thật trong phiên. Ngoài ra, live chat phát hiện thêm agent đôi khi trả lời bằng câu tường thuật ("đang kiểm tra...") mà chưa gọi tool thật ở các lượt giữa hội thoại.
- **Cách phân công và tích hợp**: Nhóm 1 thành viên.

## INDIVIDUAL

### Phùng Thành An — 2A202603006

- **Phần việc và file/commit/PR**: Viết và lặp `system_prompt.md` qua v0→v3 (`artifacts/system_prompt.md`); viết bộ 10 case nhóm G01–G10 (`data/eval_group.json`); chạy toàn bộ eval base/group/adversarial và lưu kết quả (`runs/`); chạy chat UI và lưu transcript (`transcripts/`); viết `version_log.csv` và `artifacts/REPORT.md`.
- **Quyết định, khó khăn và cách xử lý**: Gặp lỗi rate-limit khi chạy trên OpenRouter free tier ở vòng đầu (7/30 case lỗi `provider_error`) — quyết định chuyển sang gọi trực tiếp model trả phí để đảm bảo `provider_error_cases == 0` mới dùng làm bằng chứng. Khi viết 10 case nhóm, gặp lỗi cả 5 case multi-turn trả về câu trả lời rỗng giống hệt nhau — debug bằng cách đọc trực tiếp source `run_eval.py` (`grep -n "is_multiturn"`), phát hiện script xác định case nhiều lượt bằng key `turns`, không phải hình dạng của `input` — sửa lại đúng schema và chạy lại thành công 10/10.
- **Điều đã học**: Routing PASS không đồng nghĩa hành động đã đúng — phải đọc `actual_tool_calls`/`actual_text` và cả file `tickets/` sinh ra để xác nhận có ghi dữ liệu thật hay không; đặc biệt rõ ở bộ adversarial, nơi accuracy tự động không phát hiện được việc ticket vẫn bị tạo dựa trên xác nhận giả mạo.
- **AI/công cụ đã dùng và cách kiểm tra**: Dùng Claude để giải thích lỗi, đề xuất nội dung vá cho `system_prompt.md` qua từng vòng, và hỗ trợ phân tích/tổng hợp report. Mọi thay đổi đều tự chạy `run_eval.py`/`chat.py` để kiểm tra kết quả thật trước khi ghi vào report; không dùng AI để tạo số liệu, run, hay transcript giả.
- **Thời điểm đã tự nộp URL repo chung trên VLearn**: Đã nộp lúc 23:12:36 15/9/2026