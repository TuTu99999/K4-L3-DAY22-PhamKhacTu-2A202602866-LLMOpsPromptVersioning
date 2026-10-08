# Day 22 — Phạm Khắc Tú

## Kết quả RAGAS V1/V2

Đã chạy cùng 50 QA pairs qua mỗi prompt, với FAISS, chunk size 500 ký tự,
overlap 50 và top-k=3. Hai system prompt trong bước 3 khớp nguyên văn bước 2.
LLM và evaluator dùng `cx/gpt-5.6-luna` qua 9Router; embeddings chạy CPU bằng
`sentence-transformers/all-MiniLM-L6-v2`, vector 384 chiều được chuẩn hóa.

| Metric | V1 | V2 |
| --- | ---: | ---: |
| faithfulness | 0.9525 | 0.9396 |
| answer_relevancy | 0.7815 | 0.7883 |
| context_recall | 0.9800 | 0.9800 |
| context_precision | 0.9133 | 0.9183 |

Cả hai phiên bản đạt faithfulness ≥0.9. V1 cao hơn khoảng 0.0129 về
faithfulness; một cách giải thích hợp lý là chỉ dẫn trả lời ngắn gọn giúp giảm
số claims có thể vượt ngoài context. V2 cao hơn khoảng 0.0068 về relevancy,
có thể do yêu cầu phân tích các facts liên quan và trình bày đủ ý. Đây là diễn
giải từ một lượt đánh giá, không phải bằng chứng nhân quả.

Retriever được giữ nguyên nên recall bằng nhau. Chênh lệch precision nhỏ có
thể đến từ biến động phán xét của LLM evaluator, không có nghĩa V2 dùng một
retriever khác. Router chỉ trả một generation khi metric relevancy yêu cầu ba;
RAGAS tiếp tục với một generation và cảnh báo này được giữ trong log.

Báo cáo bắt buộc: `03_ragas_report.json`. Log thật: `03_ragas_evaluation_log.txt`.
Output đủ 100 QA và điểm từng sample được lưu trong `data/ragas_outputs.json`,
`data/ragas_v1_samples.json` và `data/ragas_v2_samples.json` để kiểm chứng.

Ảnh `03_ragas_scores.png` phải là ảnh chụp terminal của bảng kết quả thật.
