---
title: "Quản lý thẻ bằng bảng điều khiển"
weight: 1
chapter: false
pre: "<b> 2.1 </b>"
---

### Lấy ID Amazon Virtual Private Cloud (Amazon VPC)

1. Điều hướng đến bảng điều khiển [CloudFormation](https://console.aws.amazon.com/cloudformation/) và chọn stack **tagging workload**.

1. Chọn tab **Outputs** của stack.

1. Sao chép giá trị của ID Amazon VPC được liệt kê.

### Chọn Amazon VPC

1. Điều hướng đến [bảng điều khiển Amazon VPC](https://console.aws.amazon.com/vpc).

1. Trong ngăn điều hướng, bên dưới Virtual private cloud, hãy chọn Your VPCs.

1. Trong thanh tìm kiếm, nhập giá trị ID Amazon VPC mà bạn đã sao chép và nhấn Enter.

1. Chọn hộp để tô sáng hàng của Amazon VPC.

1. Chọn tab Tags.

![Thẻ VPC](../../../images/2/1/001-vpc_default_tags.png)

Lưu ý rằng Amazon VPC đã có một số thẻ được gán cho nó. Các thẻ đã được thêm vào Amazon VPC khi chúng được tạo bởi dịch vụ CloudFormation. Ngoài bất kỳ thẻ nào bạn xác định, CloudFormation tự động tạo các thẻ cấp stack với tiền tố `aws:`. Tiền tố `aws:` được dành riêng cho mục đích sử dụng AWS. Tiền tố này không phân biệt chữ hoa chữ thường. Nếu bạn sử dụng tiền tố này trong thuộc tính `Key` hoặc `Value`, bạn không thể cập nhật hoặc xóa thẻ. Các thẻ có tiền tố này không được tính vào số lượng thẻ trên mỗi tài nguyên.

Mặc dù `aws:` được sử dụng ở đây, nhưng đây không phải là ví dụ duy nhất về thẻ do AWS tạo. Xem bảng bên dưới để biết danh sách các ví dụ không đầy đủ:

| **Khóa thẻ do AWS tạo** | **Mục đích** |
|----------------------------------|------------------------------------------------------------------|
| aws:ec2spot:fleet-request-id | Xác định yêu cầu Amazon EC2 Spot Fleet nào đã khởi tạo instance |
aws:cloudformation:stack-name | Xác định stack trong AWS CloudFormation đã tạo tài nguyên |
| lambda-console:blueprint | Xác định bản thiết kế được sử dụng làm mẫu cho hàm AWS Lambda |
| elasticbeanstalk:environment-name | Xác định ứng dụng đã tạo tài nguyên |
| aws:servicecatalog:provisionedProductArn | Tên tài nguyên Amazon (ARN) của sản phẩm đã cấp phát |
| aws:servicecatalog:productArn | ARN của sản phẩm mẹ, nơi chạy sản phẩm đã cấp phát |

Một thẻ khác được gán cho tài nguyên là `Name`. Đây là thẻ được định trước có thể được sử dụng để đặt tên cho tài nguyên trong bảng điều khiển để dễ xác định. Trong trường hợp này, bạn đã thêm thẻ này thông qua CloudFormation. Điều này sẽ được đề cập trong bài thực hành sau.s

### Quản lý từng thẻ tài nguyên

1. Chọn tài nguyên và chọn tab **Tag**, sau đó chọn **Manage tags**.

1. Lưu ý rằng các thẻ xuất hiện với tùy chọn chỉnh sửa thẻ **Name** chỉ là các thẻ do người dùng xác định.

1. Chọn **Add new tag**.

1. Nhập cặp `khóa:giá trị` cho thẻ. Trong trường hợp này, hãy nhập `environment:dev`.

1. Chọn Thêm thẻ mới một lần nữa cho mỗi thẻ bổ sung cần thêm. Khi bạn hoàn tất việc thêm thẻ, hãy chọn **Save**.

![Thêm thẻ](../../../images/2/1/002-vpc_new_tag.png)

Bây giờ bạn đã thêm một thẻ mới vào tài nguyên Amazon VPC.

![Đã thêm thẻ](../../../images/2/1/003-vpc_final_look.png)

### Kết luận
Bạn đã có thể thêm một thẻ mới trực tiếp vào tài nguyên AWS bằng bảng điều khiển. Điều này cho bạn thấy việc thêm thẻ dễ dàng và đơn giản như thế nào khi sử dụng bảng điều khiển.