# Module cảm biến áp suất BMP180
## Giới thiệu
Đây là source code driver cho module cảm biến áp suất và nhiệt độ BMP180, giao tiếp thông qua giao thức I2C. Driver cho phép:

- Đọc và tính toán dữ liệu nhiệt độ, áp suất, và độ cao so với mực nước biển.

- Giao tiếp với lớp user space và cho phép người dùng cấu hình chế độ đo và tần số lấy mẫu.
## Tính năng
- Tạo character device để giao tiếp với cảm biến BMP180.

- Hỗ trợ đọc dữ liệu với các mức oversampling_setting (OSS) khác nhau.

- In thông tin cảm biến (id_chip, major_number) trong kernel log.

- Giao tiếp với user space để:

  - Lựa chọn tần số lấy mẫu.

  - Chọn chế độ đo: nhiệt độ / áp suất / độ cao.

  - Hiển thị kết quả ra màn hình.

## Yêu cầu hệ thống
- Raspberry Pi với bus I2C đã được bật và cấu hình.

- Cảm biến áp suất BMP180 kết nối với Raspberry Pi.

## Hướng dẫn sử dụng
1. **Kiểm tra cảm biến đã được kết nối và nhận diện trên hệ thống hay chưa**
```js
sudo i2cdetect -y 1
```
- Nếu xuất hiện địa chỉ 0x77, kết nối thành công

- Nếu xuất hiện địa chỉ UU tại vị trí 0x77, ta tiến hành gở lỗi
 ```js
echo 1-0077 | sudo tee /sys/bus/i2c/devices/1-0077/driver/unbind
```
2. **Thêm cấu hình module BMP180 vào Device Tree**
```js
dtc -I dtb -O dts -o bcm2712-rpi-5-b.dts -o bcm2712-rpi-5-b.dtb
dtc -I dts -O dtb -o bcm2712-rpi-5-b.dts bcm2712-rpi-5-b.dtb
```
3. **Cấu hình bmp180 trong Device Tree**
```js
bmp180@77 {
              compatible = "bosch,bmp180";
               reg = <0x77>;
};
```

4. **Biên dịch và cài đặt module**
```js
make
sudo insmod driver_bmp180.ko
```
5. **Kiểm tra log kernel**
```js
dmesg | tail
```
6. **Chạy chương trình mẫu**
```js
gcc test.c -o run
sudo ./run
```
7. **gở cài đặt module**
```js
sudo rmmod driver_bmp180
```

## Ghi chú
Địa chỉ I2C mặc định của BMP180 là 0x77.

Driver sử dụng thông tin hiệu chỉnh (calibration data) từ cảm biến theo hướng dẫn trong datasheet BMP180.

Cần đảm bảo thiết bị BMP180 đã được kết nối đúng với bus I2C.

# Tài liệu tham khảo
[Datasheet BMP180](https://cdn-shop.adafruit.com/datasheets/BST-BMP180-DS000-09.pdf)
