# RAG ChatBot Hỗ Trợ Tuyển Sinh Đại Học

## 📋 Giới thiệu dự án

Dự án này phát triển một **ChatBot Hỗ Trợ Tuyển Sinh Đại Học** sử dụng công nghệ **RAG (Retrieval Augmented Generation)** để tự động trả lời các câu hỏi về thông tin tuyển sinh. Bot có khả năng hiểu và trả lời các câu hỏi liên quan đến:
- Phương thức tuyển sinh
- Học phí và học bổng
- Chương trình đào tạo
- Quy trình thủ tục nhập học
- Cơ sở vật chất và tiện ích

## 🎯 Mục tiêu dự án

- **Tự động hóa** quá trình tư vấn tuyển sinh 24/7
- **Giảm tải** công việc cho nhân viên tuyển sinh
- **Cung cấp thông tin chính xác** từ tài liệu chính thức
- **Ngăn ngừa thông tin sai lệch** (hallucination) nhờ RAG pipeline

## 🔧 Công nghệ sử dụng

### RAG (Retrieval Augmented Generation)
**RAG** là viết tắt của **Retrieval Augmented Generation** (Tạo Tăng Cường Truy Xuất), bao gồm 3 bước chính:
- **Retrieval**: Tìm kiếm thông tin có liên quan từ tài liệu tuyển sinh
- **Augmented**: Kết hợp thông tin đã tìm thấy với câu hỏi của người dùng
- **Generation**: Tạo câu trả lời chính xác dựa trên ngữ cảnh

### Các thư viện và mô hình

#### Xử lý tài liệu
- **PyMuPDF (fitz)**: Đọc và xử lý file PDF
- **spaCy**: Phân tách văn bản thành các đoạn (chunking)
- **sentence-transformers**: Tạo vector embeddings

#### Mô hình AI
- **Embedding Model**: `all-mpnet-base-v2` (768-chiều)
- **LLM**: `ura-hcmut/GemSUra-2B` (Mô hình ngôn ngữ Việt Nam)
- **Transformers**: Hugging Face Transformers library

#### Khác
- **Flask**: API web server
- **NumPy**: Tính toán similarity scores
- **Pandas**: Quản lý dữ liệu

## 🏗️ Kiến trúc hệ thống

```
1. Tài liệu PDF → 2. Chunking → 3. Embedding → 4. Vector Database
                                                        ↓
6. Response ← 5. LLM Generation ← 4. Context Retrieval ← Query
```

### Quy trình hoạt động

1. **Tiền xử lý tài liệu**:
   - Đọc file PDF tuyển sinh
   - Chia văn bản thành các đoạn nhỏ (chunks)
   - Chuyển đổi thành vector embeddings

2. **Xử lý truy vấn**:
   - Nhận câu hỏi từ người dùng
   - Tìm kiếm các đoạn văn bản liên quan
   - Kết hợp ngữ cảnh với câu hỏi

3. **Tạo câu trả lời**:
   - Sử dụng LLM để sinh câu trả lời
   - Đảm bảo thông tin chính xác từ tài liệu gốc

## 📊 Dữ liệu và nguồn thông tin

- **Tài liệu chính**: File PDF "Thông Tin Tuyển Sinh.pdf"
- **Nội dung bao gồm**:
  - Thông tin về Trường Đại học CMC
  - Các ngành đào tạo: CNTT, Khoa học máy tính, AI, Ngôn ngữ
  - Phương thức xét tuyển: CMC401, CMC200, CMC303
  - Học bổng: Khai Phóng, Sáng Tạo, Tiên Phong, Kiến Tạo
  - Cơ sở vật chất và tiện ích

## 🛠️ Cài đặt và chạy dự án

### Yêu cầu hệ thống
- Python 3.8+
- GPU (khuyến nghị cho LLM)
- RAM: Tối thiểu 8GB

### Cài đặt thư viện

```bash
pip install torch torchvision transformers
pip install PyMuPDF sentence-transformers
pip install accelerate bitsandbytes
pip install flask pandas numpy tqdm
pip install spacy
```

### Chạy notebook
1. Mở file `nhóm_14.ipynb` trong Jupyter/Colab
2. Cài đặt môi trường Python
3. Chạy tuần tự các cell để:
   - Tải và xử lý tài liệu PDF
   - Tạo embeddings cho các text chunks
   - Load mô hình LLM
   - Test với các câu hỏi mẫu

### Chạy Flask API
```python
python nhóm_14.py
```

API endpoint: `POST /generate`
```json
{
  "query": "Học phí của trường bao nhiêu một năm?"
}
```

## 💡 Tính năng chính

### 1. Xử lý câu hỏi tự nhiên
- Hiểu câu hỏi bằng tiếng Việt
- Xử lý các dạng câu hỏi khác nhau về tuyển sinh

### 2. Truy xuất thông tin chính xác
- Tìm kiếm thông tin từ tài liệu gốc
- Tính toán độ tương đồng (cosine similarity)
- Trả về top 5 đoạn văn bản liên quan nhất

### 3. Sinh câu trả lời contextual
- Kết hợp ngữ cảnh từ tài liệu
- Tạo câu trả lời tự nhiên và chính xác
- Tránh thông tin sai lệch (hallucination)

### 4. API tích hợp
- RESTful API với Flask
- Dễ dàng tích hợp vào website/app
- Format JSON response

## 📈 Kết quả và hiệu suất

### Mô hình Embedding
- **Model**: all-mpnet-base-v2
- **Dimension**: 768
- **Max tokens**: 384
- **Size**: ~420MB

### Mô hình LLM
- **Model**: ura-hcmut/GemSUra-2B
- **Parameters**: 2 Billion
- **Context window**: 2048-4096 tokens
- **Specialized**: Tiếng Việt

### Accuracy
- Trả lời chính xác cho các câu hỏi về thông tin tuyển sinh
- Giảm thiểu hallucination nhờ RAG architecture
- Response time: < 5 giây trên GPU

## 🎯 Ứng dụng thực tế

### Cho nhà trường
- Tự động hóa tư vấn tuyển sinh
- Giảm tải công việc cho nhân viên
- Cung cấp thông tin nhất quán 24/7

### Cho thí sinh
- Truy cập thông tin nhanh chóng
- Câu trả lời chính xác từ nguồn chính thức
- Hỗ trợ ra quyết định chọn trường

### Cho phụ huynh
- Hiểu rõ thông tin tuyển sinh
- So sánh các phương thức xét tuyển
- Nắm bắt thông tin học phí và học bổng

## 🔄 Phát triển trong tương lai

### Cải tiến kỹ thuật
- [ ] Tích hợp mô hình LLM lớn hơn
- [ ] Cải thiện chunking strategy
- [ ] Thêm multimodal support (hình ảnh)
- [ ] Tối ưu hóa performance

### Mở rộng tính năng
- [ ] Hỗ trợ nhiều ngôn ngữ
- [ ] Chatbot interface với UI
- [ ] Tích hợp voice assistant
- [ ] Analytics và tracking

### Dữ liệu
- [ ] Cập nhật dữ liệu real-time
- [ ] Thêm nhiều nguồn tài liệu
- [ ] Fine-tuning với domain data

## 👥 Đóng góp

Dự án được phát triển bởi **Nhóm 14** như một phần của khóa học về AI và Machine Learning.

### Cách đóng góp
1. Fork repository
2. Tạo feature branch
3. Commit changes
4. Submit pull request

## 📄 License

Dự án này được phát hành dưới MIT License. Xem file LICENSE để biết thêm chi tiết.

## 📞 Liên hệ

Nếu có câu hỏi hoặc góp ý về dự án, vui lòng tạo issue trên GitHub repository.

---

**Lưu ý**: Đây là dự án demo/nghiên cứu. Để sử dụng trong production, cần thêm các bước kiểm thử và tối ưu hóa bảo mật.