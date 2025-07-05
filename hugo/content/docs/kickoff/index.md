---
title: "Báo Cáo Khảo Sát Ứng Dụng Máy Học (ML) Trong Sản Xuất"
weight: 1
draft: false
description: "Khảo sát hiện trạng, đánh giá khả thi trước khi đi vào xây dựng giải pháp."
slug: "getting-started"
tags: ["installation", "docs"]
series: ["Documentation"]
series_order: 1
---

{{< alert "fire" >}}
Bước quan trọng để cân nhắc dự án có thể bắt đầu hay không.
{{< /alert >}}

</br>

## Bối cảnh và mục tiêu

Công ty Fujikin (Nhật Bản, có chi nhánh tại Việt Nam) chuyên sản xuất các sản phẩm siêu sạch cho công nghiệp bán dẫn, y tế, hàng không. Hiện Fujikin mong muốn áp dụng ML vào sản xuất với mục tiêu chính: giảm lỗi sản phẩm, hạ chi phí sản xuất, tối ưu hóa tồn kho và nắm bắt xu hướng dịch chuyển nhu cầu khách hàng. Theo xu hướng chung của ngành, AI/ML đã được chứng minh giúp nâng cao năng suất và chất lượng, đồng thời giảm chi phí vận hành. Ví dụ, ML cho phép dự báo thiết bị hư hỏng từ dữ liệu cảm biến, giúp lên lịch bảo trì chính xác hơn. Như vậy, kế hoạch này sẽ định hướng vào các ứng dụng như phát hiện dị thường (phế phẩm), dự báo nhu cầu và tự động hóa phân tích dữ liệu khách hàng để đạt các mục tiêu đề ra.

## Nguồn lực và dữ liệu hiện có

Đội triển khai chỉ có một nhân sự nội bộ (tôi) với nền tảng Python nhưng chưa có kinh nghiệm chuyên sâu về ML. Ngân sách dưới 1 tỷ VND, không thể thuê chuyên gia ngoài. Dữ liệu sẵn có gồm:
1. Dữ liệu IoT từ máy CNC (chỉ bao gồm nhật kí về thời điểm máy dừng/chạy - nguyên nhân. Ngoài ra không có thông tin gì);
2. Dữ liệu ERP Ksystem về đơn hàng, tồn kho, sản lượng sản xuất;
3. Dữ liệu chất lượng QC (kết quả kiểm tra sản phẩm);
4. Email và tệp văn phòng lưu trên SMB chứa thông tin giao dịch và phản hồi khách hàng.

ML có thể tận dụng dữ liệu trạng thái máy CNC để dự báo sự cố hoặc cần bảo trì (ví dụ ML phân tích các mẫu trạng thái để dự đoán thiết bị hỏng).

Tuy nhiên, dữ liệu đang ở nhiều hệ thống khác nhau (sensor logs, cơ sở dữ liệu ERP, file phi cấu trúc) và cần được tổng hợp, làm sạch trước khi phân tích.

## Các thách thức chính

### Chất lượng và tích hợp dữ liệu:

Dữ liệu hiện có đa dạng về dạng thức và chất lượng:
- Dữ liệu IoT CNC có thể thiếu/gian lận,
- Dữ liệu sản xuất ERP do nhân công nhập tay có thể thiếu, sai, không đầy đủ, không theo thời gian thực,
- Dữ liệu lịch sử khách hàng rời rạc,
- Dữ liệu QC chưa gán nhãn đầy đủ.

AI/ML đòi hỏi lượng lớn dữ liệu chất lượng cao; nếu dữ liệu nhiễu hoặc không đầy đủ, kết quả sẽ không tin cậy. Cần xây dựng quy trình thu thập, hợp nhất và làm sạch dữ liệu (ETL) tỉ mỉ.

### Thiếu chuyên gia và nhân lực:

Công ty không có chuyên gia AI, chỉ có một lập trình viên Python. Việc học ML từ đầu và phát triển mô hình mất thời gian và dễ gặp sai sót. Theo khảo sát, một trong những thách thức lớn là thiếu nguồn lực có kinh nghiệm phát triển ML. Cần đầu tư thời gian tự học hoặc tận dụng các giải pháp tự động (AutoML, thư viện mẫu) để bù đắp.

### Hệ thống cũ và tích hợp:

Hệ thống Ksystem, IoT và hạ tầng IT hiện tại có thể không đồng nhất hoặc quá cũ để tích hợp trực tiếp với ứng dụng AI. Vấn đề kỹ thuật tích hợp (legacy system) cần được giải quyết để ML truy cập dữ liệu và phản hồi vào hệ thống điều khiển sản xuất. Cần xác định các giao tiếp API hoặc xuất dữ liệu định kỳ, đồng thời thử nghiệm quy trình tích hợp từng bước.

### Quy mô dự án:
Nên bắt đầu từ các mục tiêu kinh doanh cụ thể phù hợp với kỳ vọng của ban giám đốc:
- Giảm lỗi
- Giảm chi phí

Dự án cần ưu tiên từng ứng dụng cụ thể, không làm quá nhiều cùng lúc. Ví dụ, nên bắt đầu với một use-case đơn giản (chẳng hạn dự báo hỏng hóc máy CNC hoặc dự báo tồn kho một mặt hàng, dự báo điểm sẽ phát sinh lỗi, lập lịch chạy máy CNC) để có kết quả nhanh. Làm quá nhiều mục tiêu đồng thời sẽ khó duy trì, nên áp dụng theo giai đoạn (phased approach) để liên tục học hỏi và cải thiện.

## Giải pháp và công cụ đề xuất

Đề xuất ưu tiên sử dụng các công cụ mã nguồn mở, miễn phí hoặc chi phí thấp, phù hợp với kỹ năng Python hiện tại.

Nền tảng phát triển: Sử dụng Python và môi trường Jupyter Notebook/VSCode để phát triển. Các thư viện phân tích dữ liệu như Pandas, NumPy và thư viện ML như scikit-learn, XGBoost (cho học máy cổ điển) hay TensorFlow/PyTorch (cho học sâu nếu cần) đều miễn phí và phổ biến. Git/GitHub để quản lý mã nguồn. Google Colab hoặc Kaggle notebook có thể cung cấp GPU miễn phí để đào tạo nhanh mô hình.

Xử lý dữ liệu IoT và ETL: Có thể dùng Node-RED (open-source) để thu thập và tiền xử lý dữ liệu IoT từ máy CNC. Sử dụng cơ sở dữ liệu miễn phí như InfluxDB hay PostgreSQL để lưu trữ dữ liệu thời gian thực. Grafana hoặc Apache Superset có thể xây dựng dashboard trực quan cho dữ liệu sản xuất.

Dữ liệu ERP/QC: Dùng Python (SQLAlchemy, Pandas) để kết nối và trích xuất từ cơ sở dữ liệu Ksystem. OCR hoặc NLP (spaCy, Hugging Face Transformers) miễn phí để trích xuất thông tin nếu file văn bản cần phân tích. Apache NiFi hoặc Airflow (open-source) để tự động hóa luồng dữ liệu nếu cần.

Triển khai và phần cứng: Máy tính hiện tại có thể dùng ban đầu; nếu cần tính toán ML phức tạp, một máy tính để bàn với CPU mạnh hoặc card đồ họa tầm trung (Ví dụ: GPU NVIDIA RTX 3060, ~20 triệu VND) có thể mua với ngân sách dưới 1 tỉ. Ngoài ra, Google Colab Pro (chi phí thấp) có thể cung cấp GPU mạnh. Tất cả phần mềm trên đều sử dụng mã nguồn mở/miễn phí, phù hợp với hạn chế chi phí.

## Thời gian dự kiến (MVP)

Giai đoạn học và chuẩn bị (1-2 tháng): Người triển khai tự học cơ bản về ML (qua khóa trực tuyến, sách) đồng thời bắt đầu thu thập, khám phá dữ liệu (EDA).

Giai đoạn phát triển PoC (3-6 tháng): Xây dựng các mô hình đơn giản cho từng mục tiêu nhỏ (ví dụ mô hình dự báo tồn kho hoặc dự đoán lỗi chất lượng) và thử nghiệm trên dữ liệu mẫu. Đánh giá kết quả với chuyên gia nghiệp vụ.

Hoàn thiện MVP (6-12 tháng): Tinh chỉnh mô hình khả thi, tích hợp với hệ thống sản xuất/ERP để có giao diện thử nghiệm. Theo nguyên tắc chung, một MVP đơn giản thường cần khoảng 3–4 tháng, nhưng với dự án ML từ giai đoạn PoC đến triển khai thường kéo dài 10–12 tháng. Vì vậy, mốc thời gian đạt MVP đầu tiên có thể rơi vào khoảng 6–12 tháng tùy độ phức tạp và tiến độ học hỏi.


## Hiệu quả dự kiến

Nâng cao chất lượng và giảm lỗi: ML giúp phát hiện và ngăn ngừa lỗi sản xuất kịp thời. Ví dụ, Toyota sử dụng thị giác máy tính ML để kiểm tra linh kiện, phát hiện lỗi sớm, nhờ đó tăng độ chính xác và giảm chi phí xử lý lỗi. Trong thực tế, Nissan áp dụng AI để phát hiện khuyết tật bề mặt với độ chính xác cao hơn 50% so với kiểm tra thủ công; Tesla thậm chí giảm 90% tỷ lệ lỗi sản phẩm nhờ kiểm tra tự động bằng AI. Nhờ đó, chất lượng sản phẩm được cải thiện rõ rệt và giảm phế phẩm.

Giảm chi phí sản xuất và bảo trì: Dự đoán sự cố và tối ưu bảo trì giúp giảm thời gian chết máy và chi phí bảo dưỡng. Ví dụ, Bosch áp dụng bảo trì dự đoán và đã giảm thiểu thời gian ngừng máy, kéo dài tuổi thọ thiết bị và tối ưu hóa chi phí vận hành. ML còn giúp tối ưu sử dụng nguyên vật liệu. GM giảm 30% lượng phế liệu vật liệu nhờ lập kế hoạch sản xuất tối ưu hóa sử dụng nguyên liệu. Tổng hợp lại, các biện pháp này sẽ cắt giảm chi phí sản xuất đáng kể.

Tối ưu tồn kho và chuỗi cung ứng: Dự báo nhu cầu chính xác nhờ ML giúp giảm tồn kho dư thừa và tránh thiếu hàng. P&G báo cáo giảm 25% tồn kho nhờ dự báo nhu cầu bằng AI. Tương tự, ML cho phép điều chỉnh sản xuất theo biến động thị trường, giảm lãng phí tồn kho. Một hệ thống dự báo tốt sẽ giúp Fujikin có thể ứng phó kịp thời với sự chuyển dịch nhu cầu khách hàng.

Nhận biết xu hướng khách hàng: Phân tích dữ liệu khách (email, báo cáo bán hàng, truyền thông) bằng NLP cho phép hiểu rõ nhu cầu và thị hiếu. ML có thể phân tích cảm xúc và phản hồi của khách hàng để dự báo xu hướng sản phẩm. Kết quả là công ty có thể điều chỉnh chiến lược sản phẩm, nâng cao tính cạnh tranh và sự hài lòng của khách hàng.


Tóm lại, việc áp dụng ML với giải pháp thực tiễn và công cụ mở sẽ giúp Fujikin cải thiện chất lượng sản xuất, giảm chi phí, quản lý tồn kho hiệu quả và nhanh chóng thích nghi với nhu cầu thị trường. Các lợi ích này đã được các công ty hàng đầu trên thế giới chứng minh. Kế hoạch cần tiến hành theo giai đoạn, ưu tiên các mục tiêu dễ đạt được trước, đảm bảo MVP đầu tiên ra mắt trong khoảng 6–12 tháng và đem lại giá trị ngay cho doanh nghiệp.
