#### Hướng dẫn cách tạo Keypair

# 1. TOPO LAB
- Giả lập trên VMware Workstation.
- Centos 7 Minimal-2009 Server 64 bit.
- Cần có 2 máy: Client và Server.
# 2. MÔ HÌNH
![mô hình](https://user-images.githubusercontent.com/84270045/141089490-c6b1aa57-6870-4b32-a008-d8f2ac734422.png)
# 3. IP PLANNING
![ip planning](https://user-images.githubusercontent.com/84270045/140905424-d7437509-5d27-4954-a083-0a15e4d21993.png)
# 4. Các bước thực hiện
### 4.1. Giới thiệu về SSH Keys.
SSH cung cấp cho chúng ta 2 phương thức để xác thực đó là:

1. Xác thực bằng User/Password
1. Xác thực bằng SSH Keys

Việc xác thực SSH sử dụng User/Password tiềm ẩn khá nhiều nguy cơ bảo mật. Vì vậy để server bảo mật thì phương pháp xác thực bằng `SSH Keys` được khuyến nghị sử dụng.

`SSH Keys` có 2 loại: 
- SSH Keys Public: được tạo ra và lưu ở trên server, để khi xác thực chúng sẽ thực hiện trao đổi với nhau.
- SSH Keys Private: do các chương trình client sử dụng.

Khi kết nối được yêu cầu thì chương trình SSH Client sẽ gửi Private Keys lên Sever và Sever sẽ xác nhận sự phù hợp giữa 2 keys này, nếu có sự phù hợp thì sẽ cho kết nối.
### 4.2. Tạo SSH Keys trên Client.
- Để xác thực được bằng `Keys pair`trước tiên ta phải tạo ra cặp keys đó với lệnh: `ssh-keygen -t rsa`
```
[root@client ~]# ssh-keygen -t rsa

```
- Tham số `rsa`tức là cặp khóa này được tạo ra bằng thuật toán RSA.
Sau khi nhập xong dòng lệnh trên và ấn Enter thì dòng đầu tiên họ sẽ hỏi ta muốn lưu cặp Keys này ở đâu. Tại đây nếu muốn lưu mặc định thì chỉ cần ấn Enter 
```
[root@client ~]# ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa): 
```
- Sau đó thì họ sẽ yêu cầu điền `passphrase` nhằm bảo mật cặp Keys pair để an toàn hơn vì dù keys bị người khác biết Keys thì khi đăng nhập vào máy sẽ hỏi `passphrase`
```
[root@client ~]# ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase):
```
- Sau khi hoàn thành xong thì giao diện sẽ có dạng: 
```
[root@client ~]# ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:SV2gF0o1FIIaXmGWF1BA/q4fdCf+7c+dcXL058bnSKc root@laiduy
The key's randomart image is:
+---[RSA 2048]----+
|     .BB=+Bo.    |
|    .++..= +     |
|   . +..+ o      |
|    o  o o       |
|        S o .   .|
|       o o o   ..|
|        o .   oo*|
|       . . . o OX|
|      ...   ..E=B|
+----[SHA256]-----+
```
### Kiểm tra lại xem Keys đã được tạo thành công hay chưa. 
- Để kiểm tra Keys đã được tạo thành công trên `Client` hay chưa bằng lệnh `ls -l /root/.ssh`. Kết quả hiện ra 2 file là `id_rsa` và `id_rsa.pub` như dưới tức là đã thành công.
```
[root@client ~]# ls -l /root/.ssh
total 12
-rw-------. 1 root root 1766 Sep 20 04:54 id_rsa
-rw-r--r--. 1 root root  393 Sep 20 04:54 id_rsa.pub
-rw-r--r--. 1 root root  183 Sep 19 20:47 known_hosts
```
- Tại đây ta có thể thấy 2 file là `id_rsa` và `id_rsa.pub`. 
    - File `id_rsa` là file Private Key và client sẽ giữ file này.

    - File `id_rsa.pub` là file Public Key và sever sẽ giữ file này
 Trong cấu hình SSH thì `Public key`được lưu trong file là `authorized_keys` nên ta sẽ tiến hành đổi tên file:
 ```
[root@client ~]# mv /root/.ssh/id_rsa.pub /root/.ssh/authorized_keys
```
- Tiếp theo ta sẽ lấy file `id_rsa` về, dùng lệnh `cat /root/.ssh/id_rsa`:
```
[root@client ~]# cat /root/.ssh/id_rsa
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-128-CBC,BF2BA2B5B4B29FCA52F3FA3411AC7359

WN39Q/MKBtgfuoaaIoo6chW+1cG+Pk1BNh+RImSDr+87MHdv9jBPWOYdi57HshOr
EX3f0Lx+jqorq/bhFZHwHMUdU2tVk3FSUQiFer9RsQf0C6mjSLfmlAO++vZdPGSk
QP8fjAsRmrK5IynK5Nb6HVjpmlhItoi034gIZkmNsBnhMCwvvp5IYJuZkN6i9cqX
1pvIkLuj3vDxC4Y04HffkNBeFjMWk/Mo68wwAPaG8ySNYrc5kHHaRxuv2xC5wgK0
ZUta6ooTkjUmN6hVDE5emTJssmpxpTLSHgsAtZYiPEb+B+gkx1L/YYeVDRsb1qfA
VRO9Y1+cR12P8skg0C4KOhleixlagbdvvrhvlirOuS2rZOcil6lQm2A97om9Bttc
DxeDh06MEuXEPozwcO5abR7kybPAj2NeJBfltwUr2deYYBaKZvelJPqAMus9NLMy
8f8Cw+kKBoDEftKGCYmJ5r3p1WfP6q76JzrQJUfBwVo8luykSda5TZK7vmaLBdO2
QERdAaQkHI5kkeeXfNAmuO6G5LNY8Vvctqy9E5h/+vr/E5bYTGwRK6EU/+aFFspg
tZtN7L7hOJu9algfx8dwmGhm6JDsO4Gxd+xthloIdgXIQGP0FUZS38uaNN/dlgfz
MrNnovTSgirB5ZFiJ/iBLYixIOgTfpRp05aEaSZmjfHVvDPhszhSTcNb4isuk9+W
ViJAcESRRES6xqqZok3o4OT81Q/y7AFef0UnQBSFIeqnHUIsLtf6OV8IxJehWlSo
DWO+KMDsHiV6jzAsTFnpk6qjQKlFrzd6AvCdyH1iP2gj3C0Ka0pewKetZdd65jWP
hKYD8zQbt/esYp+zhgGIO5LaFvX9mvjzA/bTJ2rZwdZcP9h8qvvIdDgx7oYIn5yn
r00O+0eZeOn5wZaq5QvDN8pyoHv96d0LOhI8LAJnLCbtSWeACupr2bK6Bv05H5tt
qpBz3HrHInxUAt8GEHLfjOjptTRSzAeIL5DMJq+vuSeavxsgtftW1ZCiVwS4oIjB
tkeeIg8Fhh/l+71/ZriSCCQNDrql1ZYF9ad447JgNfn2xJRkHX3C1FyyDn+9E5Rx
hbWvdX7jYwCdF72lXdJhgNChkCpJRi+iHJly6vYGASVnwLkGGfr6aXPxGpZrtv/Z
lAwYNjl28p52wFsZDik6ZbW6qF3/eXQ7tMjY5trlVcrHipgTZGx1kV6JaJOQDpI0
kHZR+SWzQlc0ew2axutlqIZ2yVlNB+ksE3GjMSG/Whti5T21DHkhF/fTKhkIsdiK
5uRqLmnbKN39HYoNkc/uQzmWRSxLb8TPHBX2ytOq4Qbz78VpERGizD1/sWRKgGAA
UYDtZDVojfZ4eY9m5za3yJxDYuO1FHWmoBJzvHDtyJVS9AtCg/HP6BdFtCSNOpby
xhw1ZRhTMpASGTfw/3TjuIw9yP57sFnN7QMwkjLTUAWcLfc4UXKEQe2KXWWafnP1
Nrq/O4+JuNuzQTZZiDcX68mfp+l1LvjDq+GcTka9u6JxbqesmFYvaQvxYYHc0HKR
IRBZ7ce/+BIoY0ECr2Ahqqai0zlmwIX9NHqjpB0c+pgr0+rrIJa/9n9vIPKNOAuv
-----END RSA PRIVATE KEY-----
```
- Ta sẽ Coppy nội dung của file này, sau đó tại màn hình desktop của máy vật lí ta tạo 1 file text mới và Paste nội dung vừa Coppy vào và lưu lại.
### Cấu hình đăng nhập bằng Keyspair
- Tiếp theo ta sẽ đổi file cấu hình SSH để bắt buộc xác thực sử dụng `Keys pair`. Ta tiến hành truy cập tệp SSH bằng công cụ `vi`để chỉnh sửa file: 
```
[root@client ~]# vi /etc/ssh/sshd_config
```
- Tìm dòng `#PubkeyAuthentication yes`, tại đây ta xóa dấu `#` vì nếu có dấu `#` tức là dòng này đang bị vô hiệu hóa. Sau đó ta thoát và lưu file lại.
### Đẩy Key Public từ `Client` sang `Server`
- Để thực hiện, ta sử dụng lệnh `ssh-copy-id root@(địa chỉ của máy Server)`.  
```
[root@client ~]# ssh-copy-id root@192.168.124.132
```
### SSH từ Client sang Server để kiểm tra việc thực hiện lệnh Copy thành công hay không.
Để có thể SSH sang Server ngay tại Client, ta sử dụng lệnh: `ssh root@(địa chỉ của máy Server)`
```
[root@client ~]# ssh root@192.168.124.132
```

- Khi thành công sẽ có 1 thông báo như sau hiện ra như bên dưới: 
```
Last login: Mon Nov  1 02:27:15 2021 from 192.168.124.1
```
- Tại đây ta sử dụng lệnh `ls -alh` để xem các tệp. Sau khi gõ xong sẽ thấy xuất hiện 1 tệp mới có đuôi là `.ssh`. Ta tiếp tục truy cập vào tệp `.ssh` bằng lệnh `cd .ssh` để kiểm tra. Tại đây ta tiếp tục gõ lệnh `ls -alh`:
```
[root@Sever .ssh]# ls -alh
total 4.0K
drwx------. 2 root root  29 Oct 31 23:50 .
dr-xr-x---. 3 root root 147 Oct 31 23:54 ..
-rw-------. 1 root root 398 Oct 31 23:50 authorized_keys
```
- Tại đây ta thấy file tên `authorized_keys` tức là lệnh `copy` ở trên đã thành công.

### 4.3. SSH bằng Keyspair
![ssh](https://user-images.githubusercontent.com/84270045/140914686-eb7bcf50-1247-4760-b1ce-0a89d352bd2b.png)
- Giờ ta kết thúc phiên đăng nhập này và SSH lại. Sau khi nhập xong địa chỉ IP, ta chọn `Advanced SSH settings` và chọn mục `Use privatekey`. Sau đó ta sẽ chọn file text mà ta đã lưu từ trước bỏ vào và ấn `OK`. Sau khi ấn Enter thì máy sẽ bắt ta nhập `passphrase` mà ta đã tạo trước đó. Tiếp theo phần `login as` thì ta sẽ nhập User name và sau đó ấn Enter.

#### Vậy là ta đã SSH thành công với *Keys Pair* vừa tạo




