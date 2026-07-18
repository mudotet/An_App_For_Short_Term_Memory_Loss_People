<div align="center">

# 🧠✨ Trợ lý Trí nhớ

### Nhận diện người quen · Ghi nhớ hội thoại · Nhắc lại đúng lúc

Một demo kính thông minh giúp người gặp khó khăn về trí nhớ ngắn hạn nhận ra người đối diện và nhớ lại nội dung lần trò chuyện trước.

`📷 Face Recognition` · `🎙️ Vietnamese STT` · `✨ GPT Summary` · `🐘 PostgreSQL` · `🔎 pgvector`

</div>

---

## 🎬 Xem demo

![Demo Trợ lý Trí nhớ](<docs/assets/Screen Recording 2026-07-18 at 14.11.30.gif>)

## 🌟 Ứng dụng làm được gì?

- 👤 Nhận diện nhiều khuôn mặt bằng embedding 128 chiều.
- 💬 Hiển thị một bong bóng gradient với tên, transcript và nội dung lần gặp trước.
- 🪪 Tự đổi “Người quen” thành tên khi nghe rõ câu giới thiệu.
- ⏱️ Cho biết hai người đã nói chuyện cách đây bao lâu.
- 🎙️ Nhận giọng nói tiếng Việt bằng `faster-whisper`.
- 🛡️ Bỏ qua âm thanh thiếu tin cậy thay vì hiển thị nội dung đoán mò.
- ✨ Chỉ dùng GPT để tóm tắt khi transcript có thông tin rõ ràng.
- 🔊 Chỉ phát lời nhắc TTS khi đúng người đó quay lại.
- 🧠 Lưu khuôn mặt và lịch sử hội thoại bằng PostgreSQL + pgvector.

## 🧭 Luồng hoạt động

```text
📷 Webcam → 👤 Nhận diện khuôn mặt → 🔎 So khớp pgvector
                                            │
🎙️ Mic Mac → 📝 Transcript tiếng Việt ──────┤
                                            ▼
                                  ✨ GPT tóm tắt
                                            │
                                            ▼
                           💬 Bubble tên + thời gian + ghi nhớ
```

## 🧰 Công nghệ

| Thành phần | Công nghệ |
|---|---|
| 📷 Camera & giao diện | OpenCV + Pillow |
| 👤 Khuôn mặt | `face_recognition` / dlib |
| 🎙️ Giọng nói tiếng Việt | `faster-whisper` + `sounddevice` |
| ✨ Tóm tắt | OpenAI API |
| 🔊 Giọng nhắc | `pyttsx3` |
| 🐘 Dữ liệu | PostgreSQL 16 + pgvector |
| 🐳 Hạ tầng local | Docker Compose |

## 🚀 Chạy nhanh trên macOS

### 1. Chuẩn bị công cụ

Cần có:

- 🍎 macOS
- 🐍 Python 3.11
- 🐳 Docker Desktop đang chạy
- 🔑 OpenAI API key

Nếu máy chưa có thư viện hệ thống:

```bash
brew install python@3.11 cmake portaudio libsndfile
```

### 2. Mở đúng thư mục dự án

```bash
cd /Users/tus/Work/DETECH_APP/An_App_For_Short_Term_Memory_Loss_People-
```

### 3. Tạo môi trường Python

```bash
python3.11 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### 4. Cấu hình ứng dụng

```bash
cp .env.example .env
```

Mở `.env` và thay dòng sau bằng key thật:

```dotenv
OPENAI_API_KEY=sk-your-key-here
```

> 🔐 Không commit hoặc chia sẻ file `.env`.

### 5. Khởi động database

```bash
docker compose up -d
docker compose ps
```

Service `memory-assistant-postgres` cần hiển thị trạng thái `healthy`.

### 6. Cho phép Camera và Microphone

Mở:

`System Settings → Privacy & Security → Camera / Microphone`

Bật quyền cho ứng dụng bạn dùng để chạy dự án, ví dụ `Terminal`, `Visual Studio Code` hoặc `Codex`.

### 7. Chạy ứng dụng 🎉

Lệnh khuyến nghị trên máy Mac:

```bash
.venv/bin/python main.py --webcam-index 0 --fast --auto-name-new-people
```

Khi terminal hiện hai dòng dưới đây là ứng dụng đã sẵn sàng:

```text
Demo đã sẵn sàng.
Đang dùng webcam index 0.
```

## 💻 Chạy trong Visual Studio Code

1. Mở dự án:

   ```bash
   code /Users/tus/Work/DETECH_APP/An_App_For_Short_Term_Memory_Loss_People-
   ```

2. Nhấn `⌘ ⇧ P` → chọn `Python: Select Interpreter` → chọn `.venv/bin/python`.
3. Mở terminal tích hợp bằng `` Ctrl + ` ``.
4. Chạy database và ứng dụng:

   ```bash
   docker compose up -d
   .venv/bin/python main.py --webcam-index 0 --fast --auto-name-new-people
   ```

> 🎥 Nếu VS Code không thấy camera/micro, bật quyền cho **Visual Studio Code** trong `System Settings → Privacy & Security` rồi mở lại VS Code.

## 🎮 Phím điều khiển

| Phím | Tác dụng |
|:---:|---|
| `q` | 👋 Thoát và lưu các phiên đang mở |
| `e` | 💾 Kết thúc phiên hiện tại và tạo summary |
| `m` | ✍️ Thêm ghi chú thủ công |

## 🪄 Các chế độ chạy hữu ích

```bash
# Chạy mượt hơn trên laptop
.venv/bin/python main.py --fast --auto-name-new-people

# Tăng khả năng bắt khuôn mặt
.venv/bin/python main.py --fast --accurate-detect --auto-name-new-people

# Chọn camera khác
.venv/bin/python main.py --webcam-index 1 --fast --auto-name-new-people

# Tắt micro hoặc giọng nhắc
.venv/bin/python main.py --no-audio
.venv/bin/python main.py --no-tts
```

## 🩺 Xử lý lỗi nhanh

<details>
<summary><strong>📷 Không mở được camera</strong></summary>

- Kiểm tra quyền Camera trong macOS.
- Đóng Zoom, Teams hoặc ứng dụng khác đang giữ camera.
- Dùng `--webcam-index 0` để ưu tiên camera tích hợp của Mac.

</details>

<details>
<summary><strong>🎙️ Không nhận được tiếng Việt</strong></summary>

- Kiểm tra quyền Microphone và chọn `MacBook Pro Microphone` trong macOS.
- Nói gần máy, thành câu rõ ràng và tránh mở loa ngoài quá lớn.
- `STT_MIN_CONFIDENCE=0.8` trong `.env` ưu tiên bỏ sót hơn hiển thị câu đoán mò.

</details>

<details>
<summary><strong>🐘 Không kết nối được PostgreSQL</strong></summary>

```bash
docker compose up -d
docker compose ps
docker compose logs postgres
```

</details>

<details>
<summary><strong>✨ GPT không hoạt động</strong></summary>

- Kiểm tra `OPENAI_API_KEY` trong `.env`.
- Không thêm dấu nháy hoặc khoảng trắng quanh key.
- Khi GPT lỗi, transcript vẫn được lưu nhưng summary chưa xác minh sẽ không hiển thị.

</details>

## 🗂️ Cấu trúc dự án

```text
.
├── 🚀 main.py              # Điều phối camera, audio, DB và UI
├── 📷 face_module.py       # Nhận diện khuôn mặt + bubble Unicode
├── 🎙️ audio_module.py      # Whisper STT + TTS
├── 🐘 db_module.py         # PostgreSQL + pgvector
├── ✨ llm_module.py        # Tóm tắt hội thoại bằng GPT
├── 🐳 docker-compose.yml   # PostgreSQL local
├── 🗃️ db/init.sql          # Schema database
├── 🎬 docs/assets          # GIF demo
├── 📦 requirements.txt
└── ⚙️ .env.example
```

## ⚙️ Cấu hình quan trọng

| Biến | Mặc định | Ý nghĩa |
|---|:---:|---|
| `WEBCAM_INDEX` | `0` | Camera ưu tiên |
| `FACE_DISTANCE_THRESHOLD` | `0.62` | Ngưỡng so khớp khuôn mặt L2 |
| `FACE_ABSENCE_TIMEOUT_SECONDS` | `3` | Thời gian chờ trước khi chốt phiên |
| `WHISPER_LANGUAGE` | `vi` | Ngôn ngữ nhận dạng |
| `AUDIO_CHUNK_SECONDS` | `2` | Độ dài một đoạn âm thanh |
| `STT_MIN_CONFIDENCE` | `0.8` | Ngưỡng lọc transcript đoán mò |
| `TTS_ENABLED` | `true` | Bật giọng nhắc khi gặp lại |

## 📝 Ghi chú

- Một micro không thể tách chính xác nhiều người cùng nói; transcript sẽ được gắn cho các khuôn mặt đang xuất hiện.
- Mỗi lần kết thúc phiên tạo một lịch sử mới, không ghi đè cuộc trò chuyện cũ.
- Khoảng cách khuôn mặt dùng Euclid/L2 của pgvector (`<->`).
- Muốn xóa toàn bộ dữ liệu test, chạy `docker compose down -v` — thao tác này không thể khôi phục.

---

<div align="center">

Made with 🧠, 🎙️, ✨ and a slightly forgetful webcam.

</div>
