# 📄 PDF RAG Assistant (Vietnamese)

Ứng dụng AI cho phép **hỏi đáp trực tiếp với nội dung file PDF** bằng tiếng Việt, sử dụng mô hình ngôn ngữ lớn (LLM) kết hợp với kỹ thuật **Retrieval-Augmented Generation (RAG)**.

---

## 🚀 Demo

Do hạn chế về môi trường (Streamlit + GPU), ứng dụng được thiết kế chạy trên Google Colab + ngrok.

---

## 🧠 Kiến trúc hệ thống (RAG Pipeline)

Ứng dụng hoạt động theo pipeline sau:

```
User Question
      ↓
Retriever (Chroma Vector DB)
      ↓
Relevant Documents
      ↓
Prompt Template (RAG Prompt)
      ↓
LLM (Vicuna-7B - quantized 4bit)
      ↓
Answer
```

---

## ⚙️ Thành phần chính

### 1. Embedding

* Model: `bkai-foundation-models/vietnamese-bi-encoder`
* Dùng để chuyển văn bản → vector

---

### 2. Chunking

* Sử dụng `SemanticChunker`
* Chia tài liệu theo **ngữ nghĩa thay vì độ dài cố định**
* Giúp tăng chất lượng retrieval

---

### 3. Vector Database

* Sử dụng `Chroma`
* Lưu trữ embedding của các đoạn văn

---

### 4. Retriever

* Truy xuất các đoạn liên quan từ vector DB dựa trên câu hỏi

---

### 5. LLM

* Model: `lmsys/vicuna-7b-v1.5`
* Quantization: 4-bit (bitsandbytes)
* Giảm RAM nhưng vẫn giữ chất lượng

---

### 6. Prompt

* Sử dụng RAG prompt từ LangChain Hub:

```
rlm/rag-prompt
```

---

### 7. Pipeline (LCEL)

```python
rag_chain = (
    {"context": retriever | format_docs, "question": RunnablePassthrough()}
    | prompt
    | llm
    | StrOutputParser()
)
```

👉 Đây là pipeline xử lý:

* Câu hỏi được tách thành 2 nhánh:

  * Nhánh 1: tạo context từ tài liệu
  * Nhánh 2: giữ nguyên câu hỏi
* Sau đó ghép vào prompt → LLM → output

---

## 📦 Cài đặt

```bash
pip install -r requirements.txt
```

---

## ▶️ Cách chạy (Google Colab + ngrok)

### Bước 1: Chạy Streamlit

```python
!streamlit run app.py --server.port 8501 --server.headless true &
```

---

### Bước 2: Tạo public URL bằng ngrok

```python
from pyngrok import ngrok
ngrok.set_auth_token("YOUR_TOKEN")
public_url = ngrok.connect(8501)
print(public_url)
```

---

### Bước 3:

* Mở link ngrok
* Upload file PDF
* Đặt câu hỏi

---

## 📁 Cấu trúc project

```
├── app.py               # Streamlit app
├── source.ipynb        # Notebook giải thích chi tiết từng bước
├── requirements.txt
└── README.md
```

---

## 📚 Notebook giải thích

File `source.ipynb` chứa:

* Giải thích chi tiết từng dòng code
* Cách hoạt động của RAG
* Cách LangChain pipeline hoạt động

---


## 🎯 Mục tiêu dự án

* Hiểu rõ cách hoạt động của RAG
* Xây dựng pipeline end-to-end:

  * Document → Embedding → Retrieval → Generation
* Làm quen với:

  * LangChain LCEL
  * HuggingFace LLM
  * Vector DB

---

## 💡 Điểm nổi bật

* Sử dụng **Semantic Chunking** thay vì chunk cố định
* Hiểu và áp dụng **LangChain Expression Language (LCEL)**
* Triển khai LLM với **quantization (4-bit)**

---

## 📌 Hạn chế

* Chạy local cần GPU mạnh
* Streamlit khó deploy trực tiếp → cần ngrok / cloud

---

## 👤 Author

* Sinh viên Fintech – hướng AI/LLM
* Tập trung vào Data + AI Application

---

## ⭐ Ghi chú

Dự án tập trung vào **hiểu bản chất hệ thống RAG**, không chỉ sử dụng thư viện.

---

👉 Nếu bạn thấy project hữu ích, hãy ⭐ repo!
