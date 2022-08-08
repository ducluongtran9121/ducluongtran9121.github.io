---
title: \[Writeup\] Dump.raw Memory Forensis
subtitle: Challenge 2 in mid-term Forensics
cover-img: /assets/img/Memory.dmp/Cover.png
thumbnail-img:
tags: [Windows, Forensics, Memory, CTF]
readtime: true
---

# **DUMP.RAW**

![CTF](/assets/img/Dump.raw/CTF.png){: .mx-auto.d-block :}

## **Description**

Dường như đã có hành vi bất thường trên laptop của NHK, bạn có thể giúp chúng tôi điều tra: 
- Thu thập các file bất thường để ghép mảnh flag.
- Liệu họ có để lại những dấu vết trên trình duyệt web?.
- Và hình như kẻ xâm nhập bằng một cách nào đó đã lấy được password laptop của NHK. Hãy tìm password đó.

## **Writeup**

Win7SP1x64 là profile được chọn.

![image1](/assets/img/Dump.raw/image1.png){: .mx-auto.d-block :}

#### ***Challenge 1:** Ghép các file khả nghi thành flag.*

Sau một thời gian kiểm tra các tiến trình không có gì đặc biệt, mình sử dụng plugin `shellbags` để trích xuất thông tin từ registry xem timeline lần cuối user truy cập thư mục và file gì.

![image2](/assets/img/Dump.raw/image2.png){: .mx-auto.d-block :}

Điều đặc biệt xuất hiện 3 file khả nghi là `dr3w.jpg`, `flag.txt` và `h4lf-fl4g.rar`. 

![image3](/assets/img/Dump.raw/image3.png){: .mx-auto.d-block :}
 
Sử dụng filescan để kiếm file flag.txt
 
![image4](/assets/img/Dump.raw/image4.png){: .mx-auto.d-block :}

Thực hiện dumpfiles dựa trên virtual address của file, ta có được file flag.txt. Thực hiện xem nội dung thì mình có được 1 nửa flag như sau: `inseclab{w3lcom3_t0`
 
![image5](/assets/img/Dump.raw/image5.png){: .mx-auto.d-block :}
 
Thực hiện tương tự với file `h4lf-fl4g.rar`. Tuy nhiên file rar này có chứa mật khẩu. 

![image6](/assets/img/Dump.raw/image6.png){: .mx-auto.d-block :}

Đến đây mình sẽ dùng `john the ripper` để crack mật khẩu. Sử dụng `rar2john` để hash file rar về dạng john hiểu được và lưu vào `hash_flag.txt`. 

![image7](/assets/img/Dump.raw/image7.png){: .mx-auto.d-block :}
 
Download wordlist rockyou.txt và thực hiện câu lệnh sau:

```console
ubuntu@ubuntu:~$ john –wordlist=rockyou.txt hash_flag.txt.
```

![image8](/assets/img/Dump.raw/image8.png){: .mx-auto.d-block :}

Ta có ngay password của file rar là `r0cky0u`.

![image9](/assets/img/Dump.raw/image9.png){: .mx-auto.d-block :}
 
Giải nén file rar kèm mật khẩu tìm được, ta có file `h4lf_fl4g.txt`.

![image10](/assets/img/Dump.raw/image10.png){: .mx-auto.d-block :}
 
Thực hiện xem nội dung, ta có nửa flag còn lại: `_th3_w0rld_NHK}`
Kết hợp 2 nửa flag, ta có flag cần tìm.
> **FLAG: inseclab{w3lcom3_t0_th3_w0rld_NHK}**

#### ***Challenge 2:** Tìm flag từ lịch sử web.*

Sau khi sử dụng `pslist` thấy user đã sử dụng chrome để truy cập web. Ta thực hiện download plugin `chromehistory` về để kiểm tra lịch sử chrome.
 
![image11](/assets/img/Dump.raw/image11.png){: .mx-auto.d-block :}

Kéo xuống dưới ta thấy một file pastebin bất thường.

![image12](/assets/img/Dump.raw/image12.png){: .mx-auto.d-block :}

Truy cập vào link pastebin, ta có được một đường link share từ Google Drive.

![image13](/assets/img/Dump.raw/image13.png){: .mx-auto.d-block :}

Truy cập ta có được 1 file text có nội dung khả nghi.

![image14](/assets/img/Dump.raw/image14.png){: .mx-auto.d-block :}

Thực hiện sử dụng xxd để xem file empty.txt dạng hex. 
 
![image15](/assets/img/Dump.raw/image15.png){: .mx-auto.d-block :}
 
Có một điều đặc biệt đằng sau chuỗi ban đầu là một dãy có mã hex `0x20`, `0x09`, `0x2A` xen kẽ nhau ở dưới. Có vẻ như đây là một hình thức ẩn giấu thông tin gì đó. Thực hiện tra google, mình phát hiện đó là ngôn ngữ lập trình Whitespace. 

![image16](/assets/img/Dump.raw/image16.png){: .mx-auto.d-block :}

Một ngôn ngữ khá lạ và mình cần tool giải mã. Tìm hiểu và thấy `stegsnow` là một tool có thể làm được điều đó.

![image17](/assets/img/Dump.raw/image17.png){: .mx-auto.d-block :}
 
Thực hiện install và sử dụng `stegsnow -C empty.txt`, ta có ngay flag.

![image18](/assets/img/Dump.raw/image18.png){: .mx-auto.d-block :}

> **FLAG: inseclab{y0u_c4n_s33_fl4g}**

#### ***Challenge 3:** Tìm mật khẩu máy NHK.*

Sử dụng plugin `hashdump` để xem thông tin các account có trong máy. Mật khẩu của user `NHK-Inseclab` là thứ ta cần tìm.

![image19](/assets/img/Dump.raw/image19.png){: .mx-auto.d-block :}

Thực hiện crack password của NHK-Inseclab bằng Crackstation không thành công.

![image20](/assets/img/Dump.raw/image20.png){: .mx-auto.d-block :}
 
Sử dụng thử plugin `lsadump` , thì chỉ thấy password mặc định của máy ảo và một password của OpenSSH.

![image21](/assets/img/Dump.raw/image21.png){: .mx-auto.d-block :}
 
Chưa có gì đặc biệt. Đến đây, dựa vào kinh nghiệm Lab1 Memory Forensics, mình nhớ đến plugin `mimikatz` trong volatility có thể giải mã các tài khoản Windows.Các bước cài đặt có thể tham khảo [link](https://virtualception.wordpress.com/2016/05/15/volatility-mimikatz-plugin-installation-on-ubuntu-10/).
 
![image22](/assets/img/Dump.raw/image22.png){: .mx-auto.d-block :}

Sử dụng `mimikatz`, ta có ngay mật khẩu của user NHK-Inseclab là `AntiNHK`.

> **FLAG: inseclab{AntiNHK}**