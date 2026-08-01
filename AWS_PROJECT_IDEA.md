# Report\_23\_06\_2026

Date: 23/06/2026  
Main job: Explore AWS Features (Budget, IAM, VPC, EC2, IAM role on EC2, Cloud9, S3, RDS, Lightsail, Lightsail Containers, EC2 auto scaling, Amazon Cloudwatch,...)

User name: Test\_user\_admin\_01  
Pass: Test123\!

OperatorUser; sNF24|\_\_

Kiến thức cơ bản mạng máy tính qua tính năng VPC (Virtual Private Cloud): Subnet, NAT Gateway, Internet Gateway, Route Table, CIDR,

Công việc thực hiện được ngày hôm nay: Kích hoạt tài khoản 200$, thử nghiệm tài khoản con từ root email (1 admin, 1 operator user), đọc được đến phần VPC

# Report\_24\_06\_2026

Date: 24/06/2026

Mục tiêu hôm nay: Học VPC, EC2, tái thử nghiệm tài khoản IAM

AdminUser, wxmB6\]-%  
OperatorUser, \!{wGnX5+

Kết quả: Đã thử nghiệm remote thành công EC2 với các config băng thông từ VPC, IG, NAT đến Elastic IPs để chạy EC2 public. Tài khoản IAM operator thử nghiệm chạy thành công khi change role

# Report\_25\_06\_2026

Date:25/06/2026  
Ghi chú: Nhận thấy các chi phí để chạy bài lab là một điều cần lưu ý khi đã bị hao hụt 0.97$ với các thành phần bị đánh cost chưa nắm rõ. Đã tạo ra thêm 1 budget zero spending để chắc cú nắm bắt chi phí để học lab và đưa ra thêm 1 budget 4$ cho 5 ngày học lab.  
Mục tiêu: Đẩy nhanh tiến độ học lab khi thực hiện đầy đủ toàn bộ các service cần thiết của AWS bao gồm: VPC(đã thực hiện), EC2 (chưa đầy đủ config), watchcloud, database.   
Đánh giá: Không thực hiện lab chăm chỉ trong ngày hôm nay. Chỉ thực hiện làm được vào buổi tối.

Kết quả thực hiện: Hoàn thành thử nghiệm endpoint cho EC2, NAT, Elastic IPs, Kiểm tra đường đi truy cập của Instance, session có quyền truy cập instance không cần ssh, cloudwatch monitoring.

Mục tiêu ngày mai: Hoàn thành nhanh setup đầy đủ EC2, đủ thời gian thì xem qua Amazon S3, RDS và lightsail

Những kiến thức cần tiếp thu đủ trước khi khởi động ý tưởng đồ án, từ khóa:  Amazon S3, RDS và lightsail, DynamoDB, Lamda, CloudFront, ECS, Kiro AI, SQS, SNS, SAM, Cognito, CDK, FSx, WAF, KMS, Macie, Amplify

# Report\_26\_06\_2026

Date: 26/06/2026  
Mục tiêu hôm nay: Set up đầy đủ EC2, Site-to-side VPN

Phân biệt IG, NAT gateway và Route Tables:

| Đặc trưng | Internet Gateway | Network Address Translation Gateway  | Route Tables |
| :---- | :---- | :---- | :---- |
| Bản chất hệ thống | Là cổng chính 2 chiều để dữ liệu địa chỉ IP được dẫn truyền từ Internet bên ngoài vào trong VPC. Không chiếm tài nguyên IP hay tài nguyên tính toán.  | Là một thiết bị mạng có quản lý (Managed Service). Phải chiếm chỗ nằm trong một Public Subnet cụ thể và tiêu tốn chi phí theo giờ.  | Là một danh sách cấu hình logic (Bảng dữ liệu chỉ đường).  |
| Tính chất chiều mạng | 2 chiều hoàn chỉnh. | Cấu hình lại đầu ra (Outbound) và đầu vào (Inbound) tùy theo mong muốn kiểm soát dữ liệu. | Là chỉ dẫn phân luồng trong một subnet và đưa thông tin phân luồng cho NAT gateway. |

Thứ tự kết nối thường thấy IG → NAT → Route Tables → Subnet

Kết quả thực hiện: VPN chỉ đi được 1 chiều từ EC2 Customer sang private chứ không phải chiều ngược lại, nghi vấn là do thư viện chạy trong EC2 dùng để thực hiện. Thư viện Openswam quá cũ, phải thay bằng Libreswan nhưng không hiệu quả. Nên tìm trong YT hay các nguồn khác để tìm cách thực hiện đúng hiện nay và chi phí.

# Report\_29\_06\_2026

Date: 29/06/2026  
Mục tiêu hôm nay: Nhằm đẩy nhanh tiến độ học, thực hiện tiếp đến EC2 để hoàn thành setup EC2 và Set up dữ liệu RDS. VPN sẽ thực hiện sau khi hoàn thành.

Pass EC2 window: ZeuMi38TnXJ6ByD1nRYTqLzLMnK\*)g6; 

Kết quả học hôm nay: Instance được launch từ AMI của microsoft EC2 có vấn đề truy cập khi không thể tạo password để truy cập lần nữa. Đây là vấn đề khá khó hiện tại và nên ghi chú lại để tìm hiểu các giải quyết. EC2 đang học được 40%.

Mục tiêu ngày mai: Xác định mong muốn nghiên cứu và chọn đề tài với nhóm, làm một file trình bày đầy đủ thông tin để mọi người cân nhắc.

# Report\_30\_06\_2026

Date: 30/06/2026

Ghi chú họp nhóm: Họp lần đầu làm quen nhóm, nhóm khá kiệm lời. Kết quả họp hôm nay biết được các thông tin cơ bản như sau:  
Thông tin liên lạc của mn, email, v.v,..  
Ý tưởng đồ án của Khôi và Vỹ (Liên quan tới Terraform, quản lý service AWS)  
Dự định của từng người: Dương \- BA (vì nó non \- tech), Tuấn Anh \- BE, Vỹ \- DepOps, Phong \- DevOps/BE.  
Nên hỏi thêm tiến độ học công nghệ AWS của mọi người thế nào rồi.

Thứ 6 tuần này mở form để chốt lại mong muốn thực hiện đề tài của mọi người.  
Mai (1/7) xác định môi trường làm việc trên các platform cho nhóm (github, gitlab, terraform, aws service \- làm sao để kết hợp 1000$ của mọi người). 

# Report\_01\_07\_2026

Date: 01/07/2026  
Công việc cần thực hiện nhóm xong hôm nay: Set up môi trường kết hợp tài nguyên AWS cho tất cả mọi người trong nhóm; Set up env github, gitlab; Hoàn thành bài tập EC2; Set up Kiro; 

Task Setup organization của nhóm:

- Hoàn thành tutorial để thử nghiệm SCP (service control policies) lên một tài khoản với 1 policy cụ thể chặn terminate instance.  
- Trong tương lai cần phân quyền Organization Unit theo task và IAM account để quản lý resource trong phần AWS resource explorer.  
- Cần nghiên cứu thêm về Organization Configuration để nhóm sử dụng tài nguyên thoải mái và hợp lý cho đồ án.

Task Setup Kiro:

- Phần payment plan Kiro chưa đăng ký billing address ở Việt Nam ⇒ Nên thử các đăng ký khác qua AWS account.  
- Không thể đưa ra phương án lấy free (dù chưa có của region Việt Nam) nhưng đã có thể đăng ký Kiro trên AWS. ⇒ Cần hỏi kĩ hơn về đăng ký free plan pro này.

Task Setup github:

- Tạo organization thành công, thêm vào quyền của mỗi người và repo riêng biệt.  
- Có thể thêm vào github repo riêng của mỗi người để làm worklog riêng phù hợp.

Task bài tập EC2:

- Chưa thực hiện được.

\*Task thực hiện thêm học về cách vẽ kiến trúc AWS:

- Kết quả xem nhanh prompt tóm tắt video:  
    
  Video này hướng dẫn chi tiết cách vẽ sơ đồ kiến trúc \*AWS\* chuyên nghiệp và nhất quán bằng công cụ \*\*draw.io\*\*. Dưới đây là các điểm chính của video:  
    
  \*\*1. Thiết lập ban đầu và công cụ (0:00 \- 4:21)\*\*  
  \* Hướng dẫn tạo sơ đồ mới, chọn thư mục lưu trữ trên \*Google Drive\*.  
  \* Cách truy cập bộ biểu tượng (icon) \*AWS\* trong \*draw.io\*.  
  \* Mẹo tìm kiếm icon thay thế từ bộ công cụ \*PowerPoint\* chính thức của \*AWS\* nếu \*draw.io\* bị thiếu.  
    
  \*\*2. Quy tắc vẽ và định dạng (4:22 \- 14:55)\*\*  
  \* Sử dụng các nhóm (\*group\*) để bao quát \*AWS Cloud\*, \*Region\*, \*VPC\*, \*Availability Zones (AZs)\*, và các \*subnet\* (Public/Private).  
  \* Áp dụng \*\*tỉ lệ vàng (1.618)\*\* để định kích thước khung hình chuyên nghiệp.  
  \* Sử dụng phím tắt \*Command/Ctrl \+ Shift \+ B\* để quản lý các lớp (\*layers\*) khi các phần tử chồng lên nhau.  
    
  \*\*3. Chi tiết xây dựng kiến trúc (14:56 \- 34:34)\*\*  
  \* Thêm các thành phần như người dùng (\*user\*), \*Internet Gateway\*, \*Application Load Balancer (ALB)\*, \*EC2 instances\*, và cơ sở dữ liệu \*RDS\*.  
  \* Quy tắc thiết lập định dạng: đặt kích thước icon chuẩn là \*\*60x60\*\*, viền màu cam (\*\*\#ff8000\*\*) cho dịch vụ chính, và viền màu xanh dương cho các tính năng phụ.  
  \* Sử dụng \*background color\* trắng cho văn bản để tránh bị che bởi các đường kết nối.  
    
  \*\*4. Tối ưu hóa và tái sử dụng (34:35 \- 48:03)\*\*  
  \* \*\*Tạo Library cá nhân:\*\* Hướng dẫn lưu các cấu trúc đã vẽ vào một thư viện riêng trên \*Google Drive\* để tái sử dụng nhanh chóng cho các dự án sau này.  
  \* Quản lý tập trung: Cách export/import sơ đồ dạng file \*.xml\* để chia sẻ trong nhóm, giúp thống nhất phong cách trình bày và tiết kiệm thời gian vẽ.  
    
  Video nhấn mạnh tầm quan trọng của việc duy trì một phong cách đồng nhất trong sơ đồ kiến trúc để khách hàng dễ dàng theo dõi và đánh giá chuyên môn.


Mục tiêu ngày mai: Xác định lại kĩ các khái niệm hạ tầng cloud từ đó xác định và thiết kế artifact version 1 của đồ án “financial application”. Dự tính trước chi phí thực hiện cho cả AI và service sử dụng. Setup best practice organization cho mọi người có thể biết làm việc gì ở đâu.

Xác định người dùng, mục tiêu hiệu quả, quy mô thực hiện, nền tảng bổ trợ.

# Report\_02\_07\_2026

Date: 02/07/2026

Cập nhật hôm nay: Hết hạn gói AWS explorer của cá nhân sớm hơn dự định và hiện tại bị trừ mất 106.09$. Điều này cho thấy kinh phí chung của nhóm trong tương lai chỉ có thể cao lắm là \<500$ và nếu muốn thực sự sử dụng hợp lý phải tính toán thật kĩ.

Cần có người trực đồ án và quy hạn chạy thử nghiệm trong thời gian nhất định để xây dựng và kiểm thử dự án.

Đã có các chỉ mục cơ bản của PRD để trình bày. Điền cơ bản các thông tin proposal cần thiết.

Học thêm về Lambda, Athena, S3, RDB, DynamoDB, 

# Report\_04\_07\_2026

Date: 04/07/2026  
Mục tiêu hôm nay: Học về S3 và Lambda.

Kết quả hôm nay: Hoàn thành bài lab S3, có sử dụng cloudfront và hiểu thêm về các phương pháp tương tác được website truy cập có bảo mật của trang web⇒Nên cân nhắc kỹ lưỡng dung lượng dùng để chạy web sau này (dù có thể sẽ dùng web động) và chi phí thực hiện.

S3 là gì? 

- Là viết tắt của Simple Storage Service, có thể kết nối với các service khác khi bản chất đây là một service để người dùng upload object và sau đó dùng nó để hosting, kết nối với service khác tùy vào nhu cầu sử dụng của người dùng (ví dụ như Cloudfront, Lambda, Athena,...).  
- Một Bucket có thể chứa nhiều Object và xử lý các Object bên trong chúng.

Tổng hợp task và tài liệu để mọi người trong nhóm vừa làm vừa học:

Các kiến thức cơ bản của nền tảng AWS cần biết: 

- Vẽ tài liệu thiết kế cloud: [https://youtu.be/l8isyDe-GwY?si=4i3f4a\_yzHV49BpJ](https://youtu.be/l8isyDe-GwY?si=4i3f4a_yzHV49BpJ).  
- Điện toán đám mây là gì: [https://youtu.be/HxYZAK1coOI?si=ebw8q1duETpwx8jp](https://youtu.be/HxYZAK1coOI?si=ebw8q1duETpwx8jp).  
- Hạ tầng toàn cầu cơ bản của AWS: [https://youtu.be/pjr5a-HYAjI?si=IMDdKh2EmoGCnPRs](https://youtu.be/pjr5a-HYAjI?si=IMDdKh2EmoGCnPRs).  
- Công cụ quản lý AWS: [https://youtu.be/2PQYqH\_HkXw?si=d2Lo19XCkeqi4jLV](https://youtu.be/2PQYqH_HkXw?si=d2Lo19XCkeqi4jLV).  
- Tối ưu hóa chi phí trên AWS: [https://youtu.be/IY61YlmXQe8?si=BQab3AHWW1GtDhj1](https://youtu.be/IY61YlmXQe8?si=BQab3AHWW1GtDhj1).

Các từ khóa cần thiết để nghiên cứu vừa làm vừa học:

- Amazon S3  
- AWS Glue  
- AWS Athena  
- AWS Lambda  
- IAM, VPC, EC2.

Tạo tài khoản Slack và workspace với lời mời: [https://join.slack.com/t/financialcoll-ndw6029/shared\_invite/zt-42z6ehn69-6XRBZDqTNsmIfZiLGmE6OQ](https://join.slack.com/t/financialcoll-ndw6029/shared_invite/zt-42z6ehn69-6XRBZDqTNsmIfZiLGmE6OQ)

Nội dung cuộc họp tiếp theo: Thực hiện PoC mẫu, phân tích yêu cầu PRD (Có tài liệu đầy đủ) và trao đổi để chỉnh sửa mục tiêu, chức năng, v.v… để phát triển đồ án.  
Roadmap thời gian thực hiện (từ PoC, MVP đến Product).  
Task chính của mỗi người theo thế mạnh.  
Phân tích trước các task cần làm để xác định kĩ việc cần làm.  
Đưa ra deadline hợp lý và xác nhận mục tiêu yêu cầu.

# Report\_05\_07\_2026

Date: 05/07/2026  
Mục tiêu hôm nay: Hoàn thành trang artifact tài liệu cho mọi người trong group; tạo repo hugo theme để đăng tải worklog làm việc.  
Kết quả: Hoành thành các công việc đề ra.  
Mục tiêu ngày mai: Họp xây dựng phân chia các task hợp lý. Bắt đầu giai đoạn thực hiện PoC trên github nhằm thử nghiệm trước khi đẩy lên AWS. Organization không bị trừ 100$ mà do còn người trong group chưa lấy 100$ của AWS explorer ⇒ AWS Organization vẫn ổn không bị tính phí thực hiện.

Học thêm lambda và xác định tool chung cho mọi người. Tìm hiểu kỹ về cách tính tiền Kiro pool để áp dụng tool cho mọi người. Phải xác định chi phí đồ án để tính toán trích từ 1000$ của nhóm.  
Chia task hợp lý toàn bộ đồ án cho PoC này.

| Mã task | Task | Người phụ trách | Output cuối tuần |
| ----- | ----- | ----- | ----- |
| W1-01 | Chốt scope MVP tuần 1–7 | Leader \+ cả nhóm | 1 trang `project-scope.md` |
| W1-02 | Tạo GitHub repo và cấu trúc thư mục | DevOps | Repo có `/infra`, `/src`, `/docs`, `/data_sample`, `/notebooks` |
| W1-03 | Viết kiến trúc MVP bản nháp | Leader / SA | Diagram nháp: Source → Collector → S3 Raw → ETL → Curated → Athena/ML |
| W1-04 | Khảo sát nguồn dữ liệu tài chính | Data Engineer | Bảng so sánh nguồn: vnstock, CafeF, Vietstock, FireAnt, HOSE/HNX |
| W1-05 | Chạy thử collector local nhỏ | Data Engineer | Lấy được dữ liệu 5–10 mã cổ phiếu, lưu JSON/CSV local |
| W1-06 | Thiết kế cấu trúc data lake path | Data Engineer \+ DevOps | File `s3-layout.md` với raw/curated/analytics path |
| W1-07 | Tạo mapping chỉ tiêu tài chính tối thiểu | ETL/ML | File `financial_fields_mapping.csv` |
| W1-08 | Xác định feature/label MVP | ETL/ML | File `feature_label_plan.md` |
| W1-09 | Dựng Terraform skeleton | DevOps | Chạy được `terraform init`, `terraform validate`; có provider, variables, outputs |
| W1-10 | Tạo checklist bảo mật/cost cơ bản | Leader \+ DevOps | File `security-cost-checklist.md` |
| W1-11 | Tạo worklog tuần 1 | Documentation/QA | File `week-01-worklog.md` có việc đã làm và kết quả |
| W1-12 | Chuẩn bị backlog tuần 2 | Leader | Board task tuần 2 rõ người phụ trách |

# Report\_06\_07\_2026

Date: 06/07/2026  
Mục tiêu hôm nay: Tổ chức cuộc họp phân chia công việc, phân tích rõ các yêu cầu của đồ án và các công cụ dùng chung cho nhóm.

Kết quả đạt được hôm nay: Đã học xong “serverless with Lambda”.

Các công việc có thể xem xét:

- Các trường dữ liệu cần chú ý: OHLCV, chỉ báo kỹ thuật,...check  
- Công nghệ để lập trình trên github tuần đầu.check  
- Tạo sẵn các tài khoản truy xuất dữ liệu về.check  
- Tool AI chung cho mọi người (Kiro pool).check

# Report\_09\_07\_2026

Date: 09/06/2026  
Thử nghiệm Kiro pool không thành công do phát hiện 1 lỗi trong subscription không rõ nguyên do.  
→ Thử nghiệm từ bên thứ ba hoặc tìm phương thức khác để thực hiện.

# Report\_13\_07\_2026

Date: 13/07/2026

Từ ngày 13 đến 16 tháng 7 không thể học thêm về aws và điều phối nhóm vì phải thực hiện chạy bài báo deadline hội nghị EIDT 2026\.  
→ Việc cần làm khi quay lại: chia task để làm trên github và tổng hợp toàn bộ tài liệu từ bước nghiên cứu khảo sát ứng dụng để thực hiện và đặc biệt là tài liệu cho AWS.   
→ Gọi tên bài toán đúng cho việc thiết kế hạ tầng AWS.

Tài liệu để prompt AI, tài liệu high level approach.

# Report\_20\_07\_2026

Date: 20/07/2026  
Công việc hôm nay: setup portal cho organization của nhóm với các OU thử nghiệm và roles mới theo kiến trúc hiện tại. Hoàn thành cho tài khoản leader admin và workload view only.  
Có thể Kiro setting bị lỗi do group lần trước setup chưa có thành viên trong đó nên gây lỗi→Thử nghiệm lại sau khi tổ chức đã thành hình.

Xem lại kiến trúc datalake chuẩn với các giao diện chuẩn: [How to Build a Data Lake-A Complete Guide | ProjectPro](https://www.projectpro.io/article/how-to-build-a-data-lake/1071) 

# Report\_21\_07\_2026

Date: 21/07/2026

Mục tiêu hôm nay: setup xong các tài khoản chính quản lý tài nguyên chính, sau đó thử nghiệm lại với setting Kiro.

Đã gửi support center để giải quyết vấn đề lỗi Kiro Subscription. Tài khoản không trong diện phù hợp để tạo subscription cho organization.

→ Bỏ hướng tiếp cận Kiro làm tool. Suy xét pipeline tốt hơn để sử dụng.

Phân biệt rõ tính chất của dự án hiện tại: Database, Data warehouse, Data lake.

- Video học thêm: https://youtu.be/-bSkREem8dM?si=R5y\_AqhNa39WvKPK

| Database | Data warehouse | Data lake |
| :---- | :---- | :---- |
| Sử dụng nguyên lý OLTP (Online Transactional Processing), túc là xử lý giao dịch trực tuyến. Mang tính chất liên tục và theo thời gian thực. Ghi chép lại chi tiết thông tin cần thu thập và tối ưu truy vấn, tối ưu lưu trữ. Phù hợp để thực hiện các tác vụ lưu trữ giao dịch trong thời gian thực và cần sử dụng lâu dài.  Có tính chất nặng về lưu trữ và sử dụng càng lâu dài càng có ảnh hưởng về tốc độ truy vấn. | Sử dụng nguyên lý OLAP (Online Analytical Processing), tức là xử lý phân tích trực tuyến. Mang tính chất tóm gọn dữ liệu và lưu trữ để phân tích trong một khoản thời gian trong quá khứ, có tính tạm thời, không trực tiếp liên tục cập nhật. Một data warehouse có thể nhận trích xuất từ các nguồn DB khác nhau với phương pháp ETL (Extract, Transform, Load). Rigid schema Có tính chất lưu trữ gọn hơn database nên gần như không bị ảnh hưởng truy vấn.  | Sử dụng để lưu trữ toàn bộ các loại dữ liệu thô (từ dữ liệu đã cấu trúc, bán cấu trúc đến hoàn toàn không có quy luật cấu trúc). Lưu trữ trong thời gian thực và mục tiêu là lưu trữ nhiều dữ liệu. Dữ liệu chưa được xử lý chiếm chủ yếu. Phù hợp để sử dụng cho các use case huấn luyện ML hay AI. |

Kết quả hôm nay: Đã hoàn thành setup ổn cho IAM của organization của nhóm nhằm chia sẻ chung tài nguyên.  
Mục tiêu ngày mai: Vẽ lại thiết kế AWS theo các tài liệu về data lake mới và lĩnh vực tài chính chuẩn. Tham khảo của Phong.

# Report\_22\_07\_2026

Date: 22/07/2026  
Mục tiêu hôm nay:

- Phân tích kiến trúc framework data lake mẫu của aws để xem cách trình bày chuẩn của một hệ thống data lake và vẽ lại kiến trúc một các hoàn thiện. Tham khảo: https://github.com/aws-solutions-library-samples/data-lakes-on-aws.git  
- Ghép lại các code hệ thống hiện có, cần chốt lại các điều kiện biến môi trường và thư viện. 

Kết quả hôm nay:

- Ghép code ở mức độ chắp vá (AI craft), chưa viết test, nhưng có xuất hiện các bug hiển thị thông tin cổ phiếu cụ thể, cần cân nhắc chỉnh sửa lại giao diện theo mức module (chia ra các .py theo tab, graph,...). Quy mô dữ liệu vừa đủ cho PoC mục tiêu ban đầu (100 cổ phiếu).   
- Đã thống nhất được môi trường thực hiện qua uv.lock, gitignore và các requirement liên quan.   
- Chưa phân tích và vẽ được thiết kế mẫu học được từ repo data lake framework vì tính phức tạp trong phân tích (quá nhiều context được sinh ra, cần nghiên cứu kỹ hơn).  
- Phần demo có thể chấp nhận ở mức 50%, nghĩa là nó có chạy nhưng chưa chạy đúng mục tiêu ban đầu yêu cầu. Cần thiết kế lại các đầu việc chuẩn để thực hiện chính xác hơn.  
- Kết luận: Các provider chưa đủ mở rộng, cần nghiên cứu các nhà cung cấp khác nhiều hơn và uy tín hơn, có nguy cơ sẽ mất tiền sử dụng dịch vụ nhưng cần cân nhắc áp dụng cho dịch vụ. Tìm phương án phù hợp để mở rộng nguồn DB tìm được là nhiều nhất có thể và đa dạng dữ liệu nhất có thể. 

  → Tham khảo: [DataFeed \- Dữ liệu tài chính tích hợp chuyên nghiệp | Vietstock](https://dichvu.vietstock.vn/du-lieu-tai-chinh/datafeed---du-lieu-tai-chinh-tich-hop-chuyen-nghiep); [https://youtu.be/CxQJUbdoxt4?si=esU0\_gkQo9Tw4RE3](https://youtu.be/CxQJUbdoxt4?si=esU0_gkQo9Tw4RE3); [Chứng khoán DNSE \- Miễn phí giao dịch trọn đời](https://www.dnse.com.vn/); [Fast Connect API](https://www.ssi.com.vn/khach-hang-ca-nhan/fast-connect-api); [Giải pháp Dữ liệu Chứng khoán​ (API Datafeed, Microsite)](https://fiingroup.vn/vi/giai-phap-du-lieu-chung-khoan-api-datafeed-microsite.html).

# Report\_23\_07\_2026

Date: 23/07/2026  
Mục tiêu hôm nay: Sửa lại bản PoC gần nhất, kết hợp viết test (Số lượng token đang hạn chế). Đặt ra các task mới phù hợp với việc trình bày dự án cho nhóm các anh mentor. 

Prompt thiết kế rà lỗi để chỉnh sửa và chỉ ra các vấn đề trong thiết kế hệ thống kiến trúc PoC hiện tại:  
Bạn là người có góc nhìn của một QA nhiều kinh nghiệm trong phân vùng lĩnh vực về data lake và thiết kế hệ thống của một SA.   
Hiện tại repo này đang trong giai đoạn vừa được ghép các phần code logic từ FE và BE nguồn Data source. Dữ liệu được lưu local với DuckDB.   
Tuy nhiên hệ thống này chưa có các phương pháp phân vùng và kiểm thử hệ thống. Đồng thời cách thức lấy dữ liệu data source và quy mô lấy vẫn còn hạn chế. Nên được thay đổi từ việc lấy bằng thư viện chuyển sang yêu cầu nhận API key để có thêm request/phút hơn (Đơn cử với Vnstock sử dụng API key).  
Về kiểm thử, nên được phân vùng ra thành các module kể cả trong từng thành phần như BE hay FE. Với FE, nên có các bài kiểm thử theo phương pháp luồng xử lý, chức năng của từng use case. Với BE, nên cũng tương tự và chia theo các module thực hiện dự án.  
Với các dữ kiện trên tôi cần bạn thiết kế ra bộ kiểm thử để rà lỗi theo các requirement về độ trễ của hệ thống.  
Bộ test nên được làm đúng theo quy chuẩn yêu cầu của hệ thống với các tài liệu tôi cung cấp cho bạn.  
Phân chia theo các bộ unit test có mã đầy đủ và xuất ra file thống kê cũng như cách sử dụng sau khi bạn đã hoàn thành xong để tôi bắt đầu chạy test CI trên github action.  
Chỉ đưa ra và thực hiện chỉnh sửa với bài test khi bạn tự tin rằng nó đúng với yêu cầu gốc của dự án.

Kết quả hôm nay: Đã tạm thời viết test cho pipeline chính của hệ thống từ data src đến hệ thống nhưng vẫn còn vướng khá nhiều lỗi FE và chưa kiểm toàn diện BE.

# Report\_24\_07\_2026

Date: 24/07/2026

Mục tiêu hôm nay: Review giao diện mới theo chỉnh sửa của Vỹ, phát hiện lỗi và chia việc để chốt kiến trúc aws gửi cho các anh mentor trong tuần này.

Kết quả: Đã chia việc theo từng vai trò, review code chỉnh sửa của Vỹ và báo cáo có thêm các phần cần phải lưu ý trong BE, đặc biệt là lưu ý các lỗi data injection, searching injection. Hoàn thành 80% sơ đồ AWS mẫu. Cấn chú ý thêm các yếu tố sau cho thiết kế sau này:

- Mở rộng quy mô data source theo các nguồn API và kiểu dữ liệu nhập về.  
- Chuyển đổi thành hệ thống Data lakehouse nhằm phù hợp với framework xem xét.  
- Điều chỉnh lại tài liệu AWS theo các heading cần thiết để nhóm cùng thực hiện ra file cuối cùng cho nhóm.  
- Deadline nộp aws whatsapp thứ 7 tuần này (25/07).

Mục tiêu ngày mai: Hoàn thành tài liệu hệ thống AWS và chuyển sang nghiên cứu khoa học bản journal (lấy thêm resource fuzzy logic, kết hợp 3 bài báo chính và viết report).

# Report\_25\_07\_2026

Date: 25/07/2026

Mục tiêu hôm nay: Xem buổi workshop của công ty onl. Tổng hợp tài liệu thiết kế AWS và tham chiếu từ những trình bày có của các anh mentor. 

Kết quả đạt được: Đã xin được trình bày dự án hiện tại và networking được với các anh (anh Khiêm và anh Lực) để có thể học hỏi và trình bày trong tuần sau. Chưa tổng hợp được tài liệu như đã có bản 0.1 và tổng hợp hình vẽ. Cần chỉnh sửa hình vẽ thêm các số thứ tự luồng thực hiện và đánh giá. 

Bắt đầu tổng hợp tài liệu viết journal.

# Report\_27\_07\_2026

Date: 27/07/2026

Mục tiêu hôm nay: Pitching đề tài của nhóm cho các anh mentor để thử sai. 

Các service cần tìm hiểu: 15 service chính có liên quan hiện tại, tiny fish (đã thực hiện crawl fetch và search)

Amplify  
S3  
Glue crawler,  
Glue data catalog  
Athena  
Lambda worker

Kết quả: hoàn thành 1 nửa Amplify, chuẩn bị cài đặt 

# Report\_28\_07\_2026

Date: 28/07/2026

Mục tiêu hôm nay: Chia các service để xem lại các bản chất của các dịch vụ sẽ dùng và tận dụng đúng chức năng của các service, tối ưu chi phí.

Amplify  
S3  
Glue crawler,  
Glue data catalog  
Athena  
Lambda worker

Kết quả: Đã hoàn thành chạy thử nghiệm bài lab cho Amplify, được anh Minh giới thiệu chỉ dẫn thêm sử dụng Langfuse để kiểm tra dataset.  
Sơ đồ đã có tham khảo thiết kế cũng của workshop của anh Minh và có thể điều chỉnh để có thể phù hợp với pipeline lấy dữ liệu khép kín của hệ thống production. Cần thiết kế phân quyền các tài khoản IAM nhằm phân bổ quyền hợp lý không dư thừa để có thể cho nhóm có các công việc khác nhau trong hệ thống. Cần chia việc hợp lý cho các người còn lại trong team. Hiện tại: Vỹ \- FE, Phong \- BE, Khôi \- CI/CD. Nên chia thêm các công việc xử lý data cho Dương và giám sát hệ thống monitoring cho Tuấn Anh (cần xem xét). 

# Report\_29\_07\_2026

Date: 29/07/2026

Mục tiêu hôm nay: Hosting hệ thống qua Amplify và cài đặt WAF phân quyền đầy đủ, cùng với đó thiết kế phân quyền IAM cho các công việc qua các policy hợp lý không dư thừa.

Về chức năng ML nhằm huấn luyện dự đoán Agent, cần hỏi thêm hướng dẫn từ anh Quinler, nhằm đối chiếu lên hệ thống của AWS hiệu quả và cost optimization.

Hệ thống hosting và substance dự án bằng  

Lưu ý 6 pillars quan trọng để đảm bảo thiết kế hệ thống tối ưu:**Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization và Sustainability**.   
	

Chạy thử nghiệm ước lượng chi phí hệ thống trong cost caculation.

Việc tiếp theo: thiết kế IAM để tiếp hành các công việc sau theo công việc cần thiết:  
Đẩy hệ thống hiện tại lên IAM

# Report\_30\_07\_2026

Date: 30/07/2026  
Mục tiêu: Học hiểu được về Athena, hiểu về Event Bridge để ứng dụng vào hệ thống kéo dữ liệu theo 2 luồng chính.

- 2 luồng chính khi người dùng dùng hệ thống: 1 là luồng tự cập nhật mỗi ngày bằng event bridge sau đó lập thành một bộ report lên dashboard người dùng; 2 là luồng cập nhật người dùng gửi request cho hệ thống để kéo athena cho người dùng lấy về report \+ bộ tập dữ liệu của các doanh nghiệp và thời gian yêu cầu.

Xem qua hand on của event bridge: https://youtu.be/77zSXuFs4GA?si=Us\_XR5-ogDYW77hj

# Report\_31\_07\_2026

Date: 31/07/2026

Mục tiêu hôm nay: Gửi kiến trúc hiện tại lên group SGU và nhận review đánh giá, chỉnh sửa lại cho phù hợp. Sau đó tranh thủ thời gian chuyển sang làm nghiên cứu khoa học. Đầu tiên làm nghiên cứu xem FinRL đã xử lý chỉ báo kĩ thuật trước khi huấn luyện như thế nào? Sau đó xem lại qua các bài báo RDX để xem mức độ chuyển đổi cách thực hiện thuật toán sẽ được diễn ra ở mức độ nào.

Kết quả hôm nay: Đã gửi trình bày lên các anh, trước mắt cần chỉnh sửa chia vùng để trình bày dễ nhìn hơn. Nghiên cứu journal đã có tiến triển hơn ở phương pháp XAI mới. 

Câu hỏi nghiên cứu tiếp theo: Cần phát triển thêm thuật toán và các cách giải thích khác phát triển thêm hay chỉ cần kết hợp các nghiên cứu cũ nhưng chạy lại với phương pháp nghiên cứu mới?

# Report\_01\_08\_2026

Date: 01/08/2026

Mục tiêu hôm nay: Hoàn thành report báo cáo nghiên cứu của journal sau khi viết được tổng hợp các nghiên cứu cần có với cái câu hỏi xung quanh. Có thể hoàn thành: phương pháp thực hiện mới nhất. Với các đầu việc như sau:

- Tổng hợp bài báo, lấy đủ tư liệu cũ đã thực hiện phương pháp như thế nào.  
- Vẽ hình thành phương pháp chính bao gồm những gì, luồng làm gì và chi tiết ra sao theo hình phương pháp cũ.  
- Nếu phương pháp có chỉnh sửa gì khác thì phải điều chỉnh.  
- Thay đổi với phương pháp cũ ở những điểm nào.  
- 

Thử nghiệm các service chính của pipeline của kiến trúc AWS.  
Vẽ lại architecture theo layer. → Đã nhờ TA và Dương. Cần gửi lại cho group trong thời gian có thể.

Gặp phải khó khăn mới: Sang tháng mới toàn bộ các credit của nhóm bị reset và mất đi về 0, cần có phương án tạo các tài khoản mới đáp ứng cho tháng này.