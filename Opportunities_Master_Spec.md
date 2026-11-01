# OPPORTUNITIES — ĐẶC TẢ HỢP NHẤT (MASTER SPEC)

**Chức năng · UX · Thiết kế · Kiểm thử — một tài liệu duy nhất, đọc độc lập**

| Mục | Nội dung |
|---|---|
| **Tên sản phẩm** | Opportunities |
| **Loại** | Ứng dụng ghi chú (note-taking), Markdown-first, offline-first, đa thiết bị |
| **Phiên bản tài liệu** | 5.0 — Master / Self-contained |
| **Ngày biên soạn** | 27/10/2026 |
| **Nền tảng** | iOS 16+, Android 10+, Web (Desktop, Tablet, Mobile Web) |
| **Trình duyệt** | Chrome, Edge, Safari, Firefox (2 phiên bản mới nhất) |
| **Ngôn ngữ giao diện** | Tiếng Việt (mặc định), Tiếng Anh |
| **Figma** | https://www.figma.com/design/VWLZhe9YBBkKE3Fl8mNfH5/Oppotunities — Page `04 · UI Prototype` (Mobile, ID 6:5) và `06 · Web — UI Prototype` (Web, ID 64:3) |

> **Cách dùng tài liệu này:** Đây là bản hợp nhất thay thế cho toàn bộ các bản đặc tả, đề xuất UX và phụ lục đã có trước đó. Người đọc **chỉ cần file này**. Mọi xung đột giữa các bản cũ đã được giải quyết công khai ở mục 2; không cần đối chiếu thêm tài liệu nào khác.

---

## MỤC LỤC

0. Quy tắc đọc & nguyên tắc ưu tiên
1. Tổng quan sản phẩm
2. Các quyết định đã chốt & xung đột đã giải quyết
3. Mô hình dữ liệu & mô hình soạn thảo Markdown-first
4. Kiến trúc giao diện & luật chống màn hình thừa
5. Luồng nghiệp vụ chuẩn đầu-cuối
6. Chức năng nền tảng C1–C10
7. Chức năng F1–F22
8. F23 — Trình soạn thảo Markdown (23.1–23.10)
9. Chức năng mở rộng F24–F33
10. Truy vết P1/P2 (đề xuất bổ sung đã hợp nhất)
11. Hệ màu v3 & design tokens
12. Thành phần UI, typography, thuật ngữ tiếng Việt
13. Mobile vs Web: khác biệt, responsive, parity
14. Hợp đồng thiết kế Mobile (Page 04) & danh sách màn hình
15. Yêu cầu phi chức năng
16. Lưu trữ, bảo mật, toàn vẹn dữ liệu
17. Ma trận kiểm thử & chấp nhận
18. Quyết định kỹ thuật còn mở & phụ thuộc triển khai
19. Hướng dẫn thực thi thiết kế trên Figma
20. Định nghĩa "Hoàn thành" (Definition of Done)
Phụ lục A. Bảng tổng hợp chức năng
Phụ lục B. Bảng phím tắt Web
Phụ lục C. Bảng cú pháp Markdown mở rộng nhanh

---

## 0. QUY TẮC ĐỌC & NGUYÊN TẮC ƯU TIÊN

### 0.1 Thứ tự ưu tiên khi có mâu thuẫn
1. **Quyết định UX đã chốt** (mục 2) về trải nghiệm, màu sắc, cấu trúc.
2. **Yêu cầu định lượng** trong các mục chức năng (số lượng, thời gian, kích thước, shortcut).
3. Quy tắc tổ chức giao diện (mục 4).
4. Mockup cũ chỉ là hiện trạng cần kiểm tra, **không** phải nguồn định nghĩa hành vi.

### 0.2 Phân biệt "thiết kế" và "đã chạy thật"
| Loại yêu cầu | Designer phải cung cấp | Developer phải xây |
|---|---|---|
| Hành động UI | Nơi bấm, trạng thái, phản hồi, lỗi | Xử lý sự kiện, logic nghiệp vụ |
| Autosave | Trạng thái Đang lưu / Đã lưu / Offline / Lỗi | Lưu nguyên tử, hàng đợi, sync |
| OCR | Đầu vào, tiến trình, xem trước chữ, lỗi, thử lại | Engine OCR / trích xuất chữ |
| PDF/Office | Viewer, luồng xử lý, fallback | Parser, converter, viewer tích hợp |
| Markdown/LaTeX | Toolbar, render inline, UI báo lỗi | Parser, editor local, KaTeX/MathJax |
| Thông báo | Mock native, trạng thái đặt/sửa/báo lại | Bộ lập lịch của OS, quyền |

**Không** đánh dấu "chức năng hoàn chỉnh" nếu mới có UI tĩnh. **Không** khẳng định "đã đồng bộ / đã nhận thông báo / code chạy được" chỉ vì có frame Figma.

### 0.3 Không được tự bịa
- Giới hạn dung lượng PDF/DOCX/XLSX/PPTX/audio/video (ngoài giới hạn ảnh: 10 MB/ảnh, 10 ảnh/lần).
- Engine OCR trên iOS/Android/Web, độ chính xác, khả năng offline thực tế.
- Độ trung thực khi chuyển Office/PDF phức tạp sang Markdown.
- Apple Sign-in, khôi phục bằng sinh trắc học, chính sách khoá note khi chưa quyết định.

Khi thiếu quyết định kỹ thuật: **thiết kế trạng thái chuẩn và ghi `IMPLEMENTATION DEPENDENCY`**; không tự cắt tính năng khỏi phạm vi; không khẳng định đã thực thi.

### 0.4 Về nhãn ưu tiên Must/Should/Could
Các bản cũ có nhãn Must/Should/Could. Trong bản này, **phạm vi thiết kế UI prototype không được cắt tính năng bằng nhãn ưu tiên**. Nếu hạn chế OS/engine làm thay đổi UI (ví dụ split view trên điện thoại), phải nêu phương án tương đương và lý do. Nhãn ưu tiên chỉ còn ý nghĩa với kế hoạch triển khai kỹ thuật (xem Phụ lục A).

---

## 1. TỔNG QUAN SẢN PHẨM

**Opportunities** là ứng dụng ghi chú dành cho người dùng phổ thông lẫn chuyên sâu. Nhiệm vụ chính: **ghi nhanh → tổ chức → tìm lại → hành động/nhắc nhở → chia sẻ/xuất**, mà không cần học Markdown trước.

**Mục tiêu:** ghi lại nhanh mọi thông tin (chữ, giọng nói, ảnh, tài liệu scan, hình vẽ, bảng), sắp xếp rõ ràng, tìm lại dễ, nhắc đúng lúc, dùng liền mạch giữa điện thoại và máy tính.

**Người dùng:** từ người ít dùng công nghệ đến người dùng thành thạo.

### 1.1 Nguyên tắc chất lượng
1. Người dùng luôn biết mình đang ở đâu và thao tác sắp xảy ra điều gì.
2. Nội dung note là trọng tâm, không phải toolbar hay hiệu ứng.
3. Không bắt chọn lại cùng một quyết định hai lần.
4. Chỉ có màn hình riêng khi cần diện tích, quyền truy cập hoặc trạng thái nghiệp vụ riêng.
5. Không mất dữ liệu vì đóng app, rớt mạng, import lỗi, xung đột sync.
6. Tiếng Việt chính xác, chuyên nghiệp; không có câu văn nói/caption vô nghĩa cạnh nút.
7. Mobile và Web tương thích về chức năng nhưng không sao chép y nguyên desktop lên điện thoại.
8. Thiết kế vừa cho người chỉ bấm nút, vừa cho người gõ Markdown/LaTeX.
9. Mọi chức năng có: điểm vào, trigger, trạng thái/kết quả, undo/huỷ/khôi phục, kiểm thử.
10. **Ít màn hình điều hướng nhất, không phải ít chức năng nhất.**

### 1.2 Phạm vi (In scope)
| Nhóm | Chức năng |
|---|---|
| **Nền tảng (C)** | C1–C10: tài khoản, hướng dẫn lần đầu, CRUD note, autosave, undo/redo, thùng rác, lưu trữ, quyền thiết bị, trạng thái rỗng/lỗi/tải, cài đặt |
| **A. Ghi lại** | F1 Speech-to-text · F2 Chụp ảnh · F3 Scan + OCR · F4 Checklist · F5 Bảng vẽ · F6 Template & bảng tính |
| **B. Tổ chức** | F7 Folder · F8 Tag · F9 Ghim · F10 Màu note · F11 Backlink/Wiki-link |
| **C. Xem & tìm** | F12 Grid/List/Calendar · F13 Search · F14 Sort · F15 Quick filter |
| **D. Nhắc nhở** | F16 Notification theo lịch |
| **E. Chia sẻ & đồng bộ** | F17 Chia sẻ · F18 Xuất PDF/Markdown/Backup · F19 Đồng bộ & offline |
| **F. Trải nghiệm** | F20 Focus mode · F21 Cỡ chữ · F22 Light/Dark |
| **G. Markdown** | F23 Markdown đầy đủ + LaTeX + gợi ý cú pháp (23.1–23.10) |
| **Mở rộng** | F24 Kéo thả/Dán · F25 Đính kèm & xem trước · F26 Nhập tài liệu/OCR/chuyển đổi · F27 Liên kết/Deep link · F28 Chỉnh khối · F29 Web clipper · F30 Thư viện tệp · F31 Lịch sử phiên bản · F32 Chú thích ảnh/PDF · F33 Tiện ích hằng ngày |
| **Bổ sung P1/P2** | Khoá note, widget, ký tên, bảng→biểu đồ, ghi âm gắn chữ, flashcard, QR, publish, backup… (hợp nhất vào các F, mục 10) |

### 1.3 Ngoài phạm vi (Out of scope)
- Tính năng AI (tóm tắt, tự phân loại, chatbot…).
- Chỉnh sửa cùng lúc nhiều người theo thời gian thực.
- Nhắc nhở theo vị trí; ngày âm lịch.
- Gói trả phí / paywall.

---

## 2. CÁC QUYẾT ĐỊNH ĐÃ CHỐT & XUNG ĐỘT ĐÃ GIẢI QUYẾT

| Chủ đề | Phiên bản cũ | **Quy định cuối cùng** |
|---|---|---|
| **Onboarding (C2)** | 3 màn giới thiệu | **Không có 3 màn welcome chặn dùng app.** Thay bằng hint/coach marks có nút *Bỏ qua* ngay trong Home lần đầu + 1 note mẫu. Đây là thay đổi UX chủ ý, vẫn đạt mục đích hướng dẫn. |
| **Simple vs Markdown** | Có bước chọn kiểu soạn thảo | **Một editor duy nhất.** Live Preview mặc định; Source / Reading chuyển ngay trong editor. Không có "hai loại note". |
| **Lưu** | Có nút Lưu | **Autosave** 1 giây sau khi ngừng gõ và khi rời ngữ cảnh. **Không có nút Lưu** cho sửa note thường. Nút xác nhận chỉ dùng cho import / chuyển đổi / hành động phá huỷ. |
| **Dạng xem** | Sheet "Hiển thị ghi chú" gom mọi thứ; sau đó ViewSwitcher ngay trên Home | **Một Home/Collection Workspace.** Grid/List/Calendar là *state*, đổi qua menu `⋯` của Home (góp ý GV 01/11/2026). |
| **Thanh dưới (góp ý GV 01/11/2026)** | Ghi chú · Tìm kiếm · + · Nhắc nhở · Thư viện | **Ghi chú · Ôn tập · + · Nhắc nhở · Hồ sơ.** Tìm kiếm mở từ icon 🔍 trên Home và màn Thư mục. Home có đầu trang thoáng kiểu Samsung Notes (tiêu đề giữa, hàng ☰ · giao diện · 🔍 · ⋯). Menu ☰ gồm Tất cả ghi chú, Đã ghim, Thùng rác, thư mục, Thẻ, Quản lý thư mục (mở màn Thư mục riêng kiểu Apple Notes). Hồ sơ gộp toàn bộ Cài đặt (không còn màn Cài đặt riêng), nội dung cuộn dọc; CÀI ĐẶT KHÁC gồm Tệp đính kèm, Mẫu, Lưu trữ. Sắp xếp và Lọc ở menu `⋯`. Các mục 4.7 và 14 đọc theo quy định này khi còn ghi cấu trúc cũ. |
| **Chế độ editor (01/11/2026)** | Xem trước, Mã Markdown, Chỉ đọc, Tập trung nằm trong menu `⋯` của ghi chú | **Gom vào dropdown của ô chế độ** trên thanh đầu editor, cùng một cấp; mục đang dùng có dấu tích. Menu `⋯` của ghi chú không còn các mục này. |
| **Tạo mới (góp ý GV 01/11/2026)** | Sheet + chỉ có camera/scan trực tiếp | Thêm **Scan ảnh từ máy**: chọn ảnh/PDF có sẵn → xem lại chữ nhận dạng → tạo ghi chú. |
| **Toán/LaTeX** | Nhiều trang riêng cho ký hiệu/công thức | **Công cụ inline/contextual trong editor.** Workspace riêng chỉ cho đồ thị/vẽ thật sự phức tạp. |
| **Màu** | Nhiều theme cũ (Ink/Warm Paper/Mint/Stone) | **Color System v3 lấy cảm hứng Obsidian** (mục 11). Không dùng token cũ. |
| **60–30–10** | Từng nhầm Light/Dark là hai màu trong một tỷ lệ | Áp dụng **trong từng theme**: dominant neutral 60 / secondary neutral 30 / violet accent ≤ ~10. |
| **Ctrl+K** | Vừa Search toàn app vừa Insert Link | **Ngoài editor: Tìm kiếm. Trong editor khi có vùng chọn: tạo/sửa liên kết.** Tooltip & bảng phím tắt hiển thị rõ theo ngữ cảnh. |
| **Độ ưu tiên task `!` / `!!`** | Cú pháp ưu tiên trong Markdown | Hỗ trợ như metadata legacy khi nhập; **không** tạo màn hình/chỉ mục ưu tiên mới. |
| **"Scan DOCX/PDF"** | Dễ gọi chung là scan | **Đính kèm / Xem trước / Trích chữ (OCR) / Chuyển Markdown là 4 tác vụ khác nhau.** Không xử lý DOCX như ảnh nếu chưa render. |
| **Welcome, chọn loại note, trang "Đã lưu"** | Có trong mockup cũ | **Bị xoá.** |
| **Nhãn Must/Should/Could** | Dùng để cắt tính năng | Không dùng làm lý do bỏ chức năng trong thiết kế (xem 0.4). |

---

## 3. MÔ HÌNH DỮ LIỆU & MÔ HÌNH SOẠN THẢO MARKDOWN-FIRST

### 3.1 Cấu trúc dữ liệu một Note
| Trường | Quy tắc |
|---|---|
| `noteId` | Định danh **ổn định, bất biến**; đổi tên/di chuyển không làm gãy liên kết |
| **Tiêu đề** | = **dòng đầu tiên** của note (hiểu là Heading 1). Tối đa **200 ký tự**; trống → hiển thị "Ghi chú không tên" |
| **Nội dung** | Markdown thuần, từ dòng 2 trở đi; lưu thành file `.md` |
| **Thuộc tính (Properties)** | YAML frontmatter (tuỳ chọn): `tags`, `color`, `pinned`, `folder`, `aliases`, `remind`… Đồng bộ **2 chiều** với giao diện |
| **Tệp đính kèm** | File nhị phân trong `attachments/`, chèn bằng cú pháp `![[ten-file]]`; **không** nhúng base64 vào `.md` |
| **Folder** | 1 folder/note; mặc định "Chưa phân loại"; lồng tối đa 2 cấp |
| **Tag** | 0–10 tag/note; ≤ 30 ký tự/tag; không dấu cách; hỗ trợ tag lồng `#a/b` |
| **Màu note** | 1 trong 8 màu hoặc Không màu |
| **Ghim** | Có/Không |
| **Nhắc nhở** | 0–5/note, cộng nhắc nhở riêng cho từng mục checklist |
| **Liên kết / Backlink** | Cạnh có kiểu (URL / note / heading / block) trỏ tới đích ổn định; tự cập nhật |
| **Ngày tạo / sửa** | UTC, hiển thị theo múi giờ cục bộ |
| **Trạng thái** | Bình thường / Lưu trữ / Thùng rác (giữ 30 ngày) |
| **Trạng thái lưu & đồng bộ** | Tách riêng *đã lưu trên máy* và *đã đồng bộ* |
| **Phiên bản** | Tham chiếu snapshot khôi phục; bản xung đột được giữ nguyên |
| **Khoá** | Chính sách PIN/sinh trắc; quy tắc xem trước/chỉ mục/chia sẻ cho note khoá cần quyết định riêng |

### 3.2 Cấu trúc file note
```markdown
---                                   ← (tuỳ chọn) Thuộc tính – người dùng thường không thấy
tags: [hoc-tap, hci]
color: vang
pinned: true
remind: 2026-01-12 20:00
---
Bài 5 – HCI: Persona                  ← Dòng 1 = TIÊU ĐỀ
Nội dung bắt đầu từ dòng 2...         ← Dòng 2+ = NỘI DUNG
```

### 3.3 Một editor, hai kiểu người dùng
| | Người dùng thường | Người dùng Markdown |
|---|---|---|
| Cách gõ | Gõ chữ bình thường: dòng 1 là tiêu đề | Gõ cú pháp Markdown trực tiếp |
| Định dạng bằng | Thanh công cụ (B, I, danh sách, bảng, ảnh…) – app tự chèn cú pháp | Cú pháp + phím tắt + menu `/` |
| Thấy cú pháp? | **Không** (Live Preview ẩn ký hiệu) | Có: ký hiệu hiện ở dòng đang sửa, hoặc chế độ Source |
| Tag/màu/ghim/nhắc | Bấm nút giao diện | `#tag`, frontmatter, `⏰` |
| Kết quả lưu | **Cùng một file `.md`** | **Cùng một file `.md`** |

> Văn bản thuần đã là Markdown hợp lệ nên người dùng thường không cần học gì.

### 3.4 Ba chế độ hiển thị (nút chuyển trên toolbar; Web: **Ctrl+E**)
| Chế độ | Mô tả | Mặc định cho |
|---|---|---|
| **Live Preview** (*Xem trước*) | Gõ đến đâu hiển thị đẹp đến đó; ký hiệu chỉ hiện ở dòng con trỏ | Mọi người dùng |
| **Source** (*Mã Markdown*) | Toàn bộ mã, tô màu cú pháp, có số dòng (web) | Người dùng Markdown |
| **Reading** (*Chỉ đọc*) | Không sửa nhầm; dùng khi xem lại, trình chiếu, mở link chia sẻ | Khi mở từ link chia sẻ |

Đổi chế độ **không** tạo màn hình mới, note ID mới, file mới hay bản nháp trùng; **không** xoá lịch sử undo.

### 3.5 Đồng bộ 2 chiều giữa giao diện và cú pháp
- Bấm **Ghim** → frontmatter thêm `pinned: true`; xoá dòng đó → bỏ ghim.
- Gõ `#hoc-tap` → tag xuất hiện ở sidebar; thêm tag bằng nút → cũng ghi vào note.
- Chọn màu/folder/nhắc nhở bằng giao diện → ghi frontmatter; sửa frontmatter → giao diện cập nhật.
- Frontmatter mặc định **thu gọn** thành một hàng "Thuộc tính"; không hiện YAML thô cho người dùng thường trừ khi bật Source/Properties.

---

## 4. KIẾN TRÚC GIAO DIỆN & LUẬT CHỐNG MÀN HÌNH THỪA (BẮT BUỘC)

### 4.1 Định nghĩa các loại bề mặt
| Loại | Khi dùng | Ví dụ đúng |
|---|---|---|
| **Screen** | Một mục đích làm việc chính hoặc quyền truy cập riêng | Notes Home, Note Editor, Reminders, Library, Auth, Settings |
| **Workspace** | Tác vụ nhiều nội dung/công cụ đòi diện tích | Scan nhiều trang, PDF annotator, Drawing |
| **State** | Cùng màn hình/route, khác dữ liệu hoặc chế độ | Grid/List/Calendar; Live Preview/Source/Reading |
| **Inline** | Tác động ngay tại đối tượng | Checkbox, ô bảng, sửa tiêu đề, tag, link |
| **Context menu** | Hành động riêng của card/note/folder | Ghim, Chuyển, Nhân bản, Lưu trữ, Xoá |
| **Overflow `⋯`** | Tuỳ chọn phụ của **đúng màn hình hiện tại** | Kanban, Graph, Ghi chú hôm nay, Chọn nhiều |
| **Popover / Bottom sheet** | Quyết định ngắn tại chỗ | Sort/filter, folder/icon/màu, chọn giờ nhắc |
| **Dialog** | Cảnh báo rủi ro, chia sẻ công khai, xung đột | Xoá vĩnh viễn, thu hồi link, chuyển đổi mất định dạng |
| **System state** | Phản hồi trong ngữ cảnh | Loading/empty/offline/saved/error, snackbar |

**Bottom sheet không phải màn hình độc lập.** Trong Figma, thể hiện sheet **chồng lên màn hình gọi nó** (thấy nền parent, có nhãn trigger và cách đóng), hoặc tài liệu hoá như component variant. **Cấm** vẽ khung đen rỗng toàn màn hình để "cho có screen".

### 4.2 Thứ tự chọn bề mặt (rubric)
**Inline → hành động ngữ cảnh → toolbar/segmented → popover/sheet → dialog → workspace/screen riêng.** Chọn bề mặt *đầu tiên đủ dùng*. Không bắt đầu bằng "tạo screen" chỉ vì chức năng có trong danh sách.

### 4.3 Bài kiểm định trước khi thêm bất kỳ frame mới
Phải trả lời đủ 6 câu:
1. Người dùng kích hoạt từ phần tử nào, ở màn hình nào?
2. Đây là route/screen mới hay state/overlay của màn hình cũ?
3. Bỏ frame này thì mất nghiệp vụ hay ngữ cảnh nào?
4. Có thể thao tác trực tiếp trong editor/toolbar/`⋯`/sheet nhỏ không?
5. Sau Apply/Cancel/Back người dùng về đâu; có giữ scroll/focus/caret không?
6. Vì sao phải chiếm toàn màn hình (dữ liệu phức tạp / lý do OS)?

Không chứng minh được → **không tạo screen mới**; ghi nhận state/interaction ngay trong frame hiện tại.

### 4.4 Các cấu trúc bị cấm
- `Welcome → Chọn kiểu soạn thảo → Bắt đầu → Editor`.
- `Ghi chú mới → hàng loạt nút chọn chức năng → mới được nhập`.
- Trang riêng chỉ để đổi Sort, Filter, Tag, Icon, Màu, Cỡ chữ, giờ nhắc đơn giản.
- Sheet "Hiển thị ghi chú" nhét Grid/List/Calendar + Sort + Filter + Settings + Kanban + Graph.
- Tách Grid/List/Calendar thành ba "mini-app" điều hướng riêng.
- Chèn 5–8 nút full-width liên tiếp giữa trang trống khi đã có toolbar/menu phù hợp.
- Nút **Mở** toàn chiều ngang trên mỗi card (card tự mở khi chạm).
- Gắn `Back` ở mọi bước dù không đổi ngữ cảnh điều hướng.
- Lặp trạng thái sync trên từng card; trang "Lưu thành công".
- Panel thuộc tính của note A cạnh editor của note B (dữ liệu demo lệch ngữ cảnh).
- Chuỗi full-screen "Công cụ soạn thảo / Chèn công thức / Bảng ký hiệu / LaTeX" không cần thiết.
- Footer action cách form 3 trường hàng trăm pixel.
- Grid giấy làm nhiễu văn bản/bảng; bảng "chìm" vì cell cùng màu nền.
- Overlay là khối đen trống với UI nhỏ trôi ở góc, không thấy màn hình gốc.

### 4.5 Ví dụ lỗi thực tế (Figma 27/10) và thiết kế đúng
**Lỗi:** `01 · Entry & browse` có một Mobile Home nhỏ bên trái và nhiều khung đen bên phải chứa các sheet rời: *Context Menu · Tùy chọn ⋯*, *Sắp xếp ghi chú*, *Thao tác ghi chú*, *Tùy chọn lịch*; header/card/text chồng và bị cắt.

**Thiết kế đúng:**
- **Home** giữ danh sách/grid chính; phía trên có Search, View Switcher gọn, Sort/Filter, nút `⋯` cho tuỳ chọn hiếm dùng; bottom nav chỉ gồm destination thật.
- **Grid/List/Calendar:** chạm icon hoặc segmented switcher → thay danh sách tại chỗ; Calendar (tháng/tuần + ngày được chọn) nằm **trong cùng Home**.
- **Sort:** một sheet gọn liệt kê tiêu chí + Tăng/Giảm; chọn xong đóng và áp dụng ngay. Nếu đặt trong `⋯` vì thiếu chỗ: tối đa *một lần mở menu + một lựa chọn*; không sheet lồng sheet.
- **`⋯` trên Home:** Kanban, Graph, Ghi chú hôm nay, Chọn nhiều… (thao tác phụ của collection). Không chứa Settings nếu Settings đã ở Library.
- **`⋯` trên từng card:** Ghim, Chuyển thư mục, Nhân bản, Chia sẻ, Lưu trữ, Xoá. **Không lẫn** với `⋯` của Home.
- **Tuỳ chọn Calendar:** Tháng/Tuần và lọc theo Ngày tạo/sửa/nhắc nằm ngay trên toolbar Calendar, không có màn chọn lịch riêng.
- **Figma:** sheet minh hoạ phải chồng lên *bản sao đúng màn hình Home đang dùng*, có chú thích trigger/đóng.

### 4.6 Hệ nút toàn cục
| Loại | Dùng cho |
|---|---|
| **Primary** (violet đặc) | Hành động cốt lõi; hiếm khi quá một CTA chính trong một vùng |
| **Secondary** (neutral đặc/viền) | Lựa chọn hỗ trợ, xem trước, huỷ |
| **Tertiary / Ghost** | Quay lại, bỏ chọn, xem thêm, hành động ít ưu tiên |
| **Icon button** | Toolbar; có tooltip/accessible label; vùng chạm đủ lớn |
| **Segmented** | View/mode loại trừ nhau |
| **Dropdown / Sheet** | Sort/filter/tuỳ chọn |
| **Card / Row** | Tự chạm để mở; **không** lặp nút "Mở" |
| **Toggle / Radio / Checkbox** | Boolean / độc quyền / nhiều lựa chọn |

Không kéo nút 100% chiều rộng cho thao tác nhỏ nếu không có lý do tiếp cận trên mobile.

### 4.7 Kiến trúc Mobile
**Bottom navigation:** `Ghi chú` · `Tìm kiếm` · `+` (Tạo) · `Nhắc nhở` · `Thư viện`.
- **Ghi chú:** Home/Collection — mục Đã ghim + Gần đây/Tất cả, chip filter, view selector, sort, menu card.
- **Tìm kiếm:** ô tìm auto-focus, kết quả inline, lịch sử tìm; chọn kết quả mở note.
- **`+`:** *hành động tạo*, không phải destination trống. Mở compact sheet: nút **Ghi chú** chính + Checklist / Giọng nói / Ảnh / Scan / Bản vẽ / Từ mẫu. Chọn là đi thẳng tới công cụ, không hỏi lại.
- **Nhắc nhở:** nhóm theo hạn, xem/hoàn thành/báo lại, dẫn vào note/task.
- **Thư viện:** Folder, Tag, Tệp, Mẫu, Lưu trữ, Thùng rác, Cài đặt/Tài khoản. Không có trang trung gian "chọn loại thư viện".

**Các surface Mobile thật sự cần:**
| Mã | Surface | Ghi chú |
|---|---|---|
| M-A | Notes Home/Collection | Grid/List/Calendar/Kanban/Graph là state |
| M-B | Note Reader/Editor | Reading/Live Preview/Source là state; toàn màn hình |
| M-C | Search | Destination ở bottom nav + sheet filter nâng cao |
| M-D | Reminders/Tasks | Nhóm theo hạn |
| M-E | Library | Folder/Tag/Tệp/Mẫu/Lưu trữ/Thùng rác là section, không mỗi mục một trang |
| M-F | Settings & Account | Một hierarchy gọn |
| M-G | Authentication | Login/Register/Reset là state; không welcome |
| M-H | Public Share Viewer | Route chỉ đọc cho người nhận |
| M-I | Document Processing | Scan/PDF/OCR/chuyển đổi nhiều trang |
| M-J | Drawing/Annotation | Canvas toàn màn hình |

**Home:** tiêu đề rõ; icon Search và `⋯` phân biệt; View/Sort/Filter gần header collection; chip cuộn ngang *trong container đúng*; không để control dưới bottom nav hoặc đè status bar.
**Editor:** Back về đúng collection/filter/scroll; tiêu đề = dòng 1; thanh định dạng bám bàn phím; menu chèn ngữ cảnh; chỉ báo autosave nhỏ; giấy note rõ, lưới rất nhẹ.

### 4.8 Kiến trúc Web
| Mã | Surface | Mô tả |
|---|---|---|
| W-A | Main Workspace (`/app`) | Sidebar (All Notes, Pinned/Favorites, Folders, Tags, Reminders, Files, Archive, Trash) · Collection (Grid/List/Calendar/Kanban/Graph) · Editor. Thu gọn sidebar, mở rộng editor, split view, focus mode |
| W-B | Editor focus/fullscreen | Cùng route/state; chỉ editor + toolbar ngữ cảnh |
| W-C | Document Workspace | Overlay toàn màn hình: preview PDF/Office/scan, thumbnail trang, tìm trong file, OCR, annotation, trích xuất/chuyển Markdown. Nút Đóng về đúng note + caret |
| W-D | Drawing Workspace | Canvas, công cụ vẽ, sửa ảnh/scan, lớp không phá file gốc |
| W-E | Settings & Account | Một khu vực nhiều tab: hồ sơ, bảo mật/khoá, thiết bị, theme, font, thông báo, sync, import/export, phím tắt, trợ giúp |
| W-F | Authentication | Login/Register/Reset trong cùng layout; không welcome |
| W-G | Public Share Viewer | Route công khai chỉ đọc, không cần đăng nhập |

**Không tạo screen:** welcome; chọn Simple/Markdown; trang "note mới" thứ hai; chọn template lần hai; trang thành công sau lưu; trang filter/sort/property/folder/tag độc lập.

**Hợp đồng bố cục Web** (điểm khởi đầu, điều chỉnh theo độ phân giải):
| Loại | Cột | Mục đích |
|---|---|---|
| Notes workspace | Sidebar 220–300 / Collection 280–420 / Editor co giãn | Duyệt và đọc/viết đồng thời; list có thể thu gọn |
| Grid collection | Sidebar 220–300 / Grid co giãn | Duyệt card; không ép panel editor rỗng |
| Editor focus | Editor ~650–900 px, căn giữa | Viết lâu, ít phân tâm |
| Inspector | Tuỳ chọn ~280–360 | Chỉ bật khi có properties/backlinks/metadata thật |
| Form ngắn | Dialog/sheet 360–520 | Sửa folder/nhắc/tag; không kéo form 3 trường ra toàn viewport |
| Bảng dữ liệu | Rộng đủ, cuộn ngang nếu cần | Ranh giới hàng/cột rõ |
| Calendar | Mở rộng toàn content | Có chỗ xem lịch và sự kiện |

Mỗi frame Web cần chú thích: `Name · Purpose · Entry · Surface · Layout (tỉ lệ + min/max width) · Why each region exists · Dismiss/return · Responsive behavior · Dependencies/edge states`. **Không tạo cột trống chỉ để đạt tỉ lệ số học** (ví dụ 1:2:1); không phóng to mockup 1440×900 theo tỉ lệ.

---

## 5. LUỒNG NGHIỆP VỤ CHUẨN ĐẦU-CUỐI

| # | Entry → Hành động → Kết quả → Quay lại | Không được chen vào |
|---|---|---|
| 01 | Mở app (đã đăng nhập) → Home → chạm note → Reader/Editor → Back giữ scroll | Welcome; chọn loại note |
| 02 | Home `+` → Ghi chú → editor focus tiêu đề → gõ → autosave → Back | Trang "ghi chú mới" nhiều CTA; nút Lưu |
| 03 | `+` → Checklist → editor có sẵn checklist → tick → autosave | Chọn "Checklist" lần hai |
| 04 | View Switcher → Grid/List/Calendar → chạm item → Note → Back đúng view | Overlay "Hiển thị ghi chú" |
| 05 | Sort/Filter gọn → chọn → Home cập nhật → xoá lọc | Trang Sort riêng; filter wizard |
| 06 | Editor chèn file/dán → card/preview → OCR/convert khi chọn → chèn tại caret | Bắt scan mọi file; quên giữ file gốc |
| 07 | Chọn chữ/ảnh → Link picker → URL/Note/Heading → áp dụng inline | Trang giới thiệu hyperlink |
| 08 | Task → Reminder picker → ngày/giờ/lặp → áp dụng → trạng thái inline | Trang "nhắc thành công" |
| 09 | Menu note → Chia sẻ/Xuất → xem trước/tuỳ chọn → OS save/share → note | Trang success; lộ link riêng tư |
| 10 | Phát hiện xung đột sync → banner không chặn → so sánh → giữ cả hai/gộp → note | Tự đè bản chưa chọn |
| 11 | Library → folder/tag/tệp → tác vụ tại chỗ → quay lại | Mỗi lần sửa icon/màu một trang |
| 12 | `+` → Từ mẫu → xem trước → Dùng → editor đã điền → autosave | Chọn lại template khi đã vào editor |
| 13 | Thả screenshot/PDF/DOCX vào editor → screenshot nhúng ngay; PDF/DOCX hiện lựa chọn ngắn → giữ file gốc | Wizard nhiều bước |

**Quy tắc tạo note nhanh:** Web — một click `+` mở editor trống, focus tiêu đề; menu mở rộng cạnh `+` để chọn checklist/scan/voice/drawing/template và đi thẳng tới công cụ. Mobile — `+` mở compact sheet, nút Ghi chú tạo ngay.

**Quy tắc xác nhận:** chỉ xác nhận hành động phá huỷ, công khai dữ liệu, thay đổi bảo mật, hoặc chuyển đổi có thể mất định dạng.

---

## 6. CHỨC NĂNG NỀN TẢNG C1–C10

### C1 — Tài khoản, xác thực, bảo mật
- Đăng ký/đăng nhập bằng **Email + mật khẩu** hoặc **Google**; quên/đặt lại mật khẩu qua email; đăng xuất; xem phiên/thiết bị; xoá tài khoản.
- Mật khẩu **≥ 8 ký tự**; lỗi email/mật khẩu hiện inline; luồng khôi phục không tiết lộ tài khoản có tồn tại hay không khi ảnh hưởng bảo mật.
- Session hết hạn: **giữ bản nháp local**, giải thích, đăng nhập lại rồi quay đúng note.
- Xoá tài khoản = xác nhận mạnh, nêu rõ phạm vi dữ liệu bị xoá (toàn bộ), tách khỏi luồng xoá note.
- Khoá note PIN/sinh trắc: ở menu note / Settings → Bảo mật; có phương án dự phòng an toàn; không coi sinh trắc là bắt buộc trên mọi nền tảng.
- **Mobile:** auth shell có login/register/reset là state; không welcome trước auth; quản lý ở Thư viện → Cài đặt → Tài khoản. **Web:** route auth riêng; phiên/thiết bị trong Settings.
- **Lỗi:** offline đăng nhập lần đầu, session hết hạn, sai mật khẩu, huỷ Google auth, note bị khoá, thiết bị không hỗ trợ sinh trắc.
- **Chấp nhận:** đăng nhập thành công → Home; mở lại từ link riêng có quyền đúng; lỗi auth không xoá draft.

### C2 — Hướng dẫn lần đầu (thay Onboarding)
- Sau đăng nhập lần đầu: hint **không chặn** về `+`, tìm kiếm, editor; 1 note mẫu hướng dẫn (gồm hướng dẫn Markdown) mở được; có **Bỏ qua**.
- Không 3 màn welcome; không bắt chọn Simple/Markdown; không hiện lại mỗi lần mở app; xem lại được từ Trợ giúp.

### C3 — Tạo / đọc / sửa / xoá note
- `+` luôn dễ truy cập ở Home. Tiêu đề = dòng 1 (≤ 200 ký tự), Enter xuống nội dung dòng 2; tiêu đề trống → "Ghi chú không tên", không ép đặt tiêu đề.
- Một editor, không chọn loại note sau khi mở.
- Card/dòng danh sách là vùng chạm trực tiếp; chọn nhiều để chuyển/lưu trữ/xoá/xuất.
- Xoá → Thùng rác + snackbar Hoàn tác; **Nhân bản** tạo bản sao mới (chính sách tham chiếu/đính kèm rõ), **Sao chép sang folder**; không ghi đè bản gốc.
- **Chấp nhận:** Home → tạo → nhập → quay lại → note ở Home; mở lại đủ nội dung; không có màn chọn lại loại note.

### C4 — Tự động lưu & trạng thái đồng bộ
- Debounce **1 giây** sau khi ngừng gõ; lưu thêm khi back, blur, app xuống nền, trước khi đóng (nếu runtime cho phép). **Không có nút Lưu.**
- Trạng thái nhỏ: `Đang lưu…` · `Đã lưu trên thiết bị` · `Đang đồng bộ…` · `Đã đồng bộ` · `Chờ đồng bộ` · `Lỗi đồng bộ — Thử lại`.
- Không đồng nhất "đã lưu máy" với "đã đồng bộ"; không phủ dải tím lớn; không lặp trên từng card.
- Offline vẫn có bản nháp local; lỗi server không xoá thay đổi, có thử lại.
- **Chấp nhận:** gõ nhanh rồi rời note → còn nguyên; mất mạng → trạng thái local đúng; có mạng → tự đồng bộ.

### C5 — Undo / Redo
- Tối thiểu **50 bước**, gồm text, checklist, bảng, thao tác khối, chèn đính kèm (khi đảo ngược được).
- Web: **Ctrl+Z / Ctrl+Shift+Z**; Mobile: nút Undo/Redo trên toolbar + gesture native nếu có.
- Đổi chế độ Live/Source không xoá lịch sử; không còn bước → thông báo rõ.

### C6 — Thùng rác & khôi phục
- Xoá → Thùng rác **30 ngày**; snackbar "Đã chuyển vào Thùng rác · Hoàn tác" hiện **5 giây**.
- Thùng rác ở Library/Sidebar; chọn nhiều để Khôi phục / Xoá vĩnh viễn; **xoá vĩnh viễn có xác nhận** nêu hậu quả.
- Khôi phục về folder cũ nếu còn, nếu không về "Chưa phân loại" kèm thông báo.
- Liên kết tới note đã xoá hiện trạng thái gãy + tuỳ chọn khôi phục.

### C7 — Lưu trữ (Archive)
- Ẩn khỏi Home thường, không xoá nội dung; vuốt/menu card hoặc menu note. Xem/mở/bỏ lưu trữ ở Thư viện → Lưu trữ. Search/filter nhận biết trạng thái. Không có trang xác nhận trung gian.

### C8 — Quyền thiết bị/trình duyệt
- Chỉ xin quyền khi **dùng thật** mic/camera/thông báo/thư viện ảnh/tệp. Giải thích ngắn trong luồng rồi mới gọi prompt native.
- Bị từ chối → fallback (ví dụ chọn file) + hướng dẫn mở Cài đặt OS. Không xin đồng loạt khi onboarding. Rõ ba trạng thái: denied / permanently denied / granted. Không giả layout thông báo native sai chuẩn.

### C9 — Trạng thái rỗng / đang tải / lỗi / offline
- Mọi danh sách (Home, Search, Template, Tệp, Nhắc nhở, Thùng rác, Folder) có empty/loading/error **nằm trong vùng nội dung**, không tạo screen mới.
- Empty có hành động trực tiếp (`Tạo ghi chú`, `Xoá bộ lọc`). Lỗi API/upload/OCR theo từng mục + **Thử lại/Huỷ**; bảo vệ draft. Skeleton không làm nhảy layout. Banner/toast offline không chặn editor.

### C10 — Cài đặt
- Một khu vực, chia nhóm: Tài khoản · Bảo mật · Giao diện · Editor/Giấy · Thông báo/Giờ im lặng · Đồng bộ/Thiết bị · Nhập/Xuất/Sao lưu · Thùng rác · Trợ giúp/Giới thiệu · (Đang chia sẻ).
- Theme Sáng/Tối/Hệ thống tức thì; cỡ chữ + xem trước; kiểu giấy Ô vuông/Kẻ dòng/Trơn; bật/tắt gợi ý Markdown; chế độ mặc định; font code riêng.
- Không tạo trang cho từng toggle; tuỳ chỉnh đơn giản = radio/segmented/toggle inline.

---

## 7. CHỨC NĂNG F1–F22

### F1 — Speech-to-text & ghi âm
- **Điểm vào:** nút mic trên toolbar editor; `+ → Giọng nói`. Xin quyền micro khi dùng lần đầu.
- Hỗ trợ **Tiếng Việt & Tiếng Anh**, chọn ngôn ngữ 1 chạm. Giao diện ghi: sóng âm, **chữ hiện trực tiếp**, nút Tạm dừng / Dừng / Huỷ.
- Chữ chèn tại **vị trí con trỏ**, sửa được như chữ thường. **Tự dừng** sau 5 giây im lặng hoặc tối đa 5 phút/lượt. Dùng nhận dạng giọng nói có sẵn của OS/trình duyệt.
- Tuỳ chọn **lưu file ghi âm gốc** + trình phát inline; **audio timestamp** (P2.4): vừa ghi vừa gõ, chạm một dòng → phát đúng đoạn lúc gõ dòng đó. Không ép tạo note mới khi đang ghi trong note.
- **Lỗi:** chưa cấp micro (màn giải thích + nút "Cấp quyền"), "Không nghe rõ, vui lòng thử lại", thiết bị không hỗ trợ, offline không có nhận dạng. Không mất nội dung đang gõ. Không tự upload audio gốc trái phép.
- **Chấp nhận:** nói 1 câu tiếng Việt → chữ đúng vị trí con trỏ trong ≤ 2 giây sau khi dừng (khi engine đáp ứng).

### F2 — Chụp ảnh & khối ảnh
- Nguồn: **Máy ảnh** và **Thư viện** (mobile); **Tải lên/kéo thả/dán** (web). Chọn nhiều, **tối đa 10 ảnh/lần**. Định dạng JPG, PNG, HEIC, WEBP; **≤ 10 MB/ảnh**, tự nén có thông báo, không ghi đè ảnh gốc khi chỉnh.
- Ảnh lưu trong `attachments/`, chèn thumbnail tại caret; chạm mở lightbox toàn màn hình, vuốt/zoom; **Xoay, Cắt, Xoá, Tải về, Chú thích (caption), alt text**; gắn hyperlink (URL/note/heading/block/trang PDF); web có handle đổi cỡ, căn trái/giữa/phải.
- Offline: lưu local, đồng bộ sau; tiến trình/lỗi theo từng file.
- **Chấp nhận:** chụp 1 ảnh → hiện trong note; mở lại app vẫn còn.

### F3 — Scan văn bản & OCR
- **Mobile:** camera nhận diện mép giấy (khung viền nổi), chụp tự động/tay; chỉnh 4 góc; xoay; bộ lọc **Gốc / Xám / Đen trắng / Tăng nét**; scan nhiều trang; sắp xếp/xoá trang; lưu thành tài liệu nhiều trang (xem như PDF). Tên tự động theo ngày ("Scan 07/10/2026").
- **OCR:** Tiếng Việt có dấu + Tiếng Anh; nút **Sao chép chữ** và **Chèn chữ vào note**; chữ trong scan **tìm kiếm được**; kết quả theo trang; lỗi/thử lại từng trang.
- **Web:** không có camera scan; **tải lên ảnh/PDF** rồi xử lý + OCR tương tự.
- Không phá ảnh/PDF gốc. OCR offline chỉ khi engine local có thật; nếu không, thông báo cụ thể thay vì giả thành công.
- **Chấp nhận:** scan 1 hoá đơn 2 trang → note có tài liệu 2 trang; tìm được một từ trên hoá đơn bằng ô tìm kiếm.

### F4 — Checklist
- Chèn khối checklist vào note thường (`/checklist` hoặc menu); `+ → Checklist` chỉ là lối vào nhanh tới **cùng editor**. Lưu dạng `- [ ]` / `- [x]`.
- Enter = thêm mục; Backspace ở mục rỗng = xoá mục; tick/bỏ tick, mục tick **gạch ngang** và (tuỳ chọn) chuyển xuống cuối; **kéo thả** sắp xếp (có phương án thay thế bàn phím); thụt lề nhiều cấp (Tab/Shift+Tab hoặc nút/gesture).
- Tuỳ chọn: **Ẩn mục đã xong · Bỏ tick tất cả · Xoá mục đã xong**; chuyển Note thường ↔ Checklist.
- Card hiển thị tiến độ **"3/7"**; từng mục có hạn/nhắc nhở/lặp qua icon inline (xem F16, F23.6).
- Nhãn: checklist dùng "Hoàn thành/Đã hoàn thành"; **"Đã thu" chỉ dùng khi tiền đã nhận thật**.
- **Chấp nhận:** tạo 5 mục, tick 2 → card "2/5", mục đã tick gạch ngang; undo hoàn tác.

### F5 — Bảng vẽ (Drawing)
- Canvas riêng (đủ diện tích). Công cụ: **Bút, Bút dạ quang, Tẩy, Thước kẻ thẳng**; **8 màu**, **3 cỡ nét**; Undo/Redo; **Xoá hết có xác nhận**; zoom 2 ngón/pan; nền **Trắng / Kẻ ô / Kẻ dòng / Chấm**.
- Lưu **đối tượng bản vẽ có thể sửa** + thumbnail trong note (không chỉ bitmap); chạm mở lại để sửa tiếp. Tuỳ chọn lasso và lực nhấn bút. Ảnh/scan được chú thích qua F32.
- Không tách thành chuỗi màn hình "chọn công cụ".
- **Chấp nhận:** vẽ, đóng, mở lại → nét còn nguyên, sửa tiếp được.

### F6 — Template, Biểu mẫu, Bảng & tính toán
- **Điểm vào:** `+ → Từ mẫu` hoặc Library → Mẫu → lọc theo loại → **xem trước** → **Dùng mẫu** → editor đã điền sẵn. **Không hỏi lại mẫu** sau khi vào editor.
- **3 loại:** Hoá đơn · Biểu mẫu · Bảng thông dụng. Có mẫu dựng sẵn + **mẫu riêng** ("Lưu note này thành mẫu", sửa/xoá).

| Loại | Mẫu | Nội dung |
|---|---|---|
| Hoá đơn | Hoá đơn bán hàng | Tên khách, ngày, bảng (Tên hàng · SL · Đơn giá · Thành tiền = SL × Đơn giá), Tổng cộng |
| | Phiếu nhập hàng | Nhà cung cấp, ngày, bảng hàng nhập, tổng tiền, trạng thái thanh toán |
| Biểu mẫu | Biên bản họp | Ngày, thành phần, nội dung, quyết định, việc cần làm (checklist) |
| | Đơn/phiếu ghi thông tin | Trường: Chữ, Ngày, Lựa chọn, Checkbox |
| | Ghi chú bài giảng | Môn, buổi, nội dung chính, câu hỏi, tóm tắt |
| Bảng thông dụng | Thu – chi | Ngày · Nội dung · Thu · Chi · tự tính Số dư |
| | Sổ nợ | Tên · Số tiền · Ngày · Đã trả (checkbox) · Tổng còn nợ |
| | Danh sách đi chợ | Checklist + giá dự kiến + tổng |
| | Thời khoá biểu | Bảng Thứ × Tiết |
| | Kế hoạch tuần | 7 ngày × checklist |
| Thực hành | Học phí / Điểm danh | Mẫu bổ sung |

- **Khối bảng:** thêm/xoá/kéo thả hàng-cột; kiểu cột **Chữ, Số, Tiền (VNĐ), Ngày, Checkbox**; cột Số/Tiền có **dòng tổng** (Tổng, Trung bình, Đếm / SUM, AVG, COUNT, MIN, MAX); công thức `=B2*C2`, `=SUM(C2:C5)`; tiền VNĐ đúng định dạng, căn phải. Mobile: **cuộn ngang, cột đầu cố định** (khi khả thi).
- **Biến mẫu:** `{{date}}`, `{{time}}`, `{{title}}`, `{{so_hoa_don}}` (số hoá đơn tự tăng khi cấu hình rõ); xem trước biến đã giải; không để biến chưa giải mà không cảnh báo.
- **Mở rộng:** Bảng → Biểu đồ cột/tròn/đường liên kết dữ liệu bảng (P2.3); khối **Chữ ký** cho hoá đơn/phiếu, xuất PDF kèm chữ ký (P2.2).
- **Chấp nhận:** Hoá đơn, nhập (SL 2 × 10.000đ) + (SL 1 × 5.000đ) → **Tổng cộng 25.000đ**; sửa số tự tính lại; mobile không cắt cột khi cuộn.

### F7 — Folder
- Tạo / đổi tên / xoá / **đổi icon và màu**; lồng **tối đa 2 cấp**; mỗi note **1 folder**, mặc định "Chưa phân loại"; hiển thị **số note** mỗi folder.
- Di chuyển: menu "Chuyển tới…", chọn nhiều rồi chuyển; web **kéo thả** vào sidebar.
- `⋯` trên folder → Đổi tên / Đổi biểu tượng / Đổi màu / Di chuyển / Xoá trong **một sheet ngắn**, không form toàn màn hình, nút Lưu/Huỷ ngay cạnh trường nhập.
- Xoá folder → hỏi: **"Chuyển note sang Chưa phân loại"** hoặc **"Xoá cả note (vào Thùng rác)"**; có xác nhận.

### F8 — Tag
- Thêm bằng nút Tag hoặc gõ `#ten-tag`; gợi ý tag đã có; **≤ 10 tag/note, ≤ 30 ký tự, không dấu cách**; tag lồng `#hoc-tap/hci` hiện dạng cây ở sidebar.
- **Quản lý tag:** đổi tên (cập nhật mọi note, không tạo trùng), gộp, xoá, đổi màu. Bấm tag → hiện mọi note có tag đó.
- Mobile: ở Library → Tags hoặc sheet chọn từ Properties; không có màn "chọn tag" xen giữa luồng tạo note.

### F9 — Ghim
- Ghim/bỏ ghim từ card (nhấn giữ/menu/vuốt) hoặc trong note. Note ghim nằm trong mục **"Đã ghim"** ở đầu danh sách, **không bị Sort đẩy xuống**; hiệu lực theo từng folder (ghim ở folder nào hiện đầu folder đó và ở trang chủ). Icon ghim nhỏ trên card. Không tạo hai chức năng trùng nghĩa Pinned/Favorites.

### F10 — Màu note
- **8 màu + Không màu**; mỗi màu có biến thể Dark; chữ đạt **WCAG AA**. Đổi màu từ card/note/nhiều note. Màu dùng làm **bộ lọc** (F15). Dùng nền/viền/chip tuỳ kiểu card, không phủ chói cả editor; không dùng chung token với violet của UI.

### F11 — Liên kết note (Wiki-link, Backlink, Nhúng)
- Gõ **`[[`** → gợi ý note; note chưa có → **"Tạo note mới: …"**. Hiển thị chữ nổi bật, chạm mở note đích. Biến thể: `[[Note|Tên hiển thị]]`, `[[Note#Heading]]`, `[[Note#^id]]`; **aliases** trong frontmatter.
- Cuối note: mục thu gọn **"Được liên kết từ (n)"** — danh sách note trỏ tới + đoạn trích. Web có panel "Liên kết" bên phải. Không trang quản lý backlink riêng (trừ trạng thái Graph).
- **Đổi tên note → liên kết tự cập nhật** (ID ổn định). Đích bị xoá → liên kết **gãy** (gạch ngang, xám); chạm hỏi khôi phục hoặc tạo lại.
- **Nhúng** `![[Note]]` / `![[Note#Heading]]` chỉ đọc, sửa bản gốc → bản nhúng cập nhật; chống nhúng vòng.
- **Chấp nhận:** Note A chứa `[[Note B]]` → Note B thấy A ở "Được liên kết từ"; đổi tên B không làm gãy liên kết.

### F12 — Grid / List / Calendar
- Nút chuyển view trên toolbar compact; **nhớ view theo từng folder**; là *state* của cùng Collection Workspace.

| View | Mô tả |
|---|---|
| **Grid** | Masonry. Mobile 2 cột, tablet 3, web 3–5 cột. Card: màu, tiêu đề, 6 dòng đầu / thumbnail ảnh / tiến độ checklist, tag, icon nhắc nhở, icon ghim, icon đang chia sẻ |
| **List** | Mỗi dòng: tiêu đề, 1–2 dòng trích, ngày sửa, tag, icon (ảnh, checklist, nhắc nhở); chạm cả dòng để mở |
| **Calendar** | Tháng/Tuần; mỗi ngày có chấm/số lượng; chọn nguồn **Ngày tạo / Ngày sửa / Ngày nhắc**; chạm ngày → danh sách note; nút **Hôm nay**; dùng trục thời gian riêng (không áp Sort vô nghĩa) |

- Kanban/Graph mở từ menu view nâng cao (F33). Không overlay "Hiển thị ghi chú".
- **Chấp nhận:** Grid → List → Calendar → thoát/mở lại vẫn đúng view cuối; chạm note ở view nào cũng mở đúng note.

### F13 — Tìm kiếm
- Ô tìm luôn ở Home; Web **Ctrl+K / Cmd+K** (ngoài editor) mở palette tìm kiếm & lệnh nhanh.
- Tìm trong: **tiêu đề, nội dung, mục checklist, nội dung bảng, tag, tên folder, chữ trong ảnh/scan/PDF (OCR)**, văn bản tài liệu hỗ trợ.
- Kết quả **hiện ngay khi gõ**, **≤ 300 ms với 1.000 note**; **không phân biệt hoa/thường và dấu** ("ca phe" ↔ "cà phê"); **tô sáng** từ khoá + nêu nơi khớp ("Trong ảnh scan", "Trong checklist").
- **Lịch sử 5 lần gần nhất**, xoá được (không có trang lịch sử riêng). Kết hợp được Quick filter và Sort. **Tìm được khi offline** (nội dung đã lập chỉ mục).
- Cú pháp nâng cao: `tag:hci`, `has:image`, `has:math`, `task:todo`, `"cụm chính xác"`, `-loai-tru`.
- Không kết quả → "Không tìm thấy kết quả" + `Xoá bộ lọc` khi liên quan.
- **Chấp nhận:** gõ "hoa don" → ra note có scan chứa "Hoá đơn", từ khoá tô sáng; chọn kết quả mở note và neo vào chỗ khớp.

### F14 — Sắp xếp (Sort)
- Tiêu chí: **Ngày sửa (mặc định), Ngày tạo, Tên A–Z, Màu, Ngày nhắc nhở gần nhất**; chiều **Tăng/Giảm**; nhớ theo folder; áp dụng Grid/List; **note ghim luôn ở trên cùng**.
- **Mobile:** sheet ngắn (radio + chiều) hoặc segmented; chọn xong áp dụng và đóng. **Web:** popover trên toolbar. Không trang Sort riêng; không dòng dẫn tới sub-sheet rỗng.

### F15 — Quick filter
- Hàng **chip** nằm dưới ô tìm kiếm/header, bấm 1 lần để lọc; mở rộng bằng sheet lọc nâng cao gọn.

| Nhóm | Lựa chọn |
|---|---|
| Loại nội dung | Có ảnh · Có scan · Có bản vẽ · Có bảng · Có checklist · Checklist chưa xong · Có công thức · Có code |
| Nhắc nhở | Có nhắc nhở · Nhắc nhở hôm nay · Quá hạn · Có việc quá hạn |
| Thời gian | Hôm nay · 7 ngày qua · 30 ngày qua · Chọn khoảng ngày |
| Tổ chức | Folder · Tag (nhiều) · Màu (nhiều) · Đã ghim · Có liên kết |
| Trạng thái | Được chia sẻ · Đã lưu trữ |

- Chọn nhiều bộ lọc = **VÀ**; hiển thị **số kết quả** + nút **Xoá bộ lọc**; tuỳ chọn lưu "Bộ lọc của tôi". Không có "trang bộ lọc".
- **Chấp nhận:** "Có checklist" + màu Vàng → chỉ note vàng có checklist.

### F16 — Nhắc nhở & Thông báo theo lịch
- Đặt từ thuộc tính note hoặc icon đồng hồ trên task: **Ngày + Giờ**; chip nhanh *Chiều nay 17:00 · Tối nay 20:00 · Sáng mai 08:00 · Tuần sau*.
- **Lặp:** Không · Hằng ngày · Các ngày trong tuần (chọn thứ) · Hằng tuần · Hằng tháng · Hằng năm · Tuỳ chỉnh (mỗi N ngày/tuần) · Ngày kết thúc.
- **Tối đa 5 nhắc nhở/note** (mỗi task có thể có nhắc riêng).
- Thông báo hiện **tiêu đề + 2 dòng đầu** (hoặc 3 mục checklist chưa xong). Hành động: **Mở note · Đánh dấu xong · Báo lại (10 phút / 1 giờ / Ngày mai)**.
- Màn **Nhắc nhở:** nhóm *Quá hạn / Hôm nay / Ngày mai / Tuần này / Sau đó*; chọn → mở note/task.
- **Chạy offline** (lập lịch trên thiết bị khi nền tảng hỗ trợ). Cài đặt: bật/tắt, âm thanh, **giờ im lặng** (ví dụ 22:00–06:00). Web: thông báo trình duyệt (khi đã cho phép) + danh sách trong app; phụ thuộc OS/trình duyệt phải nêu rõ.
- **Lỗi:** chưa cho phép thông báo → banner + đường tới Cài đặt (không giả thông báo); giờ trong quá khứ → báo lỗi inline, không cho lưu.
- **Chấp nhận:** đặt nhắc 1 phút sau, tắt mạng, khoá máy → vẫn nhận đúng giờ; "Báo lại 10 phút" → nhắc lại sau 10 phút.

### F17 — Chia sẻ
- **Bảng chia sẻ của OS** (Zalo, Messenger, Email…) dưới dạng **Văn bản · Ảnh (chụp note) · PDF**.
- **Link chỉ xem** công khai: người nhận mở bằng trình duyệt, không cần tài khoản; hết hạn **1 / 7 / 30 ngày / Không hết hạn**; nút **Thu hồi**; icon "đang chia sẻ" trên card; danh sách **"Đang chia sẻ"** trong Cài đặt.
- Link nội bộ (wiki/deep link) **không** phải link công khai; không tự lộ nội dung riêng tư; **không lộ note đã khoá**.
- **Mã QR** sinh từ link công khai thật, đặt giữa với vùng im lặng đủ, quét được, không tràn khung điện thoại (P2.13).
- **Publish thành trang web** cần xác nhận công khai rõ + thu hồi/hết hạn (P2.14).
- **Nhận chia sẻ từ app khác** (văn bản, link, ảnh) → Quick Capture, chọn tạo note mới hoặc thêm vào note; không màn giới thiệu xen giữa.
- **Chấp nhận:** thu hồi/hết hạn → link vô hiệu; người xem công khai không sửa được.

### F18 — Xuất PDF, Markdown, Sao lưu
- **PDF:** xuất 1 note / nhiều note chọn (gộp một file) / cả folder. Tuỳ chọn: **A4 / A5 / Letter**, **Dọc/Ngang**, kèm/không kèm ảnh, kèm/không tiêu đề & ngày. Giữ định dạng: đậm/nghiêng, heading, checklist (ô tick), bảng (kèm dòng tổng), ảnh, bản vẽ, trang scan, công thức, callout, code tô màu, sơ đồ, chú thích cuối trang; **ẩn** bình luận `%% %%`; **nền luôn trắng**.
- Xem trước rồi **Lưu file** hoặc **Chia sẻ**. Tên mặc định `Tiêu-đề-note_dd-mm-yyyy.pdf` (chuẩn hoá an toàn cho OS).
- **Markdown/ZIP** riêng: note/folder/tất cả thành `.md` + `attachments/` + frontmatter; backup ZIP (md + tệp + metadata, tuỳ chọn PDF) có tiến trình; round-trip nhập lại giữ tham chiếu.
- Không trang "xuất thành công"; về lại note, hiển thị kết quả/lỗi nhẹ nhàng + thử lại.
- **Chấp nhận:** xuất hoá đơn có bảng → PDF đúng bảng và tổng tiền.

### F19 — Đồng bộ, Offline, Xung đột
- Dữ liệu gắn tài khoản; thiết bị mới đăng nhập → tải toàn bộ note. **Offline-first**: thao tác bình thường khi mất mạng, tự đồng bộ khi có mạng; thay đổi hiện trên thiết bị kia **≤ 5 giây** khi cả hai online.
- Đồng bộ: note, folder, tag, màu, ghim, nhắc nhở, mẫu riêng, cài đặt giao diện, đính kèm.
- **Trạng thái:** `Đã lưu trên máy` · `Đang đồng bộ` · `Đã đồng bộ` · `Offline — chờ đồng bộ` · `Lỗi — Thử lại`. Banner offline (chỉ khi offline): "Bạn đang offline – thay đổi sẽ đồng bộ khi có mạng".
- **Xung đột** (cùng note sửa trên 2 thiết bị khi offline): **giữ cả hai bản**, bản thứ hai tên "… (bản xung đột – tên thiết bị)"; banner không chặn → so sánh/gộp theo dòng/giữ cả hai; **không bao giờ im lặng ghi đè**.
- Tuỳ chọn mobile **chỉ đồng bộ ảnh qua Wi-Fi**; danh sách thiết bị đang đăng nhập + **đăng xuất từ xa**.
- **Chấp nhận:** tạo note khi tắt mạng → bật mạng → note xuất hiện trên web; xung đột giữ đủ cả hai bản đến khi người dùng quyết.

### F20 — Focus mode
- Bật từ editor (Web **Ctrl+Shift+F**). **Ẩn** nav/sidebar/toolbar (toolbar hiện lại khi chọn chữ hoặc rê chuột lên trên). Độ rộng dòng **~70 ký tự**, căn giữa. **Làm mờ đoạn khác**, chỉ sáng đoạn đang viết (bật/tắt). Đếm **từ/ký tự** ở góc.
- Mobile: tuỳ chọn tắt thông báo của app trong lúc tập trung (không giả định Do Not Disturb của OS). Thoát: nút **X** hoặc **Esc**, khôi phục đúng vùng chọn/vị trí cuộn. Hẹn giờ **15 / 25 / 45 phút** có đếm ngược.

### F21 — Cỡ chữ
- 4 mức: **Nhỏ 14px · Vừa 16px (mặc định) · Lớn 18px · Rất lớn 20px**; thanh trượt có **xem trước trực tiếp**; áp dụng **toàn app** (danh sách, card, editor, menu). Mobile có tuỳ chọn **"Theo cỡ chữ hệ thống"**.
- Bố cục không vỡ ở mức Rất lớn (chữ xuống dòng, không bị cắt; không dùng chiều cao cố định cho card/nút); chịu được zoom 200%; lưu qua các phiên.

### F22 — Giao diện Sáng / Tối
- **Sáng · Tối · Theo hệ thống** (mặc định); đổi ngay, không khởi động lại. Dùng Color System v3 (mục 11). Mọi màu (nền, chữ, 8 màu note, màu tag, bảng vẽ) có biến thể Dark đạt **WCAG AA ≥ 4.5:1**. Bản vẽ và ảnh **giữ màu gốc**. **PDF xuất luôn nền trắng.**
- Kiểu **giấy note** (Ô vuông/Kẻ dòng/Trơn) độc lập với Sáng/Tối, áp dụng cả hai app.

---

## 8. F23 — TRÌNH SOẠN THẢO MARKDOWN (23.1–23.10)

Hỗ trợ **CommonMark + GFM** và các mở rộng phổ biến (Obsidian/Bear/Typora) + **LaTeX**. Mọi cú pháp có **gợi ý khi gõ**, **nút toolbar** và **xem trước trực tiếp**. Điều khiển định dạng hiện ở **toolbar theo vùng chọn**, không phải màn hình riêng.

### 23.1 Định dạng chữ
| Kiểu | Cú pháp | Phím tắt (Web) |
|---|---|---|
| Heading 1–6 | `#` … `######` | Ctrl+1 … Ctrl+6 |
| Đậm · Nghiêng · Đậm nghiêng | `**chữ**` · `*chữ*` · `***chữ***` | Ctrl+B · Ctrl+I |
| Gạch ngang | `~~chữ~~` | Ctrl+Shift+X |
| Gạch chân | `<u>chữ</u>` | Ctrl+U |
| Tô sáng nhiều màu | `==chữ==` (vàng); màu khác ở toolbar: Vàng · Xanh lá · Hồng · Xanh dương · Tím | Ctrl+Shift+H |
| Chữ màu | Chọn chữ → bảng 8 màu (lưu `<span>` đã lọc an toàn) | |
| Chỉ số trên/dưới | `x^2^` · `H~2~O` | |
| Code trong dòng | `` `code` `` | Ctrl+` |
| Phím bấm | `<kbd>Ctrl</kbd>` | |
| Emoji | `:smile:` → 😄 (gõ `:` hiện gợi ý) | |
| Ký tự thoát | `\*` hiện dấu `*` thật | |

### 23.2 Cấu trúc & khối
| Khối | Cú pháp | Ghi chú |
|---|---|---|
| Danh sách chấm/số | `- ` · `1. ` | Enter tiếp tục, Enter ở dòng rỗng để thoát; Tab/Shift+Tab thụt lề nhiều cấp |
| Checklist | `- [ ] việc` · `- [x] xong` | Chạm ô để tick; lồng nhiều cấp (= F4) |
| Trích dẫn | `> chữ` | Lồng `>>` |
| **Callout** | `> [!note]` · `[!tip]` · `[!important]` · `[!warning]` · `[!danger]` · `[!question]` · `[!example]` | Tiêu đề tuỳ chỉnh `> [!tip] Mẹo nhỏ`; thu gọn được `[!tip]-` (đóng) / `[!tip]+` (mở) |
| Khối code | ` ```python ` | Tô màu **50+ ngôn ngữ**, nút Sao chép, số dòng |
| Đường kẻ | `---` | |
| **Thu gọn mục** | Mũi tên cạnh Heading để gập cả mục; hoặc `<details><summary>…</summary>…</details>` | |
| Chú thích cuối trang | `chữ[^1]` … `[^1]: Nội dung` | Rê chuột/chạm xem nhanh |
| Danh sách định nghĩa | `Thuật ngữ` + dòng `: định nghĩa` | |
| **Mục lục tự động** | `[TOC]` | Sinh từ Heading, chạm để nhảy; có Outline cạnh editor |
| Bình luận ẩn | `%% ghi chú riêng %%` | Chỉ thấy khi sửa; **không hiện** ở Reading/PDF/link chia sẻ |
| HTML cơ bản | `<br>`, `<sub>`, `<sup>`, `<mark>` | Lọc an toàn (sanitize) |

### 23.3 Bảng
- Bảng GFM `| Cột 1 | Cột 2 |` + `|---|---|`; căn lề `:---` · `:---:` · `---:`.
- Gõ `| A | B |` + Enter → tự sinh dòng phân cách + hàng mới; Source mode **tự căn thẳng mã bảng**; **Tab** nhảy ô; Enter ở ô cuối thêm hàng.
- Lệnh `/bang 3x4` chèn bảng 3 cột × 4 hàng.
- **Dán từ Excel / Google Sheets / HTML / LaTeX `tabular`** → bảng Markdown.
- Live Preview có **trình sửa bảng trực quan**: thêm/xoá/kéo hàng–cột, sắp xếp theo cột.
- **Ô tính toán:** `=SUM(C2:C5)`, `=B2*C2`; hỗ trợ `SUM, AVG, COUNT, MIN, MAX` (dùng cho Hoá đơn, Thu–chi); tính trên số, không trên chuỗi tiền đã định dạng.
- "Sao chép bảng dạng Markdown / LaTeX / CSV".
- **Quy tắc thị giác bắt buộc:** nền bảng **đục** (opaque), header nâng nền, hàng xen kẽ, đường chia hàng/cột đủ tương phản, dòng tổng phân biệt nhưng không chói; **chữ căn trái, số/tiền căn phải**; lưới của giấy note **không xuyên qua** bảng. Mobile: cuộn ngang, cột đầu cố định; không nén 6 cột vào 400px.

### 23.4 Công thức LaTeX
- Trong dòng `$E = mc^2$` · khối `$$ ... $$` · đánh số `\tag{1}`.
- **Gõ `\`** → gợi ý lệnh kèm hình xem trước (`\frac`, `\sqrt`, `\sum`, `\int`, `\lim`, `\alpha`…`\omega`, `\le`, `\ne`, `\infty`, `\rightarrow`, `\mathbb{R}`…); chọn → chèn khung `\frac{□}{□}`, **Tab** nhảy ô.
- **Bảng ký hiệu** trên toolbar: Phép toán · Hy Lạp · So sánh · Mũi tên · Tập hợp · Logic — chạm để chèn.
- **Mẫu công thức:** Phân số, Căn, Luỹ thừa, Σ, Tích phân, Giới hạn, Ma trận (`pmatrix`, `bmatrix`), Hệ phương trình (`cases`), Đạo hàm, nhiều dòng (`align`). `\boxed{...}` đóng khung kết quả; `\color{red}{...}` tô màu một phần.
- Xem trước nổi ngay dưới con trỏ; **lỗi cú pháp** gạch đỏ + gợi ý ("Thiếu dấu }"), **giữ nguyên nội dung gõ**.
- Mobile: hàng phím phụ `$ \ { } ^ _ ( ) [ ] | #`. Render **KaTeX/MathJax trên thiết bị, chạy offline** (nếu chưa làm được phải ghi là phụ thuộc).
- **Không** có các trang riêng "Math Tool / Insert Formula / LaTeX Playground / Graph Launcher" cho việc chèn thông thường; workspace riêng chỉ cho vẽ đồ thị tương tác phức tạp.

### 23.5 Liên kết, nhúng, tham chiếu
| Loại | Cú pháp | Kết quả |
|---|---|---|
| Link web | `[chữ](https://…)`; dán URL trần tự thành link | **Chọn chữ rồi dán URL → tự tạo link** |
| Liên kết note | `[[Tên note]]` | F11 |
| Tên hiển thị khác | `[[Tên note\|chữ hiển thị]]` | |
| Tới mục | `[[Tên note#Heading]]` | Mở và cuộn tới mục |
| Tới đoạn | `[[Tên note#^abc123]]` | Đánh dấu đoạn bằng `^abc123` |
| Nhúng note | `![[Tên note]]` · `![[Tên note#Heading]]` | Hiện ngay trong note, tự cập nhật |
| Ảnh | `![mô tả](anh.jpg)` · `![[anh.jpg\|300]]` | Dán/kéo thả → tự lưu file + chèn cú pháp |
| Nhúng file | `![[tai-lieu.pdf]]` · `![[ghi-am.m4a]]` · `![[ban-ve.draw]]` · `![[scan-01.pdf]]` | Xem PDF, nghe audio, mở bản vẽ, xem scan trong note |
| Video | `![](https://youtube.com/…)` | Trình phát nhúng (sau khi xử lý URL an toàn) |
| Email/điện thoại | `<mailto:…>` · `[Gọi](tel:0905…)` | Chạm để gọi/gửi mail |

Chặn scheme nguy hiểm (`javascript:` …). Link picker nằm trong toolbar vùng chọn; không có màn "Tạo liên kết" riêng.

### 23.6 Điều khiển app bằng cú pháp (metadata)
| Muốn | Cú pháp | Tương đương giao diện |
|---|---|---|
| Gắn tag | `#hoc-tap` · lồng `#hoc-tap/hci` | Nút Tag; cây tag ở sidebar |
| Thuộc tính | Frontmatter: `tags`, `color`, `pinned`, `folder`, `aliases`, `remind` | Nút Màu, Ghim, Folder, Nhắc nhở |
| Tên gọi khác | `aliases: [Persona, Chân dung người dùng]` | `[[Chân dung…` vẫn gợi ý đúng note |
| Nhắc nhở trong dòng | `- [ ] Nộp bài ⏰ 2026-01-12 20:00` (hoặc gõ `@` chọn ngày giờ) | Nhắc riêng mục đó, hiện ở Calendar |
| Hạn chót / lặp | `📅 2026-01-13` · `🔁 hằng tuần` | Calendar view, lặp như F16 |
| Ưu tiên (legacy) | `!!` (cao) · `!` (vừa) | Chỉ giữ/nhập khi có sẵn, không tạo màn/chỉ mục mới |
| Biến mẫu | `{{date}}` · `{{time}}` · `{{title}}` | Tự điền khi dùng Template |

### 23.7 Khối truy vấn (Query block, không AI)
````markdown
```query
tag: #hoc-tap
task: chua-xong
sort: remind
```
````
Hiện mọi việc chưa xong có tag `#hoc-tap`, xếp theo hạn — "bảng điều khiển" cá nhân, chạy cục bộ trên chỉ mục. Kết quả là dòng chỉ đọc liên kết về note nguồn. Có **trình tạo truy vấn bằng giao diện** (popover/sheet *trong editor*: chọn tag/folder/màu/trạng thái). Truy vấn lỗi/không hỗ trợ → báo lỗi inline.

### 23.8 Sơ đồ Mermaid
- Khối `mermaid`: flowchart, sequence, gantt, mindmap, pie (khi thư viện hỗ trợ). Render inline, nút chuyển xem mã, mẫu trong menu `/`, báo lỗi cú pháp rõ; không viewer riêng mặc định.

### 23.9 Hỗ trợ gõ & gợi ý
- **Menu `/`** liệt kê mọi khối (Heading, Checklist, Bảng, Callout, Code, Công thức, Ảnh, Scan, Bản vẽ, Ghi âm, Liên kết note, Mục lục, Sơ đồ, Query…) kèm **cú pháp tắt** bên cạnh để học dần.
- **Tự chuyển khi gõ:** `# ` → Heading, `- ` → danh sách, `[] ` → checklist, `> ` → trích dẫn, `---` → đường kẻ, ` ``` ` → khối code.
- **Tự đóng cặp:** `**`, `==`, `` ` ``, `$`, `(`, `[`, `[[`, `{`. Chọn chữ rồi gõ `*`, `=`, `~`, `` ` ``, `$` → **bọc chữ**.
- **Gợi ý theo ngữ cảnh:** `[[` note · `#` tag · `:` emoji · `\` LaTeX · `@` ngày giờ · ` ``` ` ngôn ngữ code · `> [!` loại callout.
- **Dán thông minh:** HTML/trang web → Markdown sạch; dán ảnh → lưu file + chèn; dán URL lên chữ đã chọn → link; giữ **Shift** (Ctrl+Shift+V) → chữ thuần.
- **Format document:** chuẩn hoá khoảng trắng, căn bảng, đánh lại số danh sách (không phá hoại).
- **Thanh trạng thái:** số từ, ký tự, thời gian đọc ước tính.
- **Cheat sheet:** toolbar hoặc **Ctrl+/**, có ô "thử ngay"; mobile là help sheet. Note hướng dẫn Markdown có sẵn khi cài mới.
- **Cài đặt:** bật/tắt tự chuyển Markdown · bật/tắt gợi ý · chế độ mặc định (Live/Source) · font code riêng.

### 23.10 Nhập / xuất Markdown
- **Xuất** 1 note / folder / toàn bộ thành `.md` + `attachments/`, giữ frontmatter → mở được bằng Obsidian, Typora, VS Code, GitHub.
- **Nhập** `.md` / cả thư mục (vault Obsidian), Bear, Notion (bản xuất Markdown), Google Keep, Evernote (khi định dạng hỗ trợ); có xem trước ánh xạ + tiến trình; không ghi đè khi chưa có lựa chọn gộp rõ ràng; giữ aliases, tag, heading, tham chiếu tệp tương đối.
- **Sao chép** note dạng Markdown hoặc dạng văn bản đã định dạng (dán Word/Gmail vẫn đẹp).
- PDF render đầy đủ công thức, callout, bảng tính, code tô màu, sơ đồ, chú thích cuối trang; ẩn `%% %%`.
- Nhập/xuất dùng overlay/dialog + progress, **không có trang "thành công"**.

### Ảnh hưởng của Markdown-first lên các chức năng khác
| Chức năng | Thay đổi |
|---|---|
| Checklist | `- [ ]` thuần; nhắc, hạn, lặp theo từng mục |
| Template | File `.md` có biến `{{date}}`; bảng tính dùng ô tính toán |
| Tag | `#tag` trong nội dung + tag lồng dạng cây |
| Backlink | Thêm liên kết mục/đoạn, nhúng note, aliases |
| Search | Thêm cú pháp `tag:`, `has:`, `task:`, `"…"`, `-…` |
| Quick filter | Thêm: Có công thức · Có code · Có bảng · Có việc quá hạn |
| PDF | Render đầy đủ Markdown mở rộng |
| Đồng bộ | File văn bản → nhanh; xung đột có thể **gộp theo dòng** |
| Dark mode | Tô sáng, callout, theme code có bản Dark |

### Tiêu chí chấp nhận F23
- Người dùng thường: tạo note, gõ "Đi chợ", Enter, gõ nội dung → card hiện tiêu đề "Đi chợ"; **không thấy ký hiệu Markdown nào**.
- `- [ ] Nộp bài ⏰ 2026-01-12 20:00` → mục có icon chuông, xuất hiện ở Nhắc nhở và Calendar.
- `\fr` → gợi ý `\frac`; chọn → `\frac{□}{□}`, Tab nhảy ô, preview đúng.
- `| A | B |` + Enter → bảng 2 cột; ô `=SUM()` cộng đúng.
- `> [!warning]- Hạn nộp` → callout màu cam thu gọn, bấm để mở.
- `![[Note B#Kết luận]]` → đúng mục "Kết luận"; sửa Note B → bản nhúng cập nhật.
- Xuất `.md` rồi mở bằng Obsidian → nội dung, tag, ảnh hiển thị đúng.

---

## 9. CHỨC NĂNG MỞ RỘNG F24–F33

> Phải có **coverage UI/tương tác** đầy đủ. Điều này **không** đồng nghĩa engine OCR/Office/Clipper/Graph đã chạy thật; phụ thuộc kỹ thuật phải công bố trung thực (mục 18).

### F24 — Kéo thả, Dán, Nhập nhiều tệp
- **Web:** kéo từ File Explorer/Finder vào editor, thả tại caret (có drop indicator, vùng đích rõ). Kéo ảnh, PDF, DOCX, TXT, MD, CSV, XLSX, PPTX, audio, video, nhiều file cùng lúc. **Ctrl/Cmd+V** nhận ảnh clipboard (screenshot), văn bản, HTML, URL, file; ảnh tạo **attachment thật**; HTML → Markdown sạch; **Ctrl+Shift+V** dán chữ thuần.
- Kéo từ web: URL → hyperlink; ảnh lưu cục bộ nếu được phép (nếu không giữ link + cảnh báo); vùng chọn → Markdown đã làm sạch.
- Kéo note trong sidebar/danh sách vào editor → `[[Tên note]]`; giữ modifier hoặc chọn menu → `![[Tên note]]`. Kéo attachment → link hoặc embed. Kéo khối bằng drag handle (giữ checklist state, tham chiếu, undo/redo). Kéo nhiều note sang folder.
- **Mobile:** dán clipboard, chia sẻ từ app khác (Share To), trình chọn tệp/ảnh, kéo thả giữa app khi OS hỗ trợ, nhấn giữ di chuyển khối.
- **Hợp đồng UX:** dán screenshot → **nhúng ngay tại caret**, không màn "chọn kiểu nhập". PDF/DOCX → **action sheet/popover ngắn** `Đính kèm · Xem trước · Trích chữ · Chuyển thành ghi chú`, chọn một lần rồi làm ngay, không wizard; mặc định theo hành động thả, đổi được sau. Nhiều file → tiến độ/thành công/thất bại/thử lại/huỷ **từng file**; một file hỏng không làm mất file khác; autosave + hàng đợi offline.
- **An toàn:** kiểm tra MIME thật, chặn tệp thực thi/nguy hiểm, không chạy macro/script, **không ghi đè tên trùng** (ID an toàn); giới hạn dung lượng theo MIME do backend cấu hình (chỉ ảnh có mốc 10 MB); media từ xa không cho phép → fallback URL an toàn. Vùng thả luôn có nút "Chọn tệp" tương đương.
- **Chấp nhận:** Ctrl+V screenshot → ảnh hiển thị và còn sau reload; kéo 3 file → 3 attachment đúng thứ tự, không mất khi offline.

### F25 — Đính kèm tệp & xem trước inline
- Hai cách chèn: **Card đính kèm** (icon, tên, loại, dung lượng, tải xuống, đổi tên, xoá, trạng thái sync) và **Nhúng xem trước** trong note.
- **Ảnh:** đổi cỡ, căn lề, caption, alt, crop/rotate, lightbox, gắn link.
- **PDF:** nhiều trang, chuyển trang, zoom, **tìm trong tài liệu**, tải xuống, toàn màn hình, thumbnail trang, link tới trang cụ thể.
- **DOCX:** xem trước khi renderer hỗ trợ; nếu không → tải xuống hoặc chuyển thành note. **XLSX/CSV:** preview bảng chỉ đọc trước khi nhập. **PPTX:** preview nếu bộ chuyển đổi hỗ trợ, nếu không báo rõ. **Audio/video:** player + tải về. File khác: card.
- Xem trước **không sửa file gốc**; nhị phân tách khỏi `.md`; ID ổn định → đổi tên note/folder không gãy tham chiếu; đồng bộ cùng note; truy cập offline sau khi đã tải.
- Cú pháp: `![[attachments/uuid-file.pdf]]`, `![[attachments/image.png|400]]`, link `[[attachments/report.pdf]]`; metadata caption/alt/link phải xuất Markdown tương thích.
- Xoá đính kèm đang được note khác tham chiếu → cảnh báo.
- **Chấp nhận:** thả PDF 12 trang → đọc từng trang trong note, zoom, tải lại vẫn mở; ảnh gắn được link và đổi cỡ.

### F26 — Nhập tài liệu, OCR, Chuyển đổi, Pipeline scan
**Bốn nghĩa KHÔNG được gộp:**
1. **Đính kèm** — giữ nguyên file, tạo card.
2. **Xem trước / Xem như tài liệu** — render chỉ đọc, có thể nhúng inline.
3. **Trích xuất chữ** — PDF có text layer / DOCX: đọc trực tiếp; PDF scan/ảnh: OCR; hiện bản xem trước văn bản, sửa được, chèn tại con trỏ hoặc tạo note mới.
4. **Chuyển thành Markdown** — ánh xạ heading, đoạn, danh sách, bảng, ảnh trong giới hạn parser; **báo định dạng không giữ được**; giữ bản gốc làm attachment để đối chiếu.
- **Scan/chỉnh trang** áp dụng cho ảnh/PDF scan (xoay, crop, tăng tương phản, sắp xếp trang, OCR lại); **không** áp bộ lọc ảnh lên DOCX trừ khi đã render thành trang.
- **Luồng:** Nhập/Thả → lựa chọn ngắn → tiến trình/preview ngay trong note nếu được; nếu cần nhiều trang/công cụ → mở **Document Workspace** → Chèn/Thay thế/Lưu bản sao → quay đúng note + caret.
- **OCR:** Tiếng Việt có dấu + Tiếng Anh; nhiều trang; trạng thái/lỗi theo trang; huỷ/thử lại; lập chỉ mục tìm kiếm. **Chỉ xử lý cục bộ khi engine có thật**; nếu phụ thuộc server → hiển thị rõ không khả dụng offline và **không gửi file lên server khi chưa được người dùng cho phép**. Không cam kết OCR chữ viết tay/tài liệu phức tạp chính xác tuyệt đối.
- **Trạng thái cần thiết kế:** PDF mật khẩu, tệp không hỗ trợ, OCR không khả dụng ("Không thể trích xuất chữ trên thiết bị này. Tệp gốc vẫn được giữ.").
- **Chấp nhận:** PDF 2 trang chỉ có ảnh → OCR → tìm được một từ; DOCX có heading + bảng → note Markdown có cấu trúc cơ bản + file gốc đính kèm.

### F27 — Hyperlink, Wiki-link, Backlink, Deep link
- Chọn văn bản → liên kết (Web: **Ctrl/Cmd+K trong editor khi có vùng chọn**; ngoài editor là tìm kiếm); dán URL lên chữ đang chọn → `[chữ](URL)`.
- Chọn ảnh/tệp/ô bảng/khối → gắn URL, note, heading, block, PDF/trang PDF; click không kích hoạt nhầm khi đang đổi cỡ ảnh.
- Wiki-link `[[Note]]`, `[[Note|Tên]]`, `[[Note#Heading]]`, `[[Note#^id]]` + gợi ý + backlinks (F11).
- **Copy link** tới note/heading/block; **deep link** mở đúng vị trí trên web/mobile sau khi kiểm tra quyền; **link nội bộ không tự thành link công khai**.
- Đổi tên note giữ liên kết bằng ID ổn định; xuất Markdown có đường dẫn/alias hợp lý. **Link hỏng** có trạng thái rõ + cho sửa đích; đích không có quyền có fallback an toàn.
- Link ngoài: kiểm tra scheme, chặn `javascript:`; sửa link không tự mở đích.
- **Chấp nhận:** ảnh liên kết tới Note B → click mở B; đổi tên B → link vẫn chạy; copy deep link tới heading → mở đúng vị trí.

### F28 — Chỉnh sửa theo khối & nội dung tái sử dụng
- Loại khối: heading, paragraph, quote, checklist, bảng, ảnh, code, callout, tệp, query, sơ đồ.
- **Drag handle** (Web) / nhấn giữ + sắp xếp (Mobile, có phương án bàn phím): Di chuyển, Nhân bản, Đổi loại khối (chỉ phép chuyển an toàn), Xoá, Copy/Move sang note khác.
- Chọn nhiều khối; cut/copy/paste giữ Markdown + tham chiếu tệp; undo/redo.
- **Transclusion** chỉ đọc mặc định, có lệnh mở bản gốc; **chống vòng** và nhúng note đã xoá. Collapse heading/callout; Outline bên cạnh (click nhảy; kéo heading để chuyển cả section). **Block ID ổn định khi đổi vị trí.**
- **Chấp nhận:** kéo khối ảnh qua checklist → đổi thứ tự không mất ảnh/trạng thái tick; undo khôi phục.

### F29 — Web Clipper & Quick Capture
- Nhận link/chữ/ảnh từ trình duyệt hoặc menu Share OS (Web: extension/share target nếu có). Tuỳ chọn **Lưu liên kết · Lưu trích đoạn · Lưu trang dạng Markdown** (khi được phép).
- Lưu URL nguồn, tiêu đề, ngày capture; preview trước khi tạo; **chống trùng theo URL**; không vượt nội dung bị giới hạn truy cập.
- Mặc định vào **Inbox / Chưa phân loại**, chọn folder/tag sau (không bắt chọn folder trước). Offline: lưu nháp, xử lý sau.
- **Chấp nhận:** share URL từ browser → note có tiêu đề, URL nguồn, nội dung đã chọn.

### F30 — Thư viện tệp
- Mục **Tệp** trong Library/Sidebar (không phải tab điều hướng chính): lọc Ảnh/PDF/Tài liệu/Audio; tìm theo tên hoặc chữ OCR; xem tệp được những note nào dùng.
- Đổi tên/tải/xoá; xoá tệp còn được tham chiếu → cảnh báo; dọn tệp mồ côi có xác nhận; phát hiện trùng bằng hash — **không tự hợp nhất/xoá**; tiến trình tải/lỗi/thử lại/tạm dừng/offline.

### F31 — Lịch sử phiên bản
- Menu note → **Lịch sử phiên bản**: dòng thời gian, diff cạnh nhau, xem Markdown/frontmatter/tham chiếu tệp cũ; **Khôi phục** hoặc **Lưu bản sao**.
- Snapshot trước nhập/chuyển đổi hàng loạt; khôi phục tạo phiên bản mới có xác nhận, **không tự ghi đè bản mới hơn**, không làm gãy link/đính kèm hiện tại; xung đột giữ cả hai.
- Mobile: sheet (so sánh toàn chiều rộng chỉ khi thật cần). Web: panel/dialog. Lỗi: thiếu đính kèm, phiên bản hết hạn, chưa cache offline.

### F32 — Chú thích ảnh/PDF & Chữ ký
- Bút, highlighter, khung chữ, mũi tên, hình, **chữ ký**; undo/redo, zoom, bảng màu an toàn. Lớp chú thích lưu riêng, **file gốc không đổi**, đồng bộ; xuất PDF có tuỳ chọn gộp (flatten).
- Mobile: cảm ứng/stylus; Web: chuột/bút. Từ viewer → **Chú thích** mở workspace vẽ phù hợp, quay về cùng trang. Chữ ký chèn vào hoá đơn/biểu mẫu và xuất PDF; dữ liệu chữ ký **không công khai** nếu chưa có hành động chia sẻ.

### F33 — Tiện ích hằng ngày
| Tính năng | Điểm vào | Hành vi tại chỗ |
|---|---|---|
| **Daily note** | Menu Home hoặc Calendar "Hôm nay" | Mở/tạo **một** note theo ngày theo mẫu đã chọn |
| **Kanban** | Menu view nâng cao / `⋯` | Thay view collection trong cùng workspace; kéo thẻ theo trạng thái/tag |
| **Graph view** | View nâng cao hoặc ngữ cảnh Backlinks | Mạng liên kết tương tác; chọn nút mở note; **có bản danh sách tương đương cho tiếp cận/bàn phím** |
| **Split view** | Toolbar editor (Desktop/Tablet) | Hai note cạnh nhau, chuyển đoạn/khối; **không bắt buộc trên điện thoại hẹp** (nêu phương án thay thế) |
| **Flashcards** | Menu note hoặc cú pháp `Câu hỏi :: Đáp án` | Overlay ôn tập lật thẻ, về lại note; không điểm đến mới |
| **QR Share** | Tuỳ chọn Chia sẻ sau khi có link công khai | QR quét được, có vùng im lặng |
| **Publish** | Tuỳ chọn Chia sẻ | Công khai rõ ràng + thu hồi/hết hạn |
| **Quick access / Favorites** | Home/Library | Ghim/gần đây/lối tắt; không thêm cây điều hướng thừa |
| **Template mở rộng** | F6 | Meeting minutes, lecture, journal, reading notes, project log (xem trước đầy đủ cả hai nền tảng) |

**Quy tắc F33:** bề mặt khám phá tuỳ chọn **không** làm luồng CRUD cốt lõi dài thêm; không trang welcome/feature riêng.

---

## 10. TRUY VẾT P1/P2 (ĐỀ XUẤT BỔ SUNG ĐÃ HỢP NHẤT — KHÔNG NHÂN BẢN)

### 10.1 P1 — Chức năng thông dụng
| ID | Chức năng | Hợp nhất vào | Trạng thái UI cần có |
|---|---|---|---|
| P1.1 | Đính kèm file (PDF, Word, Excel, audio) | F24–F26 | Card, preview, lỗi/thử lại |
| P1.2 | Ghi âm đính kèm | F1/F25 | Ghi/phát/chuyển bản chép chữ |
| P1.3 | **Khoá note** (PIN / vân tay / Face ID) | C1/C10 | Khoá/mở khoá, dự phòng, **preview bị che**, không lộ qua search/thông báo/chia sẻ theo chính sách |
| P1.4 | Lịch sử phiên bản | F31 | Timeline/preview/khôi phục |
| P1.5 | Nhân bản note / Sao chép sang folder | C3/F7 | Context menu + chọn đích |
| P1.6 | Mục lục (Outline) | F23.2/F28 | TOC, nhảy tới heading |
| P1.7 | Nhập từ Keep/Evernote/MD/TXT | F23.10/F26 | Chọn nguồn/tiến trình/lỗi |
| P1.8 | Nhận chia sẻ từ app khác | F17/F29 | Share target → Quick Capture |
| P1.9 | **Widget màn hình chính** | C3/F33 | Ghi chú/checklist/giọng nói/daily note; chạm đi thẳng đích (tuỳ nền tảng) |
| P1.10 | Sao lưu/xuất toàn bộ (ZIP) | F18/F23.10 | Tiến trình, lưu/chia sẻ |

### 10.2 P2 — Điểm khác biệt
| ID | Chức năng | Hợp nhất vào | Tương tác |
|---|---|---|---|
| P2.1 | Vẽ lên ảnh/scan | F32 | Viewer → Chú thích → lớp → lưu |
| P2.2 | Ký tên trên hoá đơn/biểu mẫu | F6/F32 | Chèn chữ ký → xuất PDF |
| P2.3 | Bảng → Biểu đồ | F6/F23.3 | Menu bảng → biểu đồ inline, liên kết nguồn |
| P2.4 | Ghi âm gắn chữ (timestamp) | F1/F25 | Chạm đoạn → phát từ mốc |
| P2.5 | Khối thu gọn (Toggle) | F23.2/F28 | Mở/gập inline |
| P2.6 | Mermaid | F23.8 | Sơ đồ inline |
| P2.7 | Daily note | F33 | Calendar → note ngày |
| P2.8 | Biến trong template | F6/F23.6 | Xem trước biến đã giải |
| P2.9 | Nhắc nhở từng mục checklist | F4/F16 | Chuông/ngày giờ inline |
| P2.10 | Split view | F33 | Desktop/Tablet |
| P2.11 | Graph view | F33 | View state + bản danh sách |
| P2.12 | Flashcard `Q :: A` | F33 | Overlay ôn tập |
| P2.13 | QR chia sẻ | F17/F33 | QR từ link công khai |
| P2.14 | Publish thành web | F17/F33 | Xác nhận công khai + thu hồi |

**Không nhân bản:** không xây hai hệ "đính kèm PDF"/"scan PDF" với mô hình dữ liệu khác nhau; không hai hệ "note thường"/"note Markdown"; không thêm tab điều hướng chính cho mỗi mục P1/P2.

---

## 11. HỆ MÀU v3 & DESIGN TOKENS (ĐÃ CHỐT — LẤY CẢM HỨNG OBSIDIAN)

> Chỉ mượn **ngôn ngữ màu** (neutral charcoal + violet), không sao chép bố cục Obsidian. Token dưới đây là quy định riêng của Opportunities.

### 11.1 Ba vai trò màu trong MỖI theme
| Vai trò | Dark | Light | Ứng dụng |
|---|---|---|---|
| 60% Dominant | Charcoal `#1C1C1C` | Soft White `#FAFAFA` | Nền app/workspace |
| 30% Secondary | Graphite `#282828` | Silver Gray `#E4E4E4` | Sidebar, card/panel/toolbar phân lớp |
| ~10% Accent | Violet `#A882FF` | Violet `#7852EE` | Primary action, active/focus/link/selection |

60–30–10 là **cân bằng cảm nhận**, không chia đúng % pixel. Violet có thể nhỏ hơn 10% để note là trọng tâm; không biến card/toolbar/sync thành toàn tím.

### 11.2 Semantic tokens (nguồn sự thật duy nhất)
| Token | Dark | Light |
|---|---|---|
| `bg.app` | `#1C1C1C` | `#FAFAFA` |
| `bg.sidebar` | `#282828` | `#E4E4E4` |
| `bg.workspace` | `#1C1C1C` | `#FAFAFA` |
| `bg.surface` | `#212121` | `#FFFFFF` |
| `bg.surface.secondary` | `#282828` | `#E4E4E4` |
| `bg.surface.elevated` | `#2E2E2E` | `#EFEFEF` |
| `bg.hover` | `#363636` | `#E8E4F0` |
| `border.default` | `#3F3F3F` | `#DADADA` |
| `border.strong` | `#606060` | `#A9A9A9` |
| `text.primary` | `#DADADA` | `#222222` |
| `text.secondary` | `#B3B3B3` | `#5C5C5C` |
| `text.muted` | `#9B9B9B` | `#646464` |
| `accent.base` | `#A882FF` | `#7852EE` |
| `accent.hover` | `#BEA3FF` | `#6840DD` |
| `accent.onFilled` | `#1C1C1C` | `#FFFFFF` |
| `selection.bg` | `#382C50` | `#EDE5FF` |
| `selection.text` | `#EADFFF` | `#43277B` |
| `button.primary.bg` / `.fg` | `#A882FF` / `#1C1C1C` | `#7852EE` / `#FFFFFF` |
| `button.secondary.bg` / `.fg` | `#363636` / `#DADADA` | `#E4E4E4` / `#222222` |
| `focus.ring` | `#A882FF` | `#7852EE` |
| `input.bg` | `#282828` | `#FFFFFF` |
| `shadow.overlay` | đen độ mờ thấp | mực độ mờ thấp |

Mọi thành phần dùng **semantic tokens toàn cục** (Figma variables với mode Light/Dark), không hardcode màu tuỳ ý.

### 11.3 Giấy note và bảng dữ liệu
| Thành phần | Dark | Light |
|---|---|---|
| Note canvas | `#232323` | `#FFFDFC` |
| Note grid | `rgba(218,218,218,0.045)` | `rgba(34,34,34,0.045)` |
| Note border | `#444444` | `#D9D9D9` |
| Editor text | `#DADADA` | `#222222` |
| Table surface | `#292929` | `#FFFFFF` |
| Table header | `#383838` | `#EEEAF6` |
| Table hàng lẻ / chẵn | `#292929` / `#303030` | `#FFFFFF` / `#F6F4F9` |
| Table divider | `#525252` | `#C8C3D0` |
| Table total row | `#343434` | `#EEEAF6` |

**Quy tắc:** lưới chỉ xuất hiện **trong vùng viết**, không xuyên qua bảng hoặc khối nội dung đục; mở note là nhận ra ngay "giấy" nhưng lưới không cạnh tranh với heading/công thức/nét bút. Header/hàng/cột/tổng phân biệt bằng nền/padding/divider có tương phản, không chỉ một viền mỏng. Kiểu giấy: **Ô vuông (mặc định) / Kẻ dòng / Trơn**, đổi tức thì, dùng cùng mức opacity; không phủ lưới lên preview ảnh/PDF/bảng.

### 11.4 Tương phản (WCAG, tính từ hex)
- `#DADADA` / `#1C1C1C` ≈ **12.19:1**
- `#B3B3B3` / `#282828` ≈ **7.03:1**
- `#A882FF` / `#1C1C1C` ≈ **5.93:1**
- `#FFFFFF` / `#7852EE` ≈ **4.95:1**
- `#7852EE` / `#FAFAFA` ≈ **4.75:1**

Mục tiêu: chữ thường ≥ 4.5:1, chữ lớn ≥ 3:1, viền control/focus quan trọng ≥ 3:1. Kiểm tra trên **nền thực tế sau khi chồng lớp/opacity**. Trạng thái disabled không là cách duy nhất truyền thông tin.

### 11.5 Màu ngữ nghĩa và màu người dùng
- Success/warning/error/destructive/info có màu riêng + icon/chữ; không dùng violet cho cả thành công lẫn xoá.
- Màu note (8 + không), màu folder/icon, màu tag, highlight (vàng/xanh lá/hồng/xanh dương/tím), bảng màu vẽ: độc lập với brand, mỗi theme có biến thể đủ tương phản.
- Ảnh và nét vẽ của người dùng **không đổi màu** khi chuyển theme; PDF nền trắng.
- Một màn hình không có nhiều CTA tím sáng cạnh tranh; folder đang chọn dùng tím **trầm**, CTA chính dùng tím sáng.

---

## 12. THÀNH PHẦN UI, TYPOGRAPHY, THUẬT NGỮ

### 12.1 Thành phần toàn cục (không copy-paste biến thể rời)
`AppHeader`, `BottomNavigation`, `Sidebar`, `CollectionToolbar`, `ViewSwitcher`, `SortControl`, `FilterChip`, `NoteCard`, `NoteRow`, `ContextMenu`, `OverflowMenu`, `Sheet`, `Dialog`, `PrimaryButton`, `SecondaryButton`, `GhostButton`, `IconButton`, `InlineTag`, `ColorPicker`, `FolderPicker`, `EditorToolbar`, `NoteCanvas`, `AttachmentCard`, `PDFViewer`, `Table`, `ChecklistItem`, `ReminderPicker`, `SyncIndicator`, `Toast/Snackbar`, `EmptyState`, `LoadingState`, `ErrorState`, `FocusRing`.

Mỗi thành phần có biến thể tương tác (`default`, `hover` (web), `focus-visible`, `pressed`, `selected`, `disabled`, `loading`, `error`), biến thể theme và nhãn tiếp cận. Tái dùng component Figma hiện có làm nền; chỉ refactor khi cần.

### 12.2 Typography & bố cục
- Phân cấp: Tiêu đề màn hình → Tiêu đề section → Tiêu đề note/dòng → Nội dung → Metadata. Không hai tiêu đề lớn ngang nhau tranh tiêu điểm.
- Thân chữ gốc 16px (mobile); người dùng chọn 14/16/18/20; chữ xuống dòng, hỗ trợ Dynamic Type/zoom 200%.
- Input có nhãn bền vững khi đã nhập (không "nhãn vô nghĩa" dưới control).
- Vùng chạm mobile **≥ 48×48 px**; web có focus rõ. Thang khoảng cách 4/8/12/16/24/32.
- Chip/meta vừa nội dung; hàng danh sách đủ padding; không kéo hai chip thành 600px.
- Icon có ngữ nghĩa đúng; icon không nhãn cần accessible name/tooltip; không chỉ dựa vào màu.
- Bottom sheet co theo nội dung và safe area; cuộn bên trong khi dài; **không** tạo khoảng đen trống lớn.

### 12.3 Thuật ngữ tiếng Việt chuẩn
| Ý nghĩa | **Dùng** | Tránh |
|---|---|---|
| Tạo note | `Ghi chú mới` / `Tạo ghi chú` | "Bắt đầu hành trình ghi chú" |
| Chuyển view | `Lưới` / `Danh sách` / `Lịch` | Overlay "Hiển thị ghi chú" khổng lồ |
| Sắp xếp | `Sắp xếp` / `Ngày sửa` | Hai nút full-width không liên quan |
| Hoàn tất checklist | `Hoàn thành` / `Đã hoàn thành` | `Đã thu` |
| Thu tiền | `Đã thu` / `Chưa thu` | `Hoàn thành` khi nói tiền |
| Chuyển folder | `Chuyển đến thư mục` | "Tiến hành điều hướng tập tin" |
| Lưu máy | `Đã lưu trên thiết bị` | `Đã đồng bộ` khi đang offline |
| Đồng bộ xong | `Đã đồng bộ` | `Đã lưu` khi cloud chưa xong |
| Xoá mềm | `Chuyển vào Thùng rác` | `Xoá vĩnh viễn` |
| Khôi phục | `Khôi phục` | "Lấy lại thành công rồi nhé!" |
| OCR | `Trích xuất chữ` | "Scan Word" cho DOCX |
| Tạo liên kết | `Thêm liên kết` | Câu văn nói |
| Chế độ editor | `Xem trước` / `Mã Markdown` / `Chỉ đọc` | "Đơn giản hay Markdown?" |

**Text audit:** bỏ slogan thừa dưới input/nút, văn nói, hướng dẫn kỹ thuật lộ trên UI mặc định, thông báo mơ hồ. Một hành động luôn cùng tên xuyên suốt app.

### 12.4 Văn phong trạng thái
- Home rỗng: `Chưa có ghi chú` + `Tạo ghi chú`.
- Search rỗng: `Không tìm thấy kết quả` + `Xoá bộ lọc` (khi liên quan).
- Offline: `Đang ngoại tuyến. Thay đổi được lưu trên thiết bị.`
- Upload lỗi: `Không thể tải [tên tệp]. Thử lại`.
- Link công khai vô hiệu: `Liên kết không còn hiệu lực.`
- Xung đột: `Ghi chú có thay đổi trên hai thiết bị. Xem để hợp nhất hoặc giữ cả hai bản.`
- OCR không có: `Không thể trích xuất chữ trên thiết bị này. Tệp gốc vẫn được giữ.`

---

## 13. MOBILE vs WEB: KHÁC BIỆT, RESPONSIVE, PARITY

| Hạng mục | Mobile App | Web App |
|---|---|---|
| Bố cục | 1 cột; nav dưới (Ghi chú · Tìm kiếm · **+** · Nhắc nhở · Thư viện) | Sidebar · Danh sách · Editor |
| Tạo note | `+` giữa thanh dưới; widget | Nút "Ghi chú mới"/`+`; **Ctrl+Alt+N** |
| Thao tác nhanh | Vuốt trái: Xoá · Vuốt phải: Ghim/Lưu trữ · Nhấn giữ: chọn nhiều | Chuột phải: menu ngữ cảnh · Kéo thả note vào folder · Shift/Ctrl+click chọn nhiều |
| Scan | Camera | Tải ảnh/PDF lên + OCR |
| Bảng vẽ | Ngón tay/bút | Chuột/bút/bảng vẽ điện tử |
| Thông báo | Push/local notification | Thông báo trình duyệt |
| Phím tắt | — | Xem Phụ lục B |

### 13.1 Responsive
- **Desktop ≥ 1200 px:** tối đa 3 vùng khi đủ nội dung; sidebar và inspector thu gọn được.
- **Tablet 768–1199 px:** 1–2 vùng; panel phụ qua drawer/inspector.
- **Mobile Web < 768 px:** 1 cột như app; không dùng hover-only.
- iOS/Android: tôn trọng status bar, safe-area đáy, bàn phím; chữ xuống dòng khi zoom 200%/font lớn; landscape không cắt control.
- Inspector vắng → editor mở rộng *trong độ rộng dòng dễ đọc*; sidebar vắng → workspace dùng hết chiều rộng; không để cột trống chỉ vì mockup gốc chia 1:2:1.

### 13.2 Parity bắt buộc
| Chức năng | Mobile | Web |
|---|---|---|
| Mở note | Chạm card | Click card/dòng |
| Hành động card | `⋯` / nhấn giữ | `⋯` / chuột phải |
| Sort/Filter | Sheet gọn | Popover toolbar |
| Chèn ảnh/tệp | Camera/Ảnh/Chọn tệp | Dán/kéo thả/chọn tệp |
| OCR | Document workspace native | Viewer + panel OCR |
| Ký hiệu toán | Bàn phím phụ/sheet | Autocomplete + palette popover |
| Vẽ | Cảm ứng/stylus toàn màn hình | Chuột/stylus workspace |
| Bảng | Cuộn ngang, sửa ô | Đổi độ rộng cột, sửa inline |
| Nhắc nhở | Sheet ngày/giờ, local scheduler | Picker, thông báo trình duyệt |
| Properties | Section thu gọn / sheet | Inspector/section thu gọn |
| Chia sẻ | OS share sheet | Dialog chia sẻ/link |
| Graph/Kanban | View state nâng cao | View state trong workspace |
| Split view | Không bắt buộc trên điện thoại hẹp | Có (desktop/tablet) |

### 13.3 Chất lượng Web riêng
- **Dùng được hoàn toàn bằng bàn phím**, có focus ring, xử lý Esc, nhãn đọc màn hình cho icon-only; mọi tính năng **không phụ thuộc kéo thả** (có menu/bàn phím tương đương).
- Hover/click phải/chọn con trỏ; bảng rộng giữ cột đọc được, cuộn ngang khi cần, sticky header/cell nếu hỗ trợ.
- Phím tắt có tài liệu và (nếu có thể) cho phép đổi.

---

## 14. HỢP ĐỒNG THIẾT KẾ MOBILE (PAGE 04) & DANH SÁCH MÀN HÌNH

### 14.1 Ánh xạ 10 section của Figma Page 04 → surface thật
> Giữ tên section để quản lý thiết kế; **không** biến section thành màn hình điều hướng.

| Section | Chức năng phải thể hiện | Surface chủ đạo |
|---|---|---|
| 01 · Entry & browse | Home, Grid/List/Calendar, card, Sort/Filter, overflow | M-A + overlay ngữ cảnh |
| 02 · Create, write & organize | Tạo, editor, Markdown/LaTeX, checklist, properties, tag, liên kết, focus | M-B + state chèn/chế độ/chọn |
| 03 · Capture files, handwriting & voice | Camera, scan, OCR, PDF/DOCX, ghi âm, vẽ | M-B + M-I khi cần + M-J khi cần |
| 04 · Find content & manage folders | Search, folder/tag, màu, backlink | M-C, M-E, menu inline |
| 05 · Tasks, reminders & notifications | Nhắc task, lịch, nhóm, trạng thái thông báo | M-D + sheet inline + mẫu thông báo OS |
| 06 · Share & export | Share OS, link/QR/thu hồi, PDF/MD/ZIP | Menu note → sheet + share sheet OS |
| 07 · Sync, conflict & version recovery | Lưu/sync/offline/lỗi/xung đột/lịch sử | State không chặn ở Home/editor; sheet so sánh khi thật cần |
| 08 · Delete, undo & restore | Thùng rác, khôi phục, lưu trữ, hoàn tác | Context menu + Library |
| 09 · Settings & writing comfort | Theme, giấy, cỡ chữ, thông báo, sync, bảo mật, tài khoản | M-F nhóm section |
| 10 · Predefined templates | Danh sách, xem trước, áp dụng, mẫu riêng, ví dụ | Library/Template picker → Editor đã điền |

### 14.2 Hợp đồng chi tiết từng section
**01 · Entry & browse — thành phần Home:**
1. Status bar OS; header an toàn: thương hiệu/`Ghi chú`, icon Search, `⋯` top-level (khác `⋯` card).
2. Dưới header: số lượng/lần sync cuối dạng metadata nhẹ; View Switcher gọn `Lưới · Danh sách · Lịch`; Sort/Filter gần toolbar collection.
3. Chip filter cuộn ngang; mục Đã ghim; Gần đây/Tất cả; card chạm được, có `⋯` góc phải trên.
4. Bottom nav bền vững: `Ghi chú · Tìm kiếm · + · Nhắc nhở · Thư viện`.
5. Các state: Grid/List/Calendar, sheet Sort, sheet Filter, menu card, menu `⋯` Home, rỗng/lỗi/offline.

**Nghiêm cấm:** overlay "Hiển thị ghi chú" như màn điều hướng; `Context Menu · Tùy chọn ⋯` hiển thị như điện thoại đen rỗng với sheet trôi; sort/note actions/calendar settings thành điểm đến độc lập; cắt toolbar/card/chip; chồng tiêu đề; bottom nav ra ngoài safe area; sheet lồng sheet cho một lựa chọn đơn giản. **Flow QA:** Home → Grid/List/Calendar → chọn note → Editor → Back giữ scroll/filter/view.

**02 · Create, write & organize:**
- `+` mở quick-create (`Ghi chú` chính; `Checklist`, `Giọng nói`, `Ảnh`, `Scan`, `Bản vẽ`, `Từ mẫu` — gọn nhưng không giấu chức năng).
- Một Editor: tiêu đề sửa trực tiếp, caret ở thân, thanh định dạng khi mở bàn phím, sheet chèn ngữ cảnh, Properties thu gọn, bộ chuyển chế độ. Không màn "New Note welcome".
- Bảng ký hiệu toán là extension bàn phím hoặc sheet *chồng lên editor*; không trang `Công cụ soạn thảo`/`Chèn công thức`.
- Tag/folder/màu/ghim/backlink qua menu note và Properties thu gọn, **không** inspector ma hiển thị note khác.
- Có ví dụ state Reading / Live Preview / Source, media/link/bảng/checklist trong **cùng mô hình editor**; chỉ báo autosave đúng chữ; Reading không cho sửa nhầm.

**03 · Capture:** Insert → Camera/Ảnh/Tệp native; hiện ảnh/tệp inline sau khi chọn; action sheet PDF/DOCX chỉ khi cần chọn Đính kèm/Xem trước/OCR/Markdown (chạm PDF đơn thuần → mở reader mặc định, không wizard); Document Workspace chỉ cho nhiều trang/OCR review/sắp xếp trang/chú thích; ghi âm là thanh ghi trong editor (sóng âm/bản chép/player); Drawing toàn canvas, hỗ trợ lớp/dữ liệu sửa được, quay lại đúng vị trí.

**04 · Find & organize:** Search qua tab/icon, input focus, kết quả có recent/OCR/filter; Library chứa folder/tag; `⋯` folder → **một sheet nhỏ** (đổi tên/icon/màu) với nút liền kề trường; backlinks nằm trong note; bảng chọn màu nhất quán token.

**05 · Tasks & reminders:** checkbox/đồng hồ inline → picker ngày/giờ + lặp gọn; Reminders có nhóm và hành động nhanh; mẫu thông báo giống OS và **ghi rõ là mẫu native**; `Đã hoàn thành` vs `Đã thu` đúng ngữ nghĩa; pending/overdue/snoozed không dùng chung nhãn mơ hồ.

**06 · Share & export:** menu `Chia sẻ` → share native/link công khai với hạn/thu hồi/QR; PDF có cấu hình + xem trước (giữ bảng/công thức/checklist); MD/ZIP nếu phù hợp; QR **ở giữa trong khung điện thoại, đủ vùng im lặng**; xong quay về note.

**07 · Sync & version:** trạng thái local tinh tế; banner offline chỉ khi offline; thử lại; so sánh hai bản; khôi phục không phá huỷ (mặc định); khôi phục tạo phiên bản mới có xác nhận.

**08 · Delete & restore:** Xoá → Thùng rác + snackbar 5 giây; Thùng rác ở Library; xoá vĩnh viễn có xác nhận; Lưu trữ khác Thùng rác và đảo ngược được.

**09 · Settings:** một danh sách section; theme/cỡ chữ/giấy xem trước trực tiếp; đồng bộ theme với hệ thống; giờ im lặng; tài khoản/phiên; khoá note; nhập/xuất. **Không** khung full-screen cho bộ chọn 3 lựa chọn, một color picker hay một công tắc.

**10 · Templates:** duyệt theo loại → xem trước cấu trúc → `Dùng mẫu` → editor đã điền; lưu/sửa/xoá mẫu riêng; ví dụ hoá đơn/học phí/điểm danh; xem trước là state thật của picker/editor; **không** màn hình hỏi "Bạn muốn làm gì?" sau khi đã chọn mẫu; bảng mobile cuộn ngang, số căn phải, dòng tổng nổi bật, công thức đúng.

### 14.3 Nghiệm thu Page 04
- Cả 10 section có điểm vào/ra và điều hướng Back thật; **không** là "kho màn hình" giả làm wireflow.
- Không còn: welcome thừa, xác nhận loại note, overlay hiển thị view, màn lưu, trang công cụ toán rời, trang màu folder.
- Mọi sheet gắn với trigger và có dismiss; menu ngữ cảnh Home và từng card được định nghĩa riêng.
- Nhãn tiếng Việt chuyên nghiệp, không tràn/chồng/cắt QR, không "dữ liệu ma" từ note khác, không UI giả chức năng.
- C1–C10, F1–F33 và P1/P2 đều truy vết được tới một surface/state Mobile; giới hạn OS ghi rõ là phụ thuộc/fallback.

### 14.4 Danh sách màn hình cần thiết kế (rút gọn theo kiến trúc)
**Mobile:** Auth (login/register/reset) · Home (Grid/List/Calendar/Kanban/Graph, Đã ghim, chip filter) · Menu `+` · Editor (toolbar, ảnh, bảng, checklist, `[[`, "Được liên kết từ", Properties thu gọn, bộ chuyển Live/Source/Reading, gợi ý `[[ # \ @ :`, bảng ký hiệu LaTeX + hàng phím phụ, menu `/`, callout/tô sáng/bảng tính, nhúng note, query, cheat sheet) · Ghi giọng nói · Camera/viewer ảnh · Scan (mép → khung → lọc → trang → OCR) · Bản vẽ · Template library · Search (rỗng/đang gõ/có kết quả/không kết quả) · Sort/Filter sheet · Nhắc nhở (đặt + danh sách + thông báo màn khoá) · Library (Folder/Tag/Tệp/Mẫu/Lưu trữ/Thùng rác) · Chia sẻ & Xuất PDF · Focus mode · Cài đặt · Trạng thái (offline, xung đột, rỗng, lỗi, xin quyền) · Widget · Public share viewer.

**Web:** Auth · Dashboard (Sidebar · Danh sách · Editor; Grid/List) · Calendar toàn màn · Ctrl+K palette tìm kiếm & lệnh nhanh · Editor đầy đủ (panel Liên kết) · Tải lên & scan OCR · Bản vẽ · Template library · Đặt nhắc & trang nhắc nhở · Dialog Chia sẻ & Xuất PDF · Focus mode · Quản lý Folder/Tag · Thùng rác/Lưu trữ · Cài đặt + danh sách phím tắt · Trang xem note qua link chia sẻ · Trạng thái offline/xung đột/rỗng/lỗi · Responsive Tablet & Mobile web · Editor 3 chế độ (Source có số dòng) + Properties + gợi ý + LaTeX palette + `/` + nhúng + query/Mermaid + cheat sheet (Ctrl+/) + nhập/xuất `.md`.

*(Đây là danh sách **bề mặt/trạng thái** cần có, không phải số lượng frame độc lập; xem luật mục 4.)*

---

## 15. YÊU CẦU PHI CHỨC NĂNG

| Hạng mục | Yêu cầu |
|---|---|
| **Hiệu năng** | Mở app ≤ 2 s · Mở note ≤ 0,5 s · Tìm kiếm ≤ 300 ms (1.000 note) · Cuộn mượt 60 fps |
| **Offline** | Mọi chức năng chạy khi mất mạng, **trừ**: đăng nhập lần đầu, tạo link chia sẻ; OCR/chuyển đổi mới phụ thuộc engine cục bộ thực tế (báo đúng trạng thái) |
| **Toàn vẹn dữ liệu** | Tự lưu liên tục · Thùng rác 30 ngày · Không mất dữ liệu khi app tắt đột ngột · Xuất toàn bộ dữ liệu (ZIP: md + tệp + metadata, tuỳ chọn PDF) |
| **Bảo mật** | HTTPS · Mật khẩu ≥ 8 ký tự · Link chia sẻ ngẫu nhiên, thu hồi được · Đăng xuất từ xa · Bảo vệ ZIP/tệp xuất theo quyền hiện tại |
| **Tiếp cận** | Vùng chạm ≥ 48×48 px · Tương phản WCAG AA · Icon luôn có nhãn/tooltip · VoiceOver/TalkBack · Web dùng được 100% bằng bàn phím, focus rõ · Không chỉ dựa vào màu |
| **Ngôn ngữ** | Tiếng Việt (mặc định), Tiếng Anh |
| **Tương thích** | iOS 16+, Android 10+; Chrome, Edge, Safari, Firefox (2 phiên bản mới nhất) |
| **Hiệu năng tệp** | Ảnh tạo thumbnail; PDF render/lazy-load từng trang; OCR/lập chỉ mục chạy nền, huỷ được, không khoá editor |

---

## 16. LƯU TRỮ, BẢO MẬT, TOÀN VẸN DỮ LIỆU

### 16.1 Lưu trữ
- Text lưu Markdown; metadata ở frontmatter; nhị phân trong `attachments/`; xuất portable kèm manifest ánh xạ ID ổn định. **Không** base64 cả file vào `.md`. Giữ nguyên bản gốc cho chuyển đổi/chú thích.
- Sanitize HTML/SVG; kiểm MIME + đuôi; từ chối payload thực thi; không chạy macro/script.
- Ghi **nguyên tử** note + metadata; hàng đợi upload/thử lại idempotent (ID); chống tệp mồ côi và xoá khi còn tham chiếu.
- Giới hạn từng loại MIME phải cấu hình và thông báo; chỉ ảnh có mốc cố định 10 MB/ảnh, 10 ảnh/lần.
- Thành phần không chuẩn Markdown phải có fallback xuất/nhập.

### 16.2 Offline & nhất quán
- Offline: sửa local, tìm trong nội dung đã lập chỉ mục, xem PDF đã cache, tạo/sửa task & nhắc nhở cục bộ (khi OS hỗ trợ). Sync thử lại không mất draft. Xung đột: hỏi người dùng hoặc tạo bản sao an toàn, **không bỏ** thay đổi nào.

### 16.3 Chia sẻ & riêng tư
- Link nội bộ không cấp quyền truy cập. Link công khai là hành động chia sẻ **rõ ràng**, có hạn/thu hồi. Note khoá không lộ qua preview, thông báo, kết quả tìm kiếm hay link công khai nếu chính sách cấm. HTTPS, đăng xuất phiên, quyền theo xác thực hiện tại.

---

## 17. MA TRẬN KIỂM THỬ & CHẤP NHẬN

### 17.1 Kịch bản đầu-cuối
| ID | Kịch bản | Kết quả mong đợi |
|---|---|---|
| T01 | Mở app đã đăng nhập | Vào Home (không Welcome), khôi phục folder/view |
| T02 | Home `+` → Ghi chú | Editor mở, caret ở dòng tiêu đề, không trang trung gian |
| T03 | Tiêu đề trống | Hiển thị "Ghi chú không tên", không chặn sửa |
| T04 | Gõ → chờ 1 s → Back | Lưu local; mở lại còn nội dung; không nút Lưu |
| T05 | Đổi Grid/List/Calendar | Cùng ngữ cảnh Home, giữ lựa chọn |
| T06 | Chạm card/dòng | Mở note trực tiếp, không nút "Mở" |
| T07 | `⋯` Home | Chỉ tuỳ chọn phụ của collection, không lặp view/sort/settings |
| T08 | `⋯` card | Chỉ thao tác của note đó |
| T09 | Sort/filter | Áp dụng trong một lớp tương tác, không trang riêng |
| T10 | Sửa folder | Một sheet ngắn tên/icon/màu |
| T11 | Scan 2 trang | Crop/sắp xếp/OCR/xem lại, gốc an toàn, chữ tìm được |
| T12 | Dán screenshot | Hiện tại caret, còn sau reload như attachment thật |
| T13 | Nhập PDF/DOCX | Phân biệt đính kèm / xem trước / trích chữ / chuyển đổi |
| T14 | Wiki-link A→B | B hiện backlink A; đổi tên không gãy |
| T15 | Gõ `\fr` | Gợi ý + preview trong editor, không đi trang toán |
| T16 | Bảng Markdown | Hàng/cột rõ ở cả hai theme; tổng đúng |
| T17 | Nhắc task | Đúng giờ/ngày/lặp; ngữ nghĩa thông báo đúng |
| T18 | Dùng template | Chọn một lần → xem trước → dùng → editor đã điền |
| T19 | Chia sẻ & thu hồi | Link chỉ đọc theo hạn/thu hồi; QR khớp link |
| T20 | Xuất PDF hoá đơn | Bảng/tổng/công thức giữ nguyên, dễ đọc |
| T21 | Làm việc offline | Lưu local, hàng đợi sync, đồng bộ lại khi có mạng |
| T22 | Xung đột 2 thiết bị | Giữ cả hai bản, so sánh/khôi phục được |
| T23 | Xoá note | Thùng rác + Hoàn tác 5 s; xoá vĩnh viễn có xác nhận riêng |
| T24 | Sáng/Tối | Đúng token, focus/selected, lưới tinh tế, bảng đọc được |
| T25 | Cỡ chữ Rất lớn | Chữ xuống dòng, không cắt/đè, tôn trọng safe area |
| T26 | Mobile sheet | Mở trên parent, đóng đúng, không màn đen giả |
| T27 | Back ở mọi màn | Về đúng note, caret, folder, filter, scroll, view |
| T28 | Tìm OCR offline | Kết quả với ngữ cảnh khớp (đã lập chỉ mục) |
| T29 | Khoá note | Không lộ preview trước khi mở; có dự phòng sinh trắc |
| T30 | Sao chép/Xuất Markdown | `.md` portable có frontmatter + tham chiếu tệp |
| T31 | Graph/Kanban/Daily/Flashcards | Khám phá được; không phá luồng Home cốt lõi |
| T32 | Vẽ/chú thích | Lớp mở lại sửa được; file gốc không đổi |
| T33 | Share từ app khác | Quick Capture lấy chữ/link/ảnh, quay lại hợp lý |

### 17.2 Hồi quy thị giác bắt buộc
1. Khung thiết bị: không viền ngoài, không sheet/nút/QR bị cắt; tôn trọng inset an toàn.
2. Card: tiêu đề/preview không chồng; cắt/xuống dòng hợp lý; gesture card không nuốt `⋯`.
3. Header: Search và `⋯` tách bạch, không dính thành một control mơ hồ.
4. Bottom nav: icon + nhãn rõ, trạng thái chọn; `+` là hành động, không phải tab vô nghĩa.
5. Sáng/Tối: các lớp nền thấy được, không quá tím/xám bão hoà, chữ đủ tương phản.
6. Lưới: chỉ tinh tế trong editor; nền bảng đục; không có đường kẻ xuyên checklist/bảng.
7. Dialog/sheet ngắn: cao vừa nội dung, nhóm control, Lưu/Huỷ liền kề, không khoảng trống dọc lớn.
8. Form: không nhãn lặp, không caption thừa; tên đọc màn hình ổn định.
9. Wireflow Figma: có START, trigger, nhánh, đường quay lại thật; **không** khung mồ côi hoặc "màn đen chứa sheet".
10. Trạng thái loading/empty/error/offline/permission/conflict nằm trong màn liên quan, không thành trang điều hướng mới.
11. Chữ lớn, landscape, bàn phím mở, cuộn + control dính đều được kiểm thử.
12. Bảng: chữ trái, số phải, nhãn trạng thái ngữ nghĩa, tổng thấy được và căn thẳng.

### 17.3 QA chéo nền tảng
- Camera/thư viện iOS/Android và trạng thái từ chối quyền.
- Chrome/Edge/Safari/Firefox: nhập tệp, quyền clipboard, phím tắt.
- PDF có text layer vs PDF ảnh scan; PDF mật khẩu; DOCX có bảng lồng; HEIC; thiếu codec/fallback.
- Offline → sửa 2 thiết bị → kết nối lại → xung đột; hàng đợi sync; tệp chưa upload.
- Đổi Sáng/Tối khi editor đang mở; focus mode với bàn phím; đổi tên note có wiki-link; file trùng tên; tệp quá lớn; OCR lỗi; round-trip xuất/nhập.
- Tiếp cận: thứ tự Tab, VoiceOver/TalkBack, vùng chạm 48 px.

---

## 18. QUYẾT ĐỊNH KỸ THUẬT CÒN MỞ & PHỤ THUỘC TRIỂN KHAI

Phải được nhóm quyết định; **không được tự giả định hay lặng lẽ cắt chức năng**:
1. **Hạn mức dung lượng** cho từng loại tệp không phải ảnh (PDF, DOCX, XLSX, PPTX, audio, video).
2. **Engine OCR/Office** và ma trận hỗ trợ offline; có cho server xử lý không (với sự đồng ý rõ ràng của người dùng).
3. **Chính sách khoá note:** khôi phục mật khẩu/sinh trắc, che preview, lập chỉ mục, thông báo, chia sẻ công khai.
4. **Độ trung thực** khi chuyển DOCX/PDF phức tạp (bảng lồng, toán, đính kèm nhúng) sang Markdown; có cần hỗ trợ `.doc` cũ hay chỉ `.docx`.
5. **Cơ chế gộp xung đột & lưu trữ/đồng bộ:** gộp theo dòng, quy tắc giữ tệp đính kèm, ID ổn định khi file đổi tên.
6. **Giới hạn OS/trình duyệt:** thông báo, share target, xem trước PDF, kéo thả, clipboard.
7. **Ngưỡng QA** cho tính năng tuỳ chọn phức tạp (split view chỉ desktop/tablet; OCR offline có điều kiện).
8. **Phím tắt Ctrl+K theo ngữ cảnh** (đã chốt hướng: ngoài editor = tìm; trong editor có vùng chọn = liên kết) — cần chuẩn hoá hiển thị trong trợ giúp.
9. **Xác thực:** Apple Sign-in và các hình thức bổ sung chưa quyết định.
10. **Phạm vi prototype thực tế** cho các hạng mục tuỳ chọn.

---

## 19. HƯỚNG DẪN THỰC THI THIẾT KẾ TRÊN FIGMA

### 19.1 Phạm vi làm việc
- Chỉ chạm vào **page được giao**: `04 · UI Prototype` = Mobile (6:5); `06 · Web — UI Prototype` = Web (64:3). Không sửa page khác khi chưa được yêu cầu; nếu được giao cả hai, đảm bảo parity nhưng **không nhân đôi** mọi trang mobile thành trang web.
- Kiểm tra frame/component hiện có trước; không dựng lại toàn bộ app thành file Figma mới. Giữ tên frame ổn định, tái dùng component và variables; không xoá phá huỷ trước khi ánh xạ xong mọi chức năng.

### 19.2 Thực hiện theo checkpoint nối tiếp (làm toàn bộ nhưng an toàn)
1. Kiểm kê frame ở 10 section → ánh xạ vào C/F/P → phân loại `screen/state/overlay/component`.
2. Sửa kiến trúc Home/Collection; bỏ hành vi menu/màn hình thừa. Kiểm bằng ảnh chụp.
3. Sửa Tạo note/Editor/Autosave. Kiểm.
4. Tích hợp Markdown/Toán/Ảnh/Bảng vào ngữ cảnh editor. Kiểm.
5. Sửa Search/Folder/Tag/Link/Menu note. Kiểm.
6. Sửa Camera/Scan/PDF/DOCX/OCR/Voice/Drawing/Đính kèm. Kiểm.
7. Sửa Task/Nhắc nhở/Thông báo. Kiểm.
8. Sửa Template/Chia sẻ/Xuất/Sync/Xung đột/Lưu trữ/Thùng rác/Cài đặt. Kiểm.
9. Áp Color System v3 toàn trang, giấy note, bảng đọc được, hệ typography/nút. Kiểm.
10. Tổ chức lại thành Wireflow có nhãn trigger/back/overlay; đánh giá bao phủ chức năng; báo cáo trung thực.

Mỗi checkpoint: **kiểm tra → sửa node cụ thể → ảnh chụp + kiểm cấu trúc → ghi ID đã đổi → tiếp tục**. Không gọi một section là hoàn thành khi mới sửa một phần.

### 19.3 Chính sách xoá frame
- Chỉ đưa frame ra khỏi flow chính khi các hành động duy nhất của nó đã chuyển sang bề mặt hợp lệ. Tái dùng tài sản UI cũ như component; không giữ bản sao chết chỉ vì đã tồn tại.
- Giữ **Frame audit:** `frame cũ (ID/tên) → bề mặt/state mới + hành động giữ nguyên + lý do xoá/gộp`.
- Overlay chỉ để tài liệu hoá thì parent phải hiện rõ và trigger có chú thích; nó không bao giờ là route riêng.

### 19.4 Đầu ra bắt buộc khi hoàn tất
1. Kiểm kê đầy đủ 10 section và phần nào đã cập nhật.
2. Ma trận bao phủ C1–C10, F1–F33 và P1/P2 duy nhất → frame/surface đích chính xác.
3. Frame bị xoá/gộp + tương tác thay thế.
4. Phụ thuộc engine còn lại / giới hạn thiết bị (không tuyên bố code chạy được).
5. Các vấn đề UX đã xử lý (không overlay view giả, không sheet rời, không nút full-width quá cỡ, không UI bị cắt, bảng đủ tương phản).
6. Ảnh chụp/bằng chứng: Home, Editor, Document Workspace, Template, Reminders.
7. Xác nhận page ngoài phạm vi không bị chạm.
8. Giá trị token mới đã áp và vấn đề tiếp cận còn tồn đọng.

### 19.5 Chỉ dẫn mẫu cho agent thiết kế
> Bạn đang làm việc trên dự án Figma Opportunities hiện có. Coi tài liệu này là hợp đồng chức năng & UX thống nhất. Kiểm tra mọi frame liên quan trước khi sửa. Page được giao: **04 · UI Prototype (Mobile, ID 6:5)**. Hoàn tất toàn bộ refactor 10 section theo các checkpoint tuần tự, có kiểm chứng. Ưu tiên hàng đầu: **loại bỏ màn hình trung gian giả tạo, bottom sheet rời, hành động trùng lặp.** Không dựng lại từ đầu; không tạo page Figma mới cho việc nhỏ; không đụng Page 06 hay page khác. Giữ mọi chức năng bằng cách gộp vào màn hình/state/overlay thật. Dùng Color System v3, giấy note nhận diện được và bảng dễ đọc. Ánh xạ rõ C1–C10, F1–F33 và P1/P2 duy nhất vào các surface Mobile thực. Không coi frame tĩnh là chức năng chạy thật. Khi xong, cung cấp bản ánh xạ có thể kiểm toán + bằng chứng thị giác; báo cáo trung thực các phụ thuộc và hạng mục chưa xong.

---

## 20. ĐỊNH NGHĨA "HOÀN THÀNH" (DEFINITION OF DONE)

Tài liệu/thiết kế đạt yêu cầu khi:
- Mọi chức năng gốc và mở rộng truy vết được tới điểm vào Desktop/Mobile hợp lý (gồm trạng thái lỗi và thoát).
- Home là trung tâm điều hướng thật; `⋯` của Home và `⋯` của note là **hai menu có phạm vi khác nhau, rõ ràng**.
- View/sort/filter/actions không bị thể hiện thành hàng loạt màn hình giả toàn màn hình.
- Trình soạn thảo Markdown là **MỘT editor**, không có loại note riêng hay trang toán phụ.
- Hành vi lưu trung thực, local-first, không có nút Lưu thừa.
- Tệp/PDF/DOCX/OCR/liên kết/chú thích đúng khái niệm, **không bịa backend đã xong**.
- Cả hai theme dùng token v3 và control tiếp cận được; lưới giấy nhận diện nhưng không gây nhiễu; bảng đọc rõ.
- Bố cục Web nêu mục đích từng vùng và **không kéo giãn form ngắn** toàn viewport.
- Hành trình Mobile Page 04 là **luồng người dùng thật** với điều khiển trực tiếp; đủ 10 section.
- Chức năng được thể hiện đầy đủ mà không gây "lạm phát màn hình" hay điều hướng trùng lặp.

---

## PHỤ LỤC A — BẢNG TỔNG HỢP CHỨC NĂNG

| ID | Chức năng | Mobile | Web | Ưu tiên kế hoạch kỹ thuật (tham khảo) |
|---|---|---|---|---|
| C1–C10 | Nền tảng | ✔ | ✔ | Must |
| F1 | Speech-to-text | ✔ | ✔ | Must (giữ file ghi âm: Should→Must nhờ P1.2) |
| F2 | Chụp ảnh | ✔ | ✔ (tải lên/dán) | Must |
| F3 | Scan + OCR | ✔ | ✔ (tải lên + OCR) | Must |
| F4 | Checklist | ✔ | ✔ | Must |
| F5 | Bảng vẽ | ✔ | ✔ | Must (lasso/lực nhấn: Should) |
| F6 | Template & bảng | ✔ | ✔ | Must |
| F7 | Folder | ✔ | ✔ | Must |
| F8 | Tag | ✔ | ✔ | Must |
| F9 | Ghim | ✔ | ✔ | Must |
| F10 | Màu note | ✔ | ✔ | Must |
| F11 | Backlink | ✔ | ✔ | Must |
| F12 | Grid/List/Calendar | ✔ | ✔ | Must |
| F13 | Search | ✔ | ✔ | Must |
| F14 | Sort | ✔ | ✔ | Must |
| F15 | Quick filter | ✔ | ✔ | Must (Bộ lọc của tôi: Should) |
| F16 | Notification theo lịch | ✔ | ✔ (trình duyệt) | Must |
| F17 | Chia sẻ | ✔ | ✔ | Must (nhận share: Should→Must nhờ P1.8) |
| F18 | Xuất PDF/MD/Backup | ✔ | ✔ | Must |
| F19 | Đồng bộ & offline | ✔ | ✔ | Must |
| F20 | Focus mode | ✔ | ✔ | Must (hẹn giờ: Should) |
| F21 | Cỡ chữ | ✔ | ✔ | Must |
| F22 | Sáng/Tối | ✔ | ✔ | Must |
| F23 | Markdown + LaTeX | ✔ | ✔ | Must (Query block, Mermaid: Should) |
| F24 | Kéo thả/Dán | ✔ | ✔ | Must |
| F25 | Đính kèm & xem trước | ✔ | ✔ | Must |
| F26 | Nhập tài liệu/OCR/chuyển đổi | ✔ | ✔ | Must |
| F27 | Liên kết/Deep link | ✔ | ✔ | Must |
| F28 | Chỉnh khối | ✔ | ✔ | Should |
| F29 | Web clipper | ✔ | ✔ | Should |
| F30 | Thư viện tệp | ✔ | ✔ | Should |
| F31 | Lịch sử phiên bản | ✔ | ✔ | Should |
| F32 | Chú thích ảnh/PDF | ✔ | ✔ | Should |
| F33 | Tiện ích hằng ngày | ✔ | ✔ | Could (split view: desktop/tablet) |

*(Cột ưu tiên chỉ để lập kế hoạch kỹ thuật; **không** dùng để cắt phạm vi thiết kế — xem 0.4.)*

---

## PHỤ LỤC B — BẢNG PHÍM TẮT WEB

| Phím | Chức năng |
|---|---|
| **Ctrl/Cmd+K** (ngoài editor) | Mở tìm kiếm & lệnh nhanh |
| **Ctrl/Cmd+K** (trong editor, có vùng chọn) | Tạo/sửa liên kết |
| **Ctrl+Alt+N** | Ghi chú mới |
| **Ctrl+E** | Chuyển chế độ Live Preview / Source / Reading |
| **Ctrl+Shift+F** | Focus mode (Esc để thoát) |
| **Ctrl+/** | Bảng tra cú pháp & phím tắt |
| Ctrl+Z / Ctrl+Shift+Z | Undo / Redo |
| Ctrl+B / Ctrl+I / Ctrl+U | Đậm / Nghiêng / Gạch chân |
| Ctrl+Shift+X | Gạch ngang |
| Ctrl+Shift+H | Tô sáng |
| Ctrl+` | Code trong dòng |
| Ctrl+1 … Ctrl+6 | Heading 1–6 |
| Ctrl+Shift+V | Dán chữ thuần |
| Tab / Shift+Tab | Thụt lề danh sách/checklist; nhảy ô bảng; nhảy ô công thức |
| `[[` · `/` · `$` · `#` · `@` · `:` · `\` | Gợi ý liên kết · menu khối · công thức · tag · ngày giờ · emoji · lệnh LaTeX |

---

## PHỤ LỤC C — BẢNG CÚ PHÁP MARKDOWN MỞ RỘNG NHANH

| Mục đích | Cú pháp |
|---|---|
| Tiêu đề | `# H1` … `###### H6` |
| Đậm / nghiêng / gạch ngang | `**a**` · `*a*` · `~~a~~` |
| Tô sáng | `==a==` |
| Mũ / chỉ số | `x^2^` · `H~2~O` |
| Checklist | `- [ ] việc` / `- [x] xong` |
| Nhắc nhở / hạn / lặp (mục) | `⏰ 2026-01-12 20:00` · `📅 2026-01-13` · `🔁 hằng tuần` |
| Callout | `> [!tip] Tiêu đề` · thu gọn `[!tip]-` / mở `[!tip]+` |
| Mục lục | `[TOC]` |
| Bình luận ẩn | `%% … %%` |
| Chú thích cuối trang | `chữ[^1]` / `[^1]: Nội dung` |
| Bảng + công thức ô | `\| A \| B \|` + `=SUM(C2:C5)` |
| LaTeX | `$…$` · `$$…$$` · `\tag{1}` |
| Liên kết note | `[[Note]]` · `[[Note\|alias]]` · `[[Note#Heading]]` · `[[Note#^id]]` |
| Nhúng | `![[Note]]` · `![[ảnh.png\|300]]` · `![[tài-liệu.pdf]]` |
| Tag | `#hoc-tap` · `#hoc-tap/hci` |
| Frontmatter | `tags`, `color`, `pinned`, `folder`, `aliases`, `remind` |
| Biến mẫu | `{{date}}` · `{{time}}` · `{{title}}` · `{{so_hoa_don}}` |
| Query | ` ```query ` … ` ``` ` |
| Sơ đồ | ` ```mermaid ` … ` ``` ` |
| Flashcard | `Câu hỏi :: Đáp án` |
| Tìm kiếm nâng cao | `tag:hci` · `has:image` · `has:math` · `task:todo` · `"cụm"` · `-loai-tru` |

---

**HẾT — MASTER SPEC v5.0**
