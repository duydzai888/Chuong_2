# Sử dụng gói debian
Debian được sử dụng chủ yếu trên các bản phân phối linux dựa trên Debian chẳng hạn như Ubuntu.
### Quy ước về tệp gói Debian
Debian đóng gói các tệp thành 1 tệp gói `.deb` để phân phối.

Tương tự như Centos, các gói có định dạng như sau: `PACKAGE-NAME-VERSION-RELEASE_ARCHITECTURE.deb`
### Bộ lệnh dpkg
Công cụ cốt lõi được sử dụng để xử lí các tệp `.deb` là chương trình dpkg. Đây là tiện ích dòng lệnh có các tùy chọn để cài đặt, cập nhật và xóa các tệp gói `.deb` trên hệ thống Linux của bạn.
 Cài 1 gói deb:
 ```
 [root@laiduy ~]# dpkg -i teamviewr_amd64.deb
```
Hiển thị trạng thái các gói đã cài đặt
```
[root@laiduy ~]# dpkg -s teamviewer
```
Hiển thị các gói đã cài đặt sử dụng `-l`
```
[root@laiduy ~]# dpkg -l
```
Xóa các gói đã cài đặt `-P`
```
[root@laiduy ~]# dpkg -p teamviewer
```
### Sử dụng apt-cache
Tìm các gói đã sử dụng `apt-cache`
```
[root@laiduy ~]# apt-cache search openssh-client
```
### Sử dụng apt-get
`apt-cache` sử dụng để cài đặt, cập nhật và xóa các gói khỏi lưu trữ debian.
```
[root@laiduy ~]# app-get install apache2
```
### Định cấu hình lại các gói
Để hiển thị cấu hình của các gói đã cài đặt:
```
[root@laiduy ~]# debconf-show openssh-server
```