# 🏥 Hệ thống Trích xuất Thông tin Y khoa (Medical NER)

## 📖 Giới thiệu Dự án
Dự án này ứng dụng mô hình ngôn ngữ lớn mã nguồn mở (**Qwen/Qwen2.5-7B-Instruct**) để tự động trích xuất và chuẩn hóa các thực thể y khoa quan trọng từ hồ sơ bệnh án tiếng Việt (văn bản tự do). Hệ thống giúp tự động hóa việc nhận diện các trường thông tin như: Triệu chứng, Chẩn đoán, Thuốc, Xét nghiệm, và hỗ trợ ánh xạ chúng với các mã chuẩn y tế.

## 🚀 Kiến trúc & Môi trường Triển khai
Dự án này được thiết kế và tối ưu hóa để **chạy trên môi trường Đám mây có hỗ trợ Card đồ họa rời (Cloud GPU)** nhằm đảm bảo tốc độ suy luận (Inference) cao nhất.

- **Phần cứng yêu cầu:** Cụm máy chủ GPU (Ví dụ: NVIDIA T4, P100, hoặc A100).
- **Nền tảng kiểm thử:** Google Colab / Kaggle.
- **Thư viện lõi:** `transformers`, `accelerate`, `bitsandbytes`, `rapidfuzz`, `pandas`, `torch`.
- **Cơ chế suy luận:** Sử dụng Pipeline nạp mô hình trực tiếp lên VRAM của GPU, kết hợp cùng các thuật toán Post-processing (Xử lý hậu kỳ) bằng Regex và Fuzzy Matching để gạn lọc dữ liệu.

## 📥 Dữ liệu Đầu vào (Input Example)
Hệ thống tiếp nhận đầu vào là một đoạn văn bản thô, chưa qua cấu trúc, được trích xuất từ hồ sơ bệnh án thực tế.

```text
Bệnh nhân nam 70 tuổi bị bệnh 1 tuần nay, ho đờm xanh, tức ngực, được chẩn đoán mắc bệnh trào ngược dạ dày - thực quản. Bệnh nhân có tiền sử sử dụng Chlorpheniramine 0.4 MG/ML.
```
## 📤 Kết quả Đầu ra (Output Example)
Sau khi xử lý, hệ thống trả về một cấu trúc dữ liệu JSON chuẩn hóa, liệt kê các thực thể y khoa, loại thực thể (Type), vị trí (Position), các khẳng định (Assertions - ví dụ: tiền sử, phủ định) và mã ứng viên (Candidates) nếu có.
```json
[
    {
        "text": "bệnh trào ngược dạ dày - thực quản",
        "type": "CHẨN_ĐOÁN",
        "assertions": [],
        "candidates": [K21],
        "position": [99, 133]
      },
      {
        "text": "ho đờm xanh",
        "type": "TRIỆU_CHỨNG",
        "assertions": [],
        "position": [42, 53]
      },
      {
        "text": "tức ngực",
        "type": "TRIỆU_CHỨNG",
        "assertions": [],
        "position": [55, 63]
      },
      {
        "text": "Chlorpheniramine 0.4 MG/ML",
        "type": "THUỐC",
        "assertions": ["isHistorical"],
        "candidates": [],
        "position": [164, 190]
      }
    ]
  }
]
```
## ⚙️ Hướng dẫn Chạy Dự án (How to run)
1. Tải toàn bộ mã nguồn và mở file .ipynb trên Google Colab hoặc Kaggle.
2. Đảm bảo bật Hardware Accelerator (Trình tăng tốc phần cứng) sang GPU trong phần cài đặt môi trường (Runtime).
3. Cài đặt các thư viện phụ thuộc.
4. Đảm bảo các file cơ sở dữ liệu (ví dụ: icd10.csv, rxnorm.csv) được đặt cùng thư mục gốc với file code.
5. Chạy toàn bộ các Cell trong Notebook để bắt đầu quá trình trích xuất.
