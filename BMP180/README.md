# Module cảm biến áp suất BMP180
## Giới thiệu
Đây là source code driver cho module cảm biến áp suất và nhiệt độ BMP180, giao tiếp thông qua giao thức I2C. Driver này cho phép người dùng mở cảm biến, đọc dữ liệu nhiệt độ, áp suất, và độ cao(so với mặt nước biển) in ra lớp kernel space và có khả năng tương tác với lớp user space.

## Tính năng
Tạo một character device và giao tiếp vơi module BMP180 thông qua giao thức I2C.
Đọc và tính toán giá trị nhiệt độ, áp suất và độ cao theo các hệ số **oversampling_setting** (**OSS**) khác nhau.
In thông tin ra lớp kernel space, bao gồm **id_chip** và số **major_number**
Giao tiếp với lớp user space, cho phép nguời dùng có thể lựa chọn tần số lấy mẫu, chế độ đo (nhiệt độ, áp suất, và độ cao) và in ra màn hình.

## Yêu cầu hệ thống
Raspberry Pi với I2C bus đã được bật và cấu hình trong hệ thống + 1 cảm biến áp suất BMP180.

## Cấu trúc thư mục
BMP180/
├── driver_bmp180.c  # File chính chứa code module kernel
├── test.c           # File chứa ví dụ mẫu giúp người dùng sử dụng các chức năng của module driver
├── Makefile         # Dùng để biên dịch module
└── README.md        # Tài liệu hướng dẫn sử dụng

## Ghi chú
Địa chỉ I2C mặc định của BMP180 là 0x77
Driver sử dụng thông tin hiệu chỉnh (calibration data) từ cảm biến theo hướng dẫn trong datasheet BMP180
Cần đảm bảo thiết bị BMP180 đã được kết nối đúng với bus I2C.

# Tài liệu tham khảo
Datasheet BMP180: https://www.alldatasheet.com/datasheet-pdf/download/1132068/BOSCH/BMP180.html
