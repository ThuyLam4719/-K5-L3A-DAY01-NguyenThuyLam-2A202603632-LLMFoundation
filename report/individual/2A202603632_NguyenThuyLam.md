# Báo cáo cá nhân — Day 01: Lọc tin nhắn rác bằng ML, DL, Embedding và LLM

> Đổi tên file này thành `MSSV_HoTen.md` của bạn, ví dụ `2A20260000_NguyenVanAn.md`, rồi điền từng mục. Xoá các dòng hướng dẫn in nghiêng khi xong.

- **MSSV:** @A202603632 
- **Họ và tên:** Nguyễn Thùy Lam
- **SEED** (in ở ô đầu notebook): 3526

## 1. Bằng chứng

| Tệp | Có? |
| --- | --- |
| [`day01_lab.ipynb`](day01_lab.ipynb) — notebook đã chạy, còn output | ☐ |
| [`lab1_report.json`](lab1_report.json) — do ô "Báo cáo cuối" sinh ra | ☐ |
| [`hand_calc.jpg`](hand_calc.jpg) — ảnh tính tay có các bước | ☐ |

## 2. Kết quả chính

                                 precision  recall  F1 (test)  F1 (SUB 40)  \
1. BoW + LogReg (tự code)            0.988   0.769      0.865        0.889   
2. BoW + MLP (deep learning)         1.000   0.870      0.931        1.000   
3a. Encoder (MiniLM) + kNN           0.837   0.954      0.892        1.000   
3b. Encoder (MiniLM) + LogReg        0.952   0.926      0.939        1.000   
3c. Encoder (MiniLM) + MLP head      0.971   0.935      0.953        1.000   
4a. LLM nhỏ zero-shot                0.870   1.000      0.930        0.930   
4b. LLM lớn zero-shot                0.833   1.000      0.909        0.909   
4c. LLM nhỏ few-shot                 0.909   1.000      0.952        0.952   

                                 train (s)  tin/s (cả lô)  p50 ms/tin  \
1. BoW + LogReg (tự code)            2.940      43006.743       0.045   
2. BoW + MLP (deep learning)         4.680      25377.809       0.285   
3a. Encoder (MiniLM) + kNN          82.842         21.015      57.922   
3b. Encoder (MiniLM) + LogReg       83.259         27.858      24.046   
3c. Encoder (MiniLM) + MLP head     83.869         27.751      24.765   
4a. LLM nhỏ zero-shot                0.000          0.990     402.439   
4b. LLM lớn zero-shot                0.000          1.205     326.723   
4c. LLM nhỏ few-shot                 0.000          0.836     707.946   

                                 p95 ms/tin  USD / 1k tin  
1. BoW + LogReg (tự code)             0.074           0.0  
2. BoW + MLP (deep learning)          0.416           0.0  
3a. Encoder (MiniLM) + kNN          114.168           0.0  
3b. Encoder (MiniLM) + LogReg        36.860           0.0  
3c. Encoder (MiniLM) + MLP head      36.913           0.0  
4a. LLM nhỏ zero-shot              2992.486           0.0  
4b. LLM lớn zero-shot              2428.478           0.0  
4c. LLM nhỏ few-shot               3323.097           0.0  

## 3. Dự đoán và thực tế
### Thí nghiệm 4.a
- Dự đoán F1 của model lớn cao hơn model nhỏ khoản 0.05-.0.1% -> F1 của 2 model bằng nhau (F1 = 0.93)

## 4. Phân tích 50 case khó
*Model nào tụt nhiều nhất, ở nhóm case nào? Nêu 2 case cụ thể và cơ chế khiến model sai.*
 BoW+MLP tuột nhiều nhất. Ở nhóm Spam giọng tự nhiên và Thao túng lừa đảo.

## 5. Thử thách cá nhân

*Đề theo SEED của bạn, cách làm, kết quả, giải thích.*

## 6. Phương án cho sản phẩm thật
- Chọn BoW + MLP: trên kết quả lab, phương án này đạt SLA p95 < 50 ms và có lỗi 1 − F1 thấp nhất trong các phương án đạt SLA. So với BoW + LogReg, nó phân loại tốt hơn nhưng vẫn cần kiểm tra tốc độ trên tải thực tế
- Đánh đổi: cần dữ liệu SMS có nhãn để huấn luyện và cập nhật mô hình; bag-of-words cũng khó hiểu ngữ cảnh mới. Trước khi triển khai cho 10 triệu SMS/ngày, cần xác nhận p95 dưới tải sản xuất và theo dõi chất lượng trên dữ liệu thực tế

## 7. AI log và điều đã học
- Hỏi về cách viết các hàm, call API, kiến thức mới.
- Học được nhiều kiến thức mới: Perato, SLA,...
- Hỏi giá của 2 model grod : "big": "openai/gpt-oss-120b", "mini": "openai/gpt-oss-20b" 
    -> openai/gpt-oss-120b (big): input 0.15$ output 0.6$ (1M tokens)
    -> openai/gpt-oss-20b (mini): input 0.075$ output 0.3$ (1M tokens)
- WER là gì? -> Khái niệm