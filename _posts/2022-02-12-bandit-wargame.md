---
title: WRITEUP - Bandit Wargame
subtitle: Wargame về Linux 
readtime: true
thumbnail-img: 
cover-img: 
tags: [Linux, Wargame]
---

Đây là series về Bandit - một Wargame giúp chúng ta học Linux từ cơ bản đến nâng cao một chút. Dưới đây là toàn bộ writeup mà nhóm PickMeBoiz bọn mình đã hoàn thành.

# [Bandit](https://overthewire.org/wargames/bandit/) - PickMeBoiz 

## Level 0

```console
pickmeboiz@NT101-M11-ANTN:~$ ssh bandit0@bandit.labs.overthewire.org -p 2220
bandit0@bandit.labs.overthewire.org's password: bandit0
bandit0@bandit:~$
```

## Level 0 → 1

- Xem password trong file `readme`:

  ```console
  bandit0@bandit:~$ cat readme
  boJ9jbbUNNfktd78OOpsqOltutMc3MY1
  ```

## Level 1 → 2

- Kết nối đến level 1:

  ```console
  pickmeboiz@NT101-M11-ANTN:~$ ssh bandit1@bandit.labs.overthewire.org -p 2220
  bandit1@bandit.labs.overthewire.org's password: boJ9jbbUNNfktd78OOpsqOltutMc3MY1
  bandit1@bandit:~$
  ```

- Xem password trong file `-`:

  ```console
  bandit1@bandit:~$ cat ./-
  CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9
  ```

## Level 2 → 3

- Kết nối đến level 2:

  ```console
  pickmeboiz@NT101-M11-ANTN:~$ ssh bandit2@bandit.labs.overthewire.org -p 2220
  bandit2@bandit.labs.overthewire.org's password: CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9
  bandit2@bandit:~$
  ```

- Xem password trong file `spaces in this filename`:

  ```console
  bandit2@bandit:~$ cat 'spaces in this filename'
  UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK
  ```

## Level 3 → 4

- Kết nối đến level 3:

  ```console
  pickmeboiz@NT101-M11-ANTN:~$ ssh bandit3@bandit.labs.overthewire.org -p 2220
  bandit3@bandit.labs.overthewire.org's password: UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK
  bandit3@bandit:~$
  ```

- Trong folder hiện tại có chứa folder `inhere`:

  ```console
  bandit3@bandit:~$ ls
  inhere
  ```

- Trong `inhere`, kiểm tra toàn bộ file, password được chứa trong file `.hidden`:

  ```console
  bandit3@bandit:~$ cd inhere
  bandit3@bandit:~/inhere$ ls -a
  .  ..  .hidden
  bandit3@bandit:~/inhere$ cat .hidden
  pIwrPrtPN36QITSp3EQaw936yaFoFgAB
  ```

## Level 4 → 5

- Kết nối đến level 4:

  ```console
  pickmeboiz@NT101-M11-ANTN:~$ ssh bandit4@bandit.labs.overthewire.org -p 2220
  bandit4@bandit.labs.overthewire.org's password: pIwrPrtPN36QITSp3EQaw936yaFoFgAB
  bandit4@bandit:~$
  ```

- Trong folder hiện tại có chứa folder `inhere`:

  ```console
  bandit4@bandit:~$ ls
  inhere
  ```

- Trong `inhere`, kiểm tra toàn bộ file, `cat` toàn bộ file. Password tìm được là `koReBOKuIDDepwhWk7jZC0RTdopnAYKh`:

  ```console
  bandit4@bandit:~$ cd inhere
  bandit4@bandit:~/inhere$ ls -a
  .  ..  -file00  -file01  -file02  -file03  -file04  -file05  -file06  -file07  -file08  -file09
  bandit4@bandit:~/inhere$ for f in ./-*; do (cat "${f}";echo) done
  ...
  koReBOKuIDDepwhWk7jZC0RTdopnAYKh
  ...
  ```

## Level 5 → 6

- Kết nối đến level 5:

  ```console
  pickmeboiz@NT101-M11-ANTN:~$ ssh bandit5@bandit.labs.overthewire.org -p 2220
  bandit5@bandit.labs.overthewire.org's password: koReBOKuIDDepwhWk7jZC0RTdopnAYKh
  bandit5@bandit:~$
  ```

- Trong folder hiện tại có chứa folder `inhere`:

  ```console
  bandit4@bandit:~$ ls
  inhere
  ```

- Trong `inhere`, tìm toàn bộ file theo cầu: `human-readable`, `1033 bytes in size`, `not executable`:

  ```
  find . -type f -size 1033c ! -executable -exec ls -lh {} \;
  ```

  ```console
  bandit5@bandit:~$ cd inhere
  bandit5@bandit:~/inhere$ find . -type f -size 1033c ! -executable -exec ls -lh {} \;
  -rw-r----- 1 root bandit5 1.1K May  7  2020 ./maybehere07/.file2
  bandit5@bandit:~/inhere$ cat ./maybehere07/.file2
  DXjZPULLxYr17uwoI01bNLQbtFemEgo7
  ```

## Level 6 → 7

- Kết nối đến level 6:

  ```console
  pickmeboiz@NT101-M11-ANTN:~$ ssh bandit6@bandit.labs.overthewire.org -p 2220
  bandit6@bandit.labs.overthewire.org's password: DXjZPULLxYr17uwoI01bNLQbtFemEgo7
  bandit6@bandit:~$
  ```

- Tìm toàn bộ file trong server theo cầu: `owned by user bandit7`, `owned by group bandit6`, `33 bytes in size`:

  ```
  find / -user bandit7 -group bandit6 -type f -size 33c -exec ls -a {} \;
  ```

  ```console
  bandit6@bandit:~$ find / -user bandit7 -group bandit6 -type f -size 33c -exec ls -a {} \;
  ...
  /var/lib/dpkg/info/bandit7.password
  ...

  bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password
  HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs
  ```

## Level 7 → 8

- Kết nối đến level 7:

  ```console
  pickmeboiz@NT101-M11-ANTN:~$ ssh bandit7@bandit.labs.overthewire.org -p 2220
  bandit7@bandit.labs.overthewire.org's password: HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs
  bandit7@bandit:~$
  ```

- Kiểm tra trong directory hiện tại, có file `data.txt`:

  ```console
  bandit7@bandit:~$ ls -a
  .  ..  .bash_logout  .bashrc  data.txt  .profile
  ```

- Tìm dòng có chứa `millionth`:

  ```console
  bandit7@bandit:~$ cat data.txt | grep millionth
  millionth       cvX2JJa4CFALtqS87jk27qwqGhBM9plV
  ```

## Level 8 → 9

- Kết nối đến level 8:

  ```console
  pickmeboiz@NT101-M11-ANTN:~$ ssh bandit8@bandit.labs.overthewire.org -p 2220
  bandit8@bandit.labs.overthewire.org's password: cvX2JJa4CFALtqS87jk27qwqGhBM9plV
  bandit8@bandit:~$
  ```

- Kiểm tra trong directory hiện tại, có file `data.txt`:

  ```console
  bandit8@bandit:~$ ls -a
  .  ..  .bash_logout  .bashrc  data.txt  .profile
  ```

- Tìm dòng chỉ xuất hiện 1 lần ( `-u` ):

  ```
  cat data.txt | sort | uniq -u
  ```

  ```console
  bandit8@bandit:~$ cat data.txt | sort | uniq -u
  UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR
  ```

## Level 9 → 10

- Kết nối đến level 9:

  ```console
  pickmeboiz@NT101-M11-ANTN:~$ ssh bandit9@bandit.labs.overthewire.org -p 2220
  bandit9@bandit.labs.overthewire.org's password: UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR
  bandit9@bandit:~$
  ```

- Kiểm tra trong directory hiện tại, có file `data.txt`:

  ```console
  bandit9@bandit:~$ ls -a
  .  ..  .bash_logout  .bashrc  data.txt  .profile
  ```

- Tìm password bắt đầu bằng vài ký tự `=` và có thể đọc được. Password là `truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk`:

  ```
  strings data.txt | grep -P "=+.+"
  ```

  ```console
  bandit8@bandit:~$ strings data.txt | grep -P "=+.+"
  ========== the*2i"4
  =:G e
  ========== password
  <I=zsGi
  Z)========== is
  A=|t&E
  c^ LAh=3G
  *SF=s
  &========== truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk
  S=A.H&^
  ```

## Level 10 → 11

- Kết nối đến level 10:

  ```console
  pickmeboiz@NT101-M11-ANTN:~$ ssh bandit10@bandit.labs.overthewire.org -p 2220
  bandit10@bandit.labs.overthewire.org's password: truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk
  bandit10@bandit:~$
  ```

- Kiểm tra trong directory hiện tại, có file `data.txt`:

  ```console
  bandit10@bandit:~$ ls -a
  .  ..  .bash_logout  .bashrc  data.txt  .profile
  ```

- Decode data trong data.txt:

  ```
  cat data.txt | base64 --decode
  ```

  ```console
  bandit10@bandit:~$ cat data.txt | base64 --decode
  The password is IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR
  ```

## Level 11 → 12

- Kết nối đến level 11:

  ```console
  pickmeboiz@NT101-M11-ANTN:~$ ssh bandit11@bandit.labs.overthewire.org -p 2220
  bandit11@bandit.labs.overthewire.org's password: IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR
  bandit11@bandit:~$
  ```

- Kiểm tra trong directory hiện tại, có file `data.txt`:

  ```console
  bandit10@bandit:~$ ls -a
  .  ..  .bash_logout  .bashrc  data.txt  .profile
  ```

- Shift bảng chữ đi 13 chữ cái. Dùng `tr` chuyển các chữ các trong password từ set `[N-ZA-Mn-za-m]` (bắt đầu từ M và kết thúc tại N) về `[A-Za-z]` (Bắt đầu từ A và kết thúc tại Z). Vd: N được map qua A.

  ```
  tr 'N-ZA-Mn-za-m' 'A-Za-z'
  ```

  ```console
  bandit11@bandit:~$ cat data.txt | tr 'N-ZA-Mn-za-m' 'A-Za-z'
  The password is 5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu
  ```

## Level 12 → 13

- Kết nối đến level 12:

  ```console
  pickmeboiz@NT101-M11-ANTN:~$ ssh bandit12@bandit.labs.overthewire.org -p 2220
  bandit12@bandit.labs.overthewire.org's password: 5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu
  bandit12@bandit:~$
  ```

- Kiểm tra trong directory hiện tại, có file `data.txt`:

  ```console
  bandit12@bandit:~$ ls -a
  .  ..  .bash_logout  .bashrc  data.txt  .profile
  ```

- Tạo folder lưu output file:

  ```console
  bandit12@bandit:~$ mkdir /temp/pickmeboiz
  ```

- Reverse hex dump:

  ```console
  bandit12@bandit:~$ xxd -r data.txt /tmp/pickmeboiz/out
  bandit12@bandit:~$ cd tmp/temp/pickmeboiz
  bandit12@bandit:/tmp/pickmeboiz$
  ```

- Lần 1:

  ```console
  bandit12@bandit:/tmp/pickmeboiz$ file out
  out: gzip compressed data,...
  bandit12@bandit:/tmp/pickmeboiz$ cp out out.gz
  bandit12@bandit:/tmp/pickmeboiz$ gzip -d -f out.gz
  ```

- Lần 2:

  ```console
  bandit12@bandit:/tmp/pickmeboiz$ file out
  out: bzip2 compressed data,...
  bandit12@bandit:/tmp/pickmeboiz$ cp out out.bz2
  bandit12@bandit:/tmp/pickmeboiz$ bzip2 -d -f out.bz2
  ```

- Lần 3:

  ```console
  bandit12@bandit:/tmp/pickmeboiz$ file out
  out: gzip compressed data,...
  bandit12@bandit:/tmp/pickmeboiz$ cp out out.gz
  bandit12@bandit:/tmp/pickmeboiz$ gzip -d -f out.gz
  ```

- Lần 4:

  ```console
  bandit12@bandit:/tmp/pickmeboiz$ file out
  bandit12@bandit:/tmp/pickmeboiz$ tar -xf out
  bandit12@bandit:/tmp/pinanek23$ ls
  data5.bin  out
  ```

- Lần 5:

  ```console
  bandit12@bandit:/tmp/pickmeboiz$ file data5.bin
  data5.bin: POSIX tar archive (GNU)
  bandit12@bandit:/tmp/pickmeboiz$ tar -xf out
  bandit12@bandit:/tmp/pinanek23$ ls
  data5.bin  data6.bin
  ```

- Lần 6:

  ```console
  bandit12@bandit:/tmp/pickmeboiz$ file data6.bin
  data6.bin: bzip2 compressed data
  bandit12@bandit:/tmp/pickmeboiz$ bzip2 -d -f data6.bin
  bzip2: Can't guess original name for data6.bin -- using data6.bin.out
  ```

- Lần 7:

  ```console
  bandit12@bandit:/tmp/pickmeboiz$ file data6.bin.out
  data6.bin.out: POSIX tar archive
  bandit12@bandit:/tmp/pickmeboiz$ tar -xf data6.bin.out
  bandit12@bandit:/tmp/pickmeboiz$ ls
  data5.bin  data6.bin.out  data8.bin
  ```

- Lần 8:

  ```console
  bandit12@bandit:/tmp/pickmeboiz$ file data8.bin
  data8.bin: gzip compressed data
  bandit12@bandit:/tmp/pickmeboiz$ cp data8.bin data8.bin.gz
  bandit12@bandit:/tmp/pickmeboiz$ gzip -d -f data8.bin.gz
  ```

- Cuối cùng:

  ```console
  bandit12@bandit:/tmp/pickmeboiz$ file data8.bin
  data8.bin: ASCII text
  bandit12@bandit:/tmp/pickmeboiz$ cat data8.bin
  The password is 8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL
  ```

## Level 13 → 14

- Kết nối đến level 13. Password level 14 nằm ở `/etc/bandit_pass/bandit14`:

  ```console
  pickmeboiz@NT101-M11-ANTN:~$ ssh bandit13@bandit.labs.overthewire.org -p 2220
  bandit13@bandit.labs.overthewire.org's password: 8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL
  bandit13@bandit:~$
  ```

- Kiểm tra trong directory hiện tại, có file `sshkey.private`, dùng để remote vào `bandit14`:

  ```console
  bandit13@bandit:~$ ls -a
  .  ..  .bash_logout  .bashrc  .profile  sshkey.private
  ```

## Level 14 → 15

- Tiếp tục tại user `bandit13`, kết nối đến `bandit14` sử dụng ssh private key của user `bandit14`:

  ```console
  bandit13@bandit:~$ ssh -i sshkey.private bandit14@localhost
  ```

- Và submit password lên localhost port `30000`:

  ```console
  bandit14@bandit:~$ cat /etc/bandit_pass/bandit14 | nc localhost 30000
  Correct!
  BfMYroe26WYalil77FoDi9qh59eK5xNr
  ```

## Level 15 → 16

- Kết nối đến level 15:

  ```console
  pickmeboiz@NT101-M11-ANTN:~$ ssh bandit15@bandit.labs.overthewire.org -p 2220
  bandit15@bandit.labs.overthewire.org's password: BfMYroe26WYalil77FoDi9qh59eK5xNr
  bandit15@bandit:~$
  ```

- Submit password level 15 đến localhost port `30001` sử dụng `OpenSSL`:

  ```console
  bandit15@bandit:~$ echo BfMYroe26WYalil77FoDi9qh59eK5xNr | openssl s_client -connect localhost:30001  -ign_eof
  ...
  ---
  Correct!
  cluFn7wTiGryunymYOu4RcffSxQluehd
  ...
  ```

## Level 16 → 17

- Kết nối đến level 16:

  ```console
  pickmeboiz@NT101-M11-ANTN:~$ ssh bandit16@bandit.labs.overthewire.org -p 2220
  bandit16@bandit.labs.overthewire.org's password: cluFn7wTiGryunymYOu4RcffSxQluehd
  bandit16@bandit:~$
  ```

- Check tất cả các port có kết nối bằng `nmap`:

  ```console
  bandit16@bandit:~$ nmap localhost -p 31000-32000

  Starting Nmap 7.40 ( https://nmap.org ) at 2021-09-28 13:21 CEST
  Nmap scan report for localhost (127.0.0.1)
  Host is up (0.00022s latency).
  Not shown: 996 closed ports
  PORT      STATE SERVICE
  31046/tcp open  unknown
  31518/tcp open  unknown
  31691/tcp open  unknown
  31790/tcp open  unknown
  31960/tcp open  unknown

  Nmap done: 1 IP address (1 host up) scanned in 0.08 seconds
  ```

- Check port có SSL sử dụng `nmap`:

  ```console
  bandit16@bandit:~$ for port in 31046 31518 31691 31790 31960; do (if nmap --script ssl-enum-ciphers localhost -p "$port"| grep "ssl" > /dev/null; then(echo "$port"); fi) done
  31518
  31790
  ```

- Sau đó check từng port với `OpenSSL` và port `31790` chính xác:

  ```console
  bandit16@bandit:~$ echo cluFn7wTiGryunymYOu4RcffSxQluehd | openssl s_client -connect localhost:31790 -ign_eof
  Correct!
  -----BEGIN RSA PRIVATE KEY-----
  MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
  imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
  Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
  DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
  JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
  x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
  KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
  J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
  d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
  YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
  vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
  +TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
  8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
  SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
  HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
  SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
  R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
  Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
  R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
  L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
  blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
  YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
  77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
  dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
  vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
  -----END RSA PRIVATE KEY-----
  ```

## Level 17 → 18

- Kết nối đến level 17:

  - Tạo file `bandit17` với ssh key ở trên.

    ```console
    pickmeboiz@NT101-M11-ANTN:~$ touch bandit17
    ```

  - Thêm key vào file.
  - Vì là private key nên chỉ để user hiện tại có quyền xem và sửa.

    ```console
      pickmeboiz@NT101-M11-ANTN:~$ chmod 600 bandit17
    ```

    ```console
    pickmeboiz@NT101-M11-ANTN:~$ ssh -i bandit17 bandit17@bandit.labs.overthewire.org -p 2220
    bandit17@bandit:~$
    ```

- So sánh 2 file password bằng lệnh diff với unified format. Password sẽ là `kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd`:

  ```console
  bandit17@bandit:~$ diff -u passwords.old passwords.new
  --- passwords.old       2020-05-07 20:14:35.376729786 +0200
  +++ passwords.new       2020-05-07 20:14:35.528729706 +0200
  @@ -39,7 +39,7 @@
  sydIUj42mUfYK9xw1S9aPPB72rgagnxh
  pcpkwztEjxg5EK0HABjmEvGUSCSdQW4F
  LlomcOUT6d7lA2cJrYhCEhCChKCPrRao
  -w0Yfolrc5bwjS4qw5mq1nnQi6mF03bii
  +kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd
  TVzFbgWpqUPE4fwAJPCz4rT7GemAZUjz
  WCETP1i90TZJSbKZ24ly5rhNKva8sSdy
  2o4oJXwgWyWIdKb9WpNDFUcWXlcghSzR
  ```

## Level 18 → 19

- Kết nối đến level 18.

  - Khi kết nối vào thì lập tức sẽ thoát ra ngoài khỏi `bandit18`:

    ```console
    pickmeboiz@NT101-M11-ANTN:~$ ssh bandit18@bandit.labs.overthewire.org -p 2220
    bandit18@bandit.labs.overthewire.org's password: kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd
    bandit15@bandit:~$
    ...
    Byebye !
    pickmeboiz@NT101-M11-ANTN:~$
    ```

  - Vậy ta sẽ sử dụng `Pseudo-TTY` và truy cập thẳng vào `/bin/sh`. `Pseudo-TTY` là 2 file, 1 file master được sử dụng trên các ứng dụng remote (như `SSH`), còn file slave sẽ được dùng bởi user process. Và file master có thể sử dụng terminal. Ta sẽ sử dụng `/bin/sh` nhằm tránh bị block bởi `bash`
    ```console
    pickmeboiz@NT101-M11-ANTN:~$ ssh bandit18@bandit.labs.overthewire.org -p 2220 -t /bin/sh
    bandit18@bandit.labs.overthewire.org's password: kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd
    $
    ```

- Kiểm tra trong directory hiện tại, có file `readme`:

  ```console
  $ ls -a
  readme
  ```

- Và xem password của level 18:

  ```console
  $ cat readme
  IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x
  ```

## Level 19 → 20

---
***Description***

To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (`/etc/bandit_pass`), after you have used the setuid binary.

---
***Writeup***

Sau khi login vào bandit19, ta thực hiện lệnh `ls` để kiểm tra thì thấy một file `bandit20-do`. Đây là một file [setuid](https://en.wikipedia.org/wiki/Setuid) có thể thực thi những lệnh mà chỉ user bandit20 mới có quyền thực hiện. Thử thực thi `./bandit20-do` thì kết quả trả về là `Run a command as another user`, điều này có nghĩa là mặc dù ta đang là user bandit19 nhưng với file này ta có thể thực thi các lệnh với permission của user bandit20. 

![19](/assets/img/Bandit/19.png){: .mx-auto.d-block :}

Vậy thì còn chần chờ gì nữa mà không dùng lệnh `cat /etc/bandit_pass/bandit20` cho nó đọc password. 


> Ta thu được password cho bandit20 : `GbKksEFF4yrVs6il55v6gwY5aVje5f0j`

## Level 20 → 21
---
***Description***

There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

NOTE: Try connecting to your own network daemon to see if it works as you think

---
***Writeup***

Sau khi login vào bandit20, tiếp tục `ls` thì xuất hiện file `suconnect`. Thử thực thi file thì kết quả trả về cách dùng với arg là `portnumber`, đồng thời mô tả chức năng của file này là để thực hiện kết nối TCP tới localhost thông qua port đã xác định (thực ra nó giống với lệnh `nc 127.0.0.1 <portnumber>`). 
 

![20_1](/assets/img/Bandit/20_1.png){: .mx-auto.d-block :}

Kết hợp description của đề bài, ta thực hiện truyền file password của bandit20 bằng bind shell qua port bất kỳ, ở đây em chọn port 8888. Khi đó, nó trả về password của bandit21.

![20_2](/assets/img/Bandit/20_2.png){: .mx-auto.d-block :}
![20_3](/assets/img/Bandit/20_3.png){: .mx-auto.d-block :}

> Ta thu được password cho bandit21 : `gE269g2h3mw3pwgrj0Ha9Uoqen1c9DGr`

## Level 21 → 22
---
***Description***

A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in `/etc/cron.d/` for the configuration and see what command is being executed.

---
***Writeup***

Sau khi login vào bandit21, ta xem xem thư mục `/etc/cron.d/` có gì. Đây là thư mục chứa các file cấu hình những lệnh hay chương trình sẽ được thực hiện trong thời gian đã set (theo giờ, theo ngày, theo tuần, ...). Bởi vì ta đang đi tìm password cho bandit22 nên ta sẽ xem file `cronjob_bandit22`. Có thể thấy nó sẽ thực thi một file shell script theo mỗi phút. 

![21_1](/assets/img/Bandit/21_1.png){: .mx-auto.d-block :}

Mày mò xem đoạn script đó chứa gì bằng lệnh `cat \usr\bin\cronjob_bandit22.sh`. Hóa ra nó ghi password của bandit22 vào một file trong thư mục tmp sau khi đã cấp quyền bằng `chmod`. Việc còn lại chỉ là xem nội dung password bandit22 bằng lệnh `cat` đơn giản như hình dưới.

![21_2](/assets/img/Bandit/21_2.png){: .mx-auto.d-block :}


> Ta thu được password cho bandit22 : `Yk7owGAcWjwMVRwrTesJEwB7WVOiILLI`

## Level 22 → 23
---
***Description***

A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in `/etc/cron.d/` for the configuration and see what command is being executed.

NOTE: Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.

---
***Writeup***

Tương tự như level trên, lần này ta sẽ xem file `cronjob_bandit23`. Có thể thấy nó vẫn thực thi một file shell script theo mỗi phút

![22_1](/assets/img/Bandit/22_1.png){: .mx-auto.d-block :}

Nội dung của file script đại loại gồm 2 bước:
- *Bước 1*: Thực hiện mã hóa md5 chuỗi `"I am user bandit23"` do biến `myname` ở đây là chính là `bandit23` (Đọc description cũng sẽ hiểu được ý đồ đó). 
- *Bước 2*: Thực thi câu lệnh ghi password của bandit23 vào file có tên chính là chuỗi md5 đã tạo ở *Bước 1* ở thư mục `/tmp`.

![22_2](/assets/img/Bandit/22_2.png){: .mx-auto.d-block :}

Ta sẽ thực hiện lấy chuỗi md5 cần tìm bằng câu lệnh như hình dưới. Chỉ cần đọc nội dung trong file `\tmp\(md5)` là lấy được password cho bandit23 goy.

![22_3](/assets/img/Bandit/22_3.png){: .mx-auto.d-block :}

> Ta thu được password cho bandit23 : `jc1udXuA1tiHqjIsL8yaapX5XIAI6i0n`

## Level 23 → 24
---
***Description***

A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in `/etc/cron.d/` for the configuration and see what command is being executed.

NOTE: This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!

NOTE 2: Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…


---
***Writeup***

Bài này lại tiếp tục dính đến `cron.d`, và vẫn cách thức tiến hành như cũ ta sẽ xem nội dung của file shell script bandit24. Đoạn script này được tóm tắt như sau: 

- *Bước 1*: Thực hiện move đến thư mục `/var/spool/bandit24` do biến `myname` ở đây chính là bandit24.

- *Bước 2*: Thực thi một vòng for có chức năng thực thi và đồng thời xóa tất cả các scripts có trong thư mục hiện tại. Tuy nhiên, khi nó gặp script của user `bandit23`, chương trình sẽ timeout **60s** trước khi thực thi nó rồi xóa. 

![23_1](/assets/img/Bandit/23_1.png){: .mx-auto.d-block :}

Như vậy, dựa vào description ta phải tạo một file `script.sh` thuộc `bandit23` rồi nhét qua `/var/spool/bandit24`. Ta tạo một directory `/tmp/file/` để thực hiện lưu đoạn script cần viết vào đó. 

![23_2](/assets/img/Bandit/23_2.png){: .mx-auto.d-block :}

Vậy ý tưởng của file `script.sh` cần viết là gì? Bởi vì ta cần đi tìm password của **bandit24** trong khi các scripts trong `/var/spool/bandit24` sẽ được thực thi bởi user **bandit24**, nên ta cần viết câu lệnh ghi password `cat \etc\bandit_pass\bandit24` như thường lệ vào một file bất kì, ở đây là file `bandit24_pass` nằm trong thư mục `\tmp\file\` luôn.

![23_3](/assets/img/Bandit/23_3.png){: .mx-auto.d-block :}

Sau khi tạo xong đoạn script, ta cần set quyền cho cả thư mục `\tmp\file\` và file `script.sh`, ở đây em cấp full quyền `chmod 777` luôn. Lí do phải làm như vậy để tí nữa, sau khi `script.sh` được thực thi bởi user **bandit24** thì mới thấy file `bandit24_pass` nằm trong thư mục `\tmp\file\`, nếu không sẽ không có.  

![23_4](/assets/img/Bandit/23_4.png){: .mx-auto.d-block :}

Thực hiện copy file `script.sh` sang thư mục `\var\spool\bandit24\` để nó thực thi giùm file `script.sh`. Đợi đúng **60s** thì có file `bandit24_pass` bên thư mục `\tmp\file\`. Giờ thì chỉ có việc `cat` mà xem password nữa hoy.

![Hinh](/assets/img/Bandit/23_5.png){: .mx-auto.d-block :}

> Ta thu được password cho bandit24 : `UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ`

## Level 24 → 25
---
***Description***

A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.

---
***Writeup***

Đọc *Description* ta có thể hiểu rằng, có một service đang thực hiện lắng nghe trên port **30002** có nhiệm vụ trả về password của **bandit25** nếu như truyền cho nó password của **bandit24** cộng với một pincode gồm 4 chữ số. Ta đã có password của **bandit24**, nên chỉ cần **bruteforce** 10000 số của pincode để có được password cho **bandit25**. Chú ý **pincode** và pasword **bandit24** ngăn cách nhau bởi 1 space.

![24_1](/assets/img/Bandit/24_1.png){: .mx-auto.d-block :}

Ta sẽ tạo một thư mục `\tmp\bandit`, thực hiện viết một đoạn code nhỏ thực hiện bruteforce hết 10000 trường hợp có thể xảy ra lưu vào file `wordlist.txt`. Lúc này `wordlist.txt` gồm 10000 dòng, với nội dung của mỗi dòng có format là: `password_bandit24 pincode`.

![24_2](/assets/img/Bandit/24_2.png){: .mx-auto.d-block :}

Thử truyền dòng đầu tiên đến service tại port **30002** thì thấy nó trả về dòng `Wrong! Please enter the correct pincode. Try again.` 

![24_3](/assets/img/Bandit/24_3.png){: .mx-auto.d-block :}

Lần này ta truyền hết nội dung của `wordlist.txt`, ghi kết quả trả về vào `out.txt`.

![24_4](/assets/img/Bandit/24_4.png){: .mx-auto.d-block :}

Bây giờ ta chỉ việc `cat output.txt` để xem password thoii.
```console
cat out.txt | grep "Wrong! Please enter the correct pincode. Try again."
```

![24_5](/assets/img/Bandit/24_5.png){: .mx-auto.d-block :}
> Ta thu được password cho bandit25 : `uNG9O58gUE7snukf3bvZ0rxhtnjzSGzG`

## Level 25 → 26
---
***Description***

Logging in to **bandit26** from **bandit25** should be fairly easy… The shell for user bandit26 is not `/bin/bash`, but something else. Find out what it is, how it works and how to break out of it.

---
***Background***

- `/etc/passwd/`: Đây là file chứa các thông tin về các user trong hệ thống, có format như sau: **User name** : **Encrypted password** : **User ID number (UID)** : **User's group ID number (GID)**: **Full name of the user** : **User home directory** : **Login shell**. 
- `more` : Cho phép hiển thị text trên một màn hình terminal tại một thời điểm. Một điều thú vị khi sử dụng `more` đó là nếu kích thước terminal nhỏ thì nó sẽ không hiển thị hết nội dung file text mà đợi chúng ta thao tác để tiếp tục hiển thị những phần còn lại. Bên cạnh đó, trong khi sử dụng `more` ta có thể thực thi các command dựa trên một text editor khi nhấn **v** (mặc định là **vim**).

---
***Writeup***

Sau khi login vào user bandit25, ta thực thi `ls` thì xuất hiện file `bandit26.sshkey` là **private key** để truy cập ssh đến **bandit26**.

![25_1](/assets/img/Bandit/25_1.png){: .mx-auto.d-block :}

Thực hiện kết nối **ssh** đến **bandit26** bằng option `-i`, thì sau khi kết nối thành công thì ngay lập tức bị logout ra ngoài. 

```console
bandit25@bandit:~$ ssh -i bandit26.sshkey bandit26@localhost
```

![25_2](/assets/img/Bandit/25_2.png){: .mx-auto.d-block :}

Ta đọc **Description** có thể hiểu **login shell** của bandit26 không phải là `/bin/bash`. Khi đó ta sẽ kiểm tra login shell của bandit26 là gì ở file `\etc\passwd` như đã đề cập ở phần **Background**.

![25_3](/assets/img/Bandit/25_3.png){: .mx-auto.d-block :}

Kết quả có thể thấy login shell của bandit26 là `usr/bin/showtext`. Thực hiện đọc file này thì nó thực hiện lệnh `more` file `~/tetx.txt` trên terminal của linux. Ngay sau khi lệnh more được thực thi xong nó sẽ `exit 0`. Đây là lí do mình bị logout lúc nãy.

Tuy nhiên, sử dụng trick về `more` đã nhắc ở **Background**, ta sẽ thu nhỏ size của terminal để lệnh `more` chưa thực thi xong (hình dưới), tránh bị `exit` ngay. 
 
```console
bandit25@bandit:~$ ssh -i bandit26.sshkey bandit26@localhost
```

![25_4](/assets/img/Bandit/25_4.png){: .mx-auto.d-block :}

Lúc này, ta có thể nhấn phím **v** để truy cập editor vim. Tại đây, ta có thể thực thi lệnh, lấy được bash shell bằng cách set biến môi trường **shell** như các lệnh dưới:

```console
v
:set shell=/bin/sh
:shell
```

Như vậy ta đã truy cập shell thành công dưới quyền thực thi của user **bandit26**. Lúc này ta thực thi các lệnh như trên shell thông thường. Thực hiện xem password cho **bandit26**:
```console  
cat /etc/bandit_pass/bandit26
```

![25_5](/assets/img/Bandit/25_5.png){: .mx-auto.d-block :}


> Ta thu được password cho bandit26 : `5czgV9L3Xx8JPOyRbXh6lQbmIOWvPT6Z`

## Level 26 → 27
---

***Description***

Good job getting a shell! Now hurry and grab the password for **bandit27**!

---

***Writeup***

Sau khi đã truy cập được shell trong **bandit26**, ta có thể thấy có 1 file `bandit27-do` có quyền thực thi các lệnh dưới quyền của user **bandit27** (tương tự **Level 19 → 20**). Giờ ta chỉ cần xem password của **bandit27** bằng lệnh:

```console
$ ./bandit27-do cat /etc/bandit_pas/bandit27
```

![26](/assets/img/Bandit/26.png){: .mx-auto.d-block :}


> Ta thu được password cho bandit27 : `3ba3118a22e93127a4ed485be72ef5ea`


## Level 27 → 28

---

`git clone <url>`: tải repository về từ url

---

- Download repository từ ssh://bandit27-git@localhost/home/bandit27-git/repo

![27_1](/assets/img/Bandit/27_1.png){: .mx-auto.d-block :}

- Dùng lệnh `cat` để đọc file README.md trong đường dẫn repo

![27_2](/assets/img/Bandit/27_2.png){: .mx-auto.d-block :}

> Ta thu được password cho bandit28 : `0ef186ac70e04ea33b4c1853d2526fa2`

## Level 28 → 29

---

`git clone <url>`: tải repository về từ url

`git log` : xem lại thông tin lịch sử commit

`git show <commit id>`: xem thông tin chi tiết về một commit

---

- Download repository từ ssh://bandit28-git@localhost/home/bandit28-git/repo

![28_1](/assets/img/Bandit/28_1.png){: .mx-auto.d-block :}

- Dùng lệnh `cat` để đọc file README.md trong đường dẫn repo

![28_2](/assets/img/Bandit/28_2.png){: .mx-auto.d-block :}

> Nội dung của file README.md gợi ý cho chúng ta một số thông tin về password nhưng không hữu dụng và cũng không có thư mục nào khác. Nên chúng ta sẽ check lần lượt các commit, tag, nhánh khác và cuối cùng là check các file config của từng nhánh

- Dùng lệnh `git log` để kiểm tra các thông tin đã commit

![28_3](/assets/img/Bandit/28_3.png){: .mx-auto.d-block :}

> Từ comment của các phiên bản commit ta có thể thấy được các từ khóa quan trọng như **fix info leak**. Chúng ta nên kiểm tra chi tiết từng phiên bản commit

- Dùng lệnh `git commit <commit id>` để kiểm tra các commit

![28_4](/assets/img/Bandit/28_4.png){: .mx-auto.d-block :}

---

> Ta thu được password cho bandit29 : `bbc96594b4e001778eee9975372716b2`

---

## Level 29 → 30

---

`git branch -a`: xem toàn bộ các nhánh của repository

`git checkout <branch_name>`: chuyển đến nhánh khác trong repository

---

- Download Repository từ ssh://bandit29-git@localhost/home/bandit29-git/repo

![29_1](/assets/img/Bandit/29_1.png){: .mx-auto.d-block :}

- Dùng lệnh `cat` để đọc file README.md trong đường dẫn repo

![29_2](/assets/img/Bandit/29_2.png){: .mx-auto.d-block :}

> Nội dung của file README.md có thể gợi ý cho chúng ta biết là password cho user bandit30 không được khởi tạo ở đây. Do trong repo chỉ có file README.md và một số file config được ẩn đi nên chúng ta có thể hiểu là mật khẩu đã được khởi tạo ở một nhánh khác

- Dùng lệnh `git branch -a` để xem toàn bộ nhánh của repository

![29_3](/assets/img/Bandit/29_3.png){: .mx-auto.d-block :}

> Ta thấy được ngoài nhánh master được mặc định tải về còn có các nhánh khác. Ta sẽ tiếp tục chuyển sang các nhánh đó để tìm kiếm thêm thông tin

- Dùng lệnh `git checkout <branch_name>` để kiểm tra từng nhánh

![29_4](/assets/img/Bandit/29_4.png){: .mx-auto.d-block :}

---

> Ta thu được password cho bandit30 : `5b90576bedb2cc04c86a9e924ce42faf`

---

## Level 30 → 31

---

`git tag -l` or `git tag`: xem danh sách các tag đã được tạo

`git show <tag_name>`: hiện thông tin chi tiết về tag

---

- Dowload Repository từ ssh://bandit30-git@localhost/home/bandit30-git/repo

![30_1](/assets/img/Bandit/30_1.png){: .mx-auto.d-block :}

- Dùng lệnh `cat` để đọc file README.md trong đường dẫn repo

![30_2](/assets/img/Bandit/30_2.png){: .mx-auto.d-block :}

> Lần này README.md không cho chúng ta bất cứ thông tin nào về password và cũng không có thư mục nào khác. Nên chúng ta lần lượt check các commit, tag, các nhánh khác và cuối cùng là check các file config của từng nhánh

- Dùng lệnh `git tag -l` để hiện các tag

![30_3](/assets/img/Bandit/30_3.png){: .mx-auto.d-block :}

- Dùng lệnh `git show <tag_name>` để hiện thông tin chi tiết về tag

![30_4](/assets/img/Bandit/30_4.png){: .mx-auto.d-block :}

> Ta thu được password cho bandit31: `56a9bf19c63d650ce78e6ec0354ee45e`

## Level 31 → 32

---

`git add f <file_name>` : đánh chỉ mục cho file đã được tạo trong thư mục

`git commit -m <message>`: commit và tạo comment cho commit đó

`git push` : đẩy những thay đổi lên repository

---

- Dowload Repository từ ssh://bandit31-git@localhost/home/bandit30-git/repo

![31_1](/assets/img/Bandit/31_1.png){: .mx-auto.d-block :}

- Dùng lệnh `cat` để đọc file README.md trong đường dẫn repo

![31_2](/assets/img/Bandit/31_2.png){: .mx-auto.d-block :}

> Lần này tác giả yêu câu chúng ta file tạo và đẩy file đó lên repository

---

> **Quy trình thêm một file vào repository:** tạo file trong thư mục → tạo chỉ mục add cho file → commit → đẩy những thay đổi lên repository

---

- Dùng lệnh `echo "May I come in?" > key.txt` để tạo file key.txt

![31_3](/assets/img/Bandit/31_3.png){: .mx-auto.d-block :}

- Dùng lệnh `git add -f <file_name>` và `git commit -m <message>` để đánh chỉ mục cho file key.txt sau đó commit

![31_4](/assets/img/Bandit/31_4.png){: .mx-auto.d-block :}

- Dùng lệnh `git push` để lưu những thay đổi lên repository

![31_5](/assets/img/Bandit/31_5.png){: .mx-auto.d-block :}

> Ta thu được password cho bandit32: 56a9bf19c63d650ce78e6ec0354ee45e

## Level 32 → 33

- Đang nhập vào shell và thực hiện một số lệnh để test

![32_1](/assets/img/Bandit/32_1.png){: .mx-auto.d-block :}

> Ta nhận thấy các lệnh sẽ được thực thi trong [Bourne Shell](https://en.wikipedia.org/wiki/Bourne_shell) (sh) và trước khi được thực thi thì các lệnh sẽ được chuyển thành chữ in hoa.

> Vậy thì đối với các biến môi trường thì sẽ như thế nào?

- Sử dụng các biến môi trường như `$HOME`, `$SHELL`,... để kiểm tra tiếp

![32_2](/assets/img/Bandit/32_2.png){: .mx-auto.d-block :}

> Ta thấy rằng các biến môi trường sau khi được nhập vào sẽ xử lý như một chuỗi ký tự bình thường và sau đó được đưa vào thực hiện thư một lệnh trong Bourne Shell. Do Vậy ta có thể sử dụng $0 để gọi Bourne Shell

> **NOTE:** Ta biết rằng khi biến môi trường `$0` trong các shell sẽ chứa đường dẫn đến shell đang gọi nó

- Sử dụng `$0` để gọi Bourne Shell từ shell đang thực thi

![32_3](/assets/img/Bandit/32_3.png){: .mx-auto.d-block :}

> Ta đã vào được Bourne Shell và chúng ta cũng biết thêm shell mà chúng ta được vào ban đầu

- Sử dụng lệnh `cat` để đọc password từ user bandit33

![32_4](/assets/img/Bandit/32_4.png){: .mx-auto.d-block :}

> Ta thu được password cho bandit33: `c9c3199ddf4121b10cf581a98d51caee`
