---
title: King of Forensics
subtitle: Two challenges in the final-term exam
cover-img: /assets/img/KingofForensics/cover-image.jpg
thumbnail-img:
tags: [Windows, Forensics, Memory, Steganography, CTF]
readtime: true
---

Hi, sau đây là 2 challenge về Digital Forensics mình cùng các teammates thực hiện trong kì thi cuối kì.

# **Challenge 1: King of Memory Forensics**

![CTF](/assets/img/KingofForensics/KingofMemory.png){: .mx-auto.d-block :}

## **Writeup**

Đầu tiên, chúng ta sử dụng `imageinfo` để tìm profile cho file memory dump.

![Mem1](/assets/img/KingofForensics/Mem1.png){: .mx-auto.d-block :}
 
Vì Challenge gợi ý cho chúng ta tìm password của laptop nên chúng ta sẽ sử dụng `hashdump` và `mimikatz` plugin để dump user password, tuy nhiên không tìm thấy được gì.

![Mem2](/assets/img/KingofForensics/Mem2.png){: .mx-auto.d-block :}
 
Sau một thời gian tìm kiếm, nhóm đã tìm được một tool là `Passware Kit Forensics`. Thực hiện chạy và tìm thấy password của laptop.

![Mem3](/assets/img/KingofForensics/Mem3.png){: .mx-auto.d-block :}
 
> **PasswordLaptop: welcometow1**

Tiếp theo, trong quá trình điều tra sử dụng plugin `consoles` để điều tra các lệnh được gõ thì chúng ta phát hiện một rootkit driver được compile.

![Mem4](/assets/img/KingofForensics/Mem4.png){: .mx-auto.d-block :}
 
Sử dụng `filescan` plugin và lệnh grep để tìm kiếm các file có liên quan đến rootkit.
 
![Mem5](/assets/img/KingofForensics/Mem5.png){: .mx-auto.d-block :}

Thực hiện dump driver `HideProcessHookMDL.sys` và sử dụng IDA Pro để phân tích nó thì ta thu được một message bên trong hàm `DriverEntry`.

![Mem6](/assets/img/KingofForensics/Mem6.png){: .mx-auto.d-block :}
 
> **Message: H3y_Y0u_F0und_M3**

Dựa vào kinh nhiệm làm việc với rootkit từ trước, ta nhận thấy rằng driver rootkit này sẽ thực hiện che dấu một tiến trình nào đó khi được kích hoạt và tên của tiến trình được ẩn sẽ được so sánh ở một nơi nào đó trong chương trình. Trong hàm `sub_11006`, ta tìm được lệnh so sánh khả nghi.

![Mem7](/assets/img/KingofForensics/Mem7.png){: .mx-auto.d-block :}

![Mem8](/assets/img/KingofForensics/Mem8.png){: .mx-auto.d-block :}
 
Tìm đến địa chỉ `word_1122E`, ta thu được chuỗi `WindowUpdate`

![Mem9](/assets/img/KingofForensics/Mem9.png){: .mx-auto.d-block :}
 
> **ProcessIsHidden: WindowUpdate**

Cuối cùng, ta thực hiện dump file `WindowUpdate.exe` và thực thi nó trên một máy ảo Windows.

![Mem9](/assets/img/KingofForensics/Mem10.png){: .mx-auto.d-block :}
 
> **WhatDoesTheMaliciousCodeShow: easy_malware_detection**

Kết hợp tất cả các dữ liệu trên, ta có được flag cuối cùng:

> **FLAG: W1{welcometow1-WindowUpdate-H3y_Y0u_F0und_M3-easy_malware_detection}**

# **Challenge 2: King of Stegano**

![Stegano](/assets/img/KingofForensics/KingofStegano.png){: .mx-auto.d-block :}

## **Writeup**

Kiểm tra file với file, ta thấy file không được coi là png.

![Stegano1](/assets/img/KingofForensics/Stegano1.png){: .mx-auto.d-block :}
 
Kiểm tra với `xxd`, ta không thấy signature của PNG và phần cuối cũng thiếu chunk `IEND`.

![Stegano2](/assets/img/KingofForensics/Stegano2.png){: .mx-auto.d-block :}
 
Ta thêm signature của file png là `89 50 4E 47 0D 0A 1A 0A`, thấy rằng nó cũng thiếu một chunk quan trọng là `IHDR`, ta thêm `00 00 00 0D 49 48 44 52`, và phần ở byte 0 đến chunk `IDAT` sẽ thuộc chunk `IHDR`. Và cuối cùng là thêm `IEND` là `49 45 4E 44 AE 42 60 82`.
 
![Stegano3](/assets/img/KingofForensics/Stegano3.png){: .mx-auto.d-block :}

![Stegano4](/assets/img/KingofForensics/Stegano4.png){: .mx-auto.d-block :}

Kiểm tra lại với `pngcheck` và không có lỗi.

![Stegano5](/assets/img/KingofForensics/Stegano5.png){: .mx-auto.d-block :}
 
Sau một lúc thực hiện kiểm tra với các tool steganography nhưng không có kết quả, thử với tool `appa`, decode và có được phần đầu của flag: `W1-Y0u-4r3`.

![Stegano6](/assets/img/KingofForensics/Stegano6.png){: .mx-auto.d-block :}
 
Tiếp theo đến với file zip `Crackme.zip`, thấy có 2 file một file là file `flag.txt`, hai là file `Chall.png`. Rất có thể đây là file `Chall.png` trước khi bị chỉnh sửa, từ đây có thể thực hiện crack file zip nhờ vào kỹ thuật  `plaintext-known`.

![Stegano7](/assets/img/KingofForensics/Stegano7.png){: .mx-auto.d-block :}
 
Sử dụng `pkcrack` để crack, ta cần một file zip chứa file `Chall.png` ta đã có (đặt tên lại là `PlainChall.png`). Kiểm tra lại đảm bảo 2 file cùng giá trị `CRC`.

![Stegano8](/assets/img/KingofForensics/Stegano8.png){: .mx-auto.d-block :}

![Stegano9](/assets/img/KingofForensics/Stegano9.png){: .mx-auto.d-block :}
 
 
Thực hiện crack và đã thành công
 
![Stegano10](/assets/img/KingofForensics/Stegano10.png){: .mx-auto.d-block :} 
 
Thực hiện unzip file đã được crach và có nửa flag còn lại: `-K1ng-0f-F0r3ns1cs`.

![Stegano11](/assets/img/KingofForensics/Stegano11.png){: .mx-auto.d-block :}

![Stegano12](/assets/img/KingofForensics/Stegano12.png){: .mx-auto.d-block :}

Kết hợp 2 nửa flag, ta có flag hoàn chỉnh:
 
> **Flag: W1{Y0u-4r3-K1ng-0f-F0r3ns1cs}**

![End](/assets/img/KingofForensics/End.png){: .mx-auto.d-block :}

Finishing with the two First Bloods hihi 😍