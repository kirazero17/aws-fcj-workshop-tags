---
title: "Thẻ (tag) và gắn thẻ - Mở rộng"
weight: 1
chapter: false
---

# Thẻ và gắn thẻ - Mở rộng

### Thẻ (tag) là gì ?

Mỗi **_tag_** là một **_cặp khóa-giá trị_** được áp dụng cho một tài nguyên để **_lưu metadata_** _(siêu dữ liệu)_ về tài nguyên đó. Mỗi thẻ là một nhãn bao gồm một khóa và một giá trị tùy chọn. Không phải tất cả dịch vụ và loại tài nguyên hiện có đều hỗ trợ thẻ - xem [Các dịch vụ hỗ trợ API gắn thẻ nhóm tài nguyên (resource group)](https://docs.aws.amazon.com/resourcegroupstagging/latest/APIReference/supported-services.html). Các dịch vụ khác có thể hỗ trợ thẻ thông qua API riêng của chúng. Cần lưu ý rằng thẻ không được mã hóa và không nên được sử dụng để lưu trữ dữ liệu nhạy cảm, chẳng hạn như thông tin nhận dạng cá nhân (PII).

Các thẻ được người dùng tạo và áp dụng cho các tài nguyên AWS bằng Giao diện dòng lệnh AWS (AWS CLI), API hoặc AWS Management Console được gọi là **_user-defined tags_** _(thẻ do người dùng xác định)_. Một số dịch vụ AWS, chẳng hạn như AWS CloudFormation, AWS Elastic Beanstalk và AWS Auto Scaling, tự động gán thẻ cho các tài nguyên được chúng tạo và quản lý. Các khóa này được gọi là **_AWS generated tags_** _(thẻ do AWS tạo)_ và thường được thêm tiền tố `aws`. Tiền tố này không thể được sử dụng trong các khóa thẻ do người dùng xác định.

Có những yêu cầu sử dụng và giới hạn về số lượng thẻ do người dùng xác định có thể được thêm vào tài nguyên AWS. Để biết thêm thông tin, hãy tham khảo [Giới hạn và yêu cầu khi đặt tên thẻ](https://docs.aws.amazon.com/general/latest/gr/aws_tagging.html#tag-conventions) trong **Hướng dẫn tham khảo chung của AWS**. Các giới hạn và yêu cầu này chỉ áp dụng cho thẻ do người dùng đặt, các thẻ do AWS tạo là ngoại lế.

### Mục tiêu của workshop

Mục tiêu của workshop này là hướng dẫn bạn những cách khác nhau để gắn thẻ tài nguyên AWS và truy vấn các thẻ đó. Sau khi workshop kết thúc, bạn sẽ hiểu cách gắn thẻ và quản lý các thẻ của các tài nguyên AWS khác nhau.

### Kiến trúc hạ tầng workshop
![arch](../images/0-home/0001-architecture.png)

### Cấu trúc nội dung
- Cài đặt môi trường Workshop
- Quản lý tag bằng bảng điều khiển & AWS CLI
    - Quản lý tag bằng bảng điều khiển
    - Quản lý tag bằng AWS CLI
- AWS Resource Groups
    - Tạo resource group dựa trên Stack
    - Trình chỉnh sửa tag
    - Tạo resource group dựa trên tag
- IaC & gắn thẻ
    - AWS CloudFormation
        - AWS CloudFormation Guard
        - AWS CloudFormation Hooks
- CI/CD & gắn thẻ
    - Tạo repo CodeCommit
    - Tạo dự án trên CodeBuild
    - Tạo CodePipeline
    - Xác thực công việc CI/CD pipeline
- Quản lý tag bằng AWS Config
    - Yêu cầu tag bằng AWS Config
    - Truy vấn nâng cao trên AWS Config
    - Dọn dẹp AWS Config
- AWS Organizations
    - Chính sách thẻ của AWS
    - Áp thẻ băng SCP
- Dọn dẹp tài nguyên

### Đọc thêm
1. [Workshop - Quản Lý Tài Nguyên Bằng Tag Và Resource Groups](https://000027.awsstudygroup.com/)
2. [Workshop - Quản Lý Truy Cập Vào Dịch Vụ EC2 Resource Tag Với AWS IAM](https://000028.awsstudygroup.com/)
3. [Tài liệu về gắn thẻ](https://docs.aws.amazon.com/whitepapers/latest/tagging-best-practices/tagging-best-practices.html)