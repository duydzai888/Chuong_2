# Sử dụng RPM
RPM được phát triển tại Red Had cho phép cài đặt, sửa đổi và xóa các gói phần mềm. Nó cũng giảm bớt quá trình cập nhật phần mềm.
### Phân phối và công ước RPM
Một số bản phân phối sử dụng RPM như: bản phân phối dựa trên Red Hat như Fedora và Centos. Ngoài ra có những bản phân phối khác không dựa trên Red Hat, chẳng hạn như openSUSE, Open Mandriva Lx. Các gói rpm có phần mở rộng là .rpm

Cấu trúc gói tin:
`PACKAGE-NAME-VERSION-RELEASE.ARCHITECTURE.rpm`
### Bộ lệnh RPM
Công cụ chính để làm việc với RPM là chương trình rpm.

Cấu trúc: `rpm ACTION [OPTION] PACKAGE-FILE`
#### Cài đặt và nâng cấp các gói RPM
Để sử dụng lệnh `rpm` trước tiên ta cần tải về các gói rpm sau đó sử dụng tùy chọn `-i`hoặc `-U`để cài đặt . Tùy chọn `-U`sẽ phổ biến hơn vì nó sẽ cài đặt gói mới hoặc nâng cấp gói đã được cài đặt.
#### Truy vấn các gói RPM
Sử dụng tùy chọn `-q`để tìm hoặc tra các gói có được cài đặt trên máy hay chưa.
```
[root@laiduy ~]# rpm -q httpd
httpd-2.4.6-97.el7.centos.x86_64
```
thêm tùy chọn `-i`để xem chi tiết các gói
```
[root@laiduy ~]# rpm -qi httpd
Name        : httpd
Version     : 2.4.6
Release     : 97.el7.centos
Architecture: x86_64
Install Date: Sat 10 Jul 2021 10:14:05 PM +07
Group       : System Environment/Daemons
Size        : 9821064
License     : ASL 2.0
Signature   : RSA/SHA256, Wed 18 Nov 2020 09:17:43 PM +07, Key ID 24c6a8a7f4a80eb5
Source RPM  : httpd-2.4.6-97.el7.centos.src.rpm
Build Date  : Mon 16 Nov 2020 11:21:17 PM +07
Build Host  : x86-02.bsys.centos.org
Relocations : (not relocatable)
Packager    : CentOS BuildSystem <http://bugs.centos.org>
Vendor      : CentOS
URL         : http://httpd.apache.org/
Summary     : Apache HTTP Server
Description :
The Apache HTTP Server is a powerful, efficient, and extensible
web server.
```
Xác định các tệp cấu hình của các gói đã cài đặt.
```
[root@laiduy ~]# rpm -qc httpd
/etc/httpd/conf.d/autoindex.conf
/etc/httpd/conf.d/userdir.conf
/etc/httpd/conf.d/welcome.conf
/etc/httpd/conf.modules.d/00-base.conf
/etc/httpd/conf.modules.d/00-dav.conf
/etc/httpd/conf.modules.d/00-lua.conf
/etc/httpd/conf.modules.d/00-mpm.conf
/etc/httpd/conf.modules.d/00-proxy.conf
/etc/httpd/conf.modules.d/00-systemd.conf
/etc/httpd/conf.modules.d/01-cgi.conf
/etc/httpd/conf/httpd.conf
/etc/httpd/conf/magic
/etc/logrotate.d/httpd
/etc/sysconfig/htcacheclean
/etc/sysconfig/httpd
```
### Xóa các gói RPM
Để xóa 1 gói đã cài đặt ta sử dụng tùy chọn `-e`
```
[root@laiduy ~] rpm -e httpd
```
```
[root@laiduy ~] rpm -q httpd
package httpd is not installed
```
### Trích xuất dữ liệu từ RPM
Giải nến tệp RPM mà không cần cài đặt nó. Tiện ích RPM rất hữu ích trong trường hợp này. Nó cho phép tạo ra 1 file mở rộng là `.cpio`để lưu trữ.
### Sử dụng lệnh YUM
`Yum`là 1 dòng lệnh mã nguồn mở cho phép người dùng và quản trị viên hệ thống dễ dàng cài đặt, cập nhật, gỡ bỏ hoặc tìm kiếm các gói phần mềm hệ thống.
#### Cấu trúc
```
yum [tùy chọn] [lệnh] [tên gói]
```
1. Kiểm tra các cập nhật
Kiểm tra các gói nào đã cài đặt đang có sẵn phiên bản cập nhật
```
[root@laiduy ~]# yum check-update
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.vhost.vn
 * extras: mirrors.vhost.vn
 * updates: mirrors.vhost.vn
base                                                                                                   | 3.6 kB  00:00:00
extras                                                                                                 | 2.9 kB  00:00:00
nginx                                                                                                  | 2.9 kB  00:00:00
updates                                                                                                | 2.9 kB  00:00:00
```
Cập nhật tất cả các gói đã cài đặt 
```
[root@laiduy ~]# yum update
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.vhost.vn
 * extras: mirrors.vhost.vn
 * updates: mirrors.vhost.vn
No packages marked for update
```
2. Cài đặt gói
Cài đặt 1 gói hoặc phiên bản cụ thể của gói
```
[root@laiduy ~]# yum install [tên gói]
```
3. Liệt kê gói
Để tìm kiếm 1 gói cụ thể ta dùng `list`
```
[root@laiduy ~]# yum list [tên gói]
```
4. Xem thông tin gói
Để biết thông tin 1 gói trước khi ta cài đặt nó, ta sử dụng `info`
```
[root@laiduy ~]# yum info [tên gói]
```
5. Xóa gói bằng yum
Lệnh này sẽ xóa gói được chỉ định cùng với tất cả các phụ thuộc của nó
```
[root@laiduy ~]# yum remove [tên gói]
```
6. Tải 1 gói mà không cài đặt
Trước tiên ta phải cài đặt gói `plugin"downloadnly"`
```
[root@laiduy ~]# yum install yum-plugin-downloadonly
```
sau đó ta chạy lệnh yum với tùy chọn`--downloadnly`
```
[root@laiduy ~]# yum install --downloadonly --downloaddir=<tên ổ đĩa lưu> <gói cài đặt>
```
sau đó ta vào ổ đĩa và xác nhận tệp đã tồn tại
### Sử dụng ZYpp
Bản phân phối `openSUSE`sử dụng hệ thống quản lý gói RPM và phân phối phần mềm dưới dạng tệp `.rpm`nhưng không sử dụng công cụ `yum`hoặc`dnf`. Thay vào đó nó sử dụng `ZYpp`còn được gọi là `libzypp`.

