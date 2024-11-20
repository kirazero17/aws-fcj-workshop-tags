---
title: "Quản lý thẻ bằng AWS CLI"
weight: 2
chapter: false
pre: "<b> 2.2 </b>"
---

Giao diện dòng lệnh AWS ([AWS CLI](https://aws.amazon.com/cli/)) là một công cụ hợp nhất để quản lý các dịch vụ AWS của bạn. Chỉ cần tải xuống và cấu hình một công cụ, bạn có thể kiểm soát nhiều dịch vụ AWS từ dòng lệnh và tự động hóa chúng thông qua các tập lệnh.

Hướng dẫn cấu hình AWS CLI có trong [tài liệu AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html). Trong hội thảo này, bạn sẽ mở phiên bản AWS Cloud9 đã cung cấp để bắt đầu sử dụng AWS CLI.

### Truy cập AWS Cloud9

1. Điều hướng đến bảng điều khiển [CloudFormation](https://console.aws.amazon.com/cloudformation/) .

1. Chọn stack `cloud9-ws` rồi chọn tab Đầu ra.

1. Khóa đầu ra sẽ là `Cloud9DevEnvUrl`.

1. Mở Giá trị URL trong một tab trình duyệt mới, thao tác này sẽ mở môi trường AWS Cloud9 để sử dụng trong các bước sau.

### Lấy ID Amazon Virtual Private Cloud (Amazon VPC)

1. Đi tới bảng điều khiển CloudFormation và chọn stack của khối công việc cho workshop gắn thẻ.

1. Chọn tab **Output** của stack.

1. Sao chép giá trị của **Amazon VPC ID** được liệt kê và lưu thành một tập tin cục bộ.

### Chọn Amazon VPC

Chạy lệnh sau trong Cloud9/VSCode Server Terminal sau khi thực hiện các chỉnh sửa sau:

- a. Nhập giá trị của ID Amazon VPC từ bước trước vào vị trí `"VPC-ID"`

- b. Nhập tên vùng của bạn, chẳng hạn như `eu-west-1`, vào vị trí `"REGION"`

- c. Đảm bảo dấu ngoặc kép (" ") được xóa khỏi các chỗ giữ chỗ này trong lệnh bên dưới

```bash
aws ec2 describe-vpcs --vpc-ids "VPC-ID" --region "REGION" --query 'Vpcs[*][Tags]'
```

Lệnh này liệt kê các thẻ trên Amazon VPC mà bạn đã tạo trước đó. Đầu ra sẽ trông giống như bên dưới:

```json
[
    [
        [
            {
                "Key": "aws:cloudformation:stack-name",
                "Value": "tagging-workload"
            },
            {
                "Key": "aws:cloudformation:logical-id",
                "Value": "tagworkshopvpc"
            },
            {
                "Key": "Name",
                "Value": "tagworkshop"
            },
            {
                "Key": "environment",
                "Value": "dev"
            },
            {
                "Key": "aws:cloudformation:stack-id",
                "Value": "arn:aws:cloudformation:us-east-1:012345678901:stack/tagging-workload/a1b2c3d4-5678-90ab-cdef-EXAMPLE11111"
            }
        ]
    ]
]
```

Lưu ý thẻ đã thêm vào trong bài thực hành trước đó `environment:dev`.

### Quản lý từng thẻ tài nguyên

1. Chạy lệnh sau sau khi thực hiện các chỉnh sửa sau:

    a. Nhập giá trị của **Amazon VPC ID** từ bước trước vào vị trí của `VPC-ID`

    b. Nhập tên vùng của bạn, chẳng hạn như `eu-west-1`, vào vị trí của `REGION`

    c. Đảm bảo rằng dấu ngoặc kép (" ") đã được xóa khỏi các chỗ giữ chỗ này

Lệnh sau đây sẽ thêm một thẻ khác, trong trường hợp này là `owner:workshop`, vào khối công việc đã tạo :

```bash
aws ec2 create-tags --resources vpc-1234567890abcdef0 --tags Key=owner,Value=workshop --region eu-west-1
```

Sau đó kiểm tra xem thẻ đã được thêm đúng chưa:

```bash
aws ec2 describe-vpcs --vpc-ids vpc-1234567890abcdef0 --region us-east-1 --query 'Vpcs[*][Tags]'
```
**Đầu ra:**
```json
[
    [
        [
            {
                "Key": "aws:cloudformation:stack-name",
                "Value": "tagging-workload"
            },
            {
                "Key": "aws:cloudformation:logical-id",
                "Value": "tagworkshopvpc"
            },
            {
                "Key": "Name",
                "Value": "tagworkshop"
            },
            {
                "Key": "owner",
                "Value": "workshop"
            },
            {
                "Key": "environment",
                "Value": "dev"
            },
            {
                "Key": "aws:cloudformation:stack-id",
                "Value": "arn:aws:cloudformation:us-east-1:012345678901:stack/tagging-workload/tagging-workload/a1b2c3d4-5678-90ab-cdef-EXAMPLE11111"
            }
        ]
    ]
]
```

### Kết luận

Bạn có thể tạo thẻ trực tiếp thông qua AWS CLI trên tài nguyên AWS. Điều này cho bạn thấy việc thêm thẻ dễ dàng và đơn giản như thế nào khi sử dụng AWS CLI.