# Một Số Tool và Extension mà mình hay dùng để giải bài
  - Burp Suite
  - JWT Cracker
  - SQLMAP
  - Edit This Cookie
  - HTTP Request Maker

# old-01
 Link Challenge: ```https://webhacking.kr/challenge/web-01/```
 
 ![image](https://user-images.githubusercontent.com/89735990/179356191-8e7d373a-9d0f-4664-b80d-39ac1d28c89d.png)
 
 Source Code:
 
 ![image](https://user-images.githubusercontent.com/89735990/179356233-3b36009f-9e2e-4b7a-8600-575543a0d518.png)
 
 Xem qua đoạn code PHP:
  ```
  <?php
  if(!is_numeric($_COOKIE['user_lv'])) $_COOKIE['user_lv']=1;
  if($_COOKIE['user_lv']>=4) $_COOKIE['user_lv']=1;
  if($_COOKIE['user_lv']>3) solve(1);
  echo "<br>level : {$_COOKIE['user_lv']}";
?>
```
Ta thấy có Hàm ```$_COOKIE```. Hàm này sẽ trả về giá trị Cookie của biến truyền vào, cụ thể là ```user_lv```.
Xem xét một hồi thì Hàm này sẽ kiểm tra giá trị của ```user_lv``` có thuộc khoảng ```3 < user_lv <= 4``` hay không.
Nếu đúng thì sẽ cho chạy hàm solve(1).
Nhiệm Vụ của chúng ta đơn giản là gửi một giá trị ```user_lv``` mới đúng với điều kiện của đoạn code:

![image](https://user-images.githubusercontent.com/89735990/179356642-3a290016-1dc1-44a2-a86e-12dde9782b32.png)\

Mình dùng Edit This Cookie để gán giá trị ```user_lv = 3.1``` và gửi lên server.
Reload lại Web.

![image](https://user-images.githubusercontent.com/89735990/179356722-dfff0db7-64ef-4814-aabe-103d76029796.png)

# old-06
 Link Challenge: ```https://webhacking.kr/challenge/web-06/```
 
 ![image](https://user-images.githubusercontent.com/89735990/179357637-25f823fb-39cf-4b87-b0c6-615f7efd0821.png)

 Source Code: ``` https://webhacking.kr/challenge/web-06/?view_source=1 ```

 ![image](https://user-images.githubusercontent.com/89735990/179357685-a7c472c1-8250-4f6c-abe5-82e3f5d2518a.png)
 
 Trong đoạn này ta thấy hai giá trị ```id="guest"``` và ```pw="123qwe"``` được mã hóa bằng base64 20 lần và thay thế các chữ số bằng các kí tự đặt biệt.
 
 ![image](https://user-images.githubusercontent.com/89735990/179357854-40d1fb56-d3d3-4e86-8367-4feeb4855bb8.png)

 Ở đoạn này ta phát hiện được id và pw đúng là ```id="admin"``` và ```pw="nimda"```.
 Bây giờ thì nhiệm vụ của chúng ta chỉ là thực hiện lại các bước như trên rồi gửi id và pw mới lên thông qua cookie.
 + Mã hóa id và pw theo base64 20 lần.
 + Thay thế các số từ 1 tới 8 bằng các kí tự đặt biệt tương ứng.
 + Mã hóa base64 bằng URLencode vì chúng ta sẽ gửi nó thông qua cookie và cookie ở đây ở định dạng URLencode.
 Sau đây là đoạn script Python của mình để giải quyết 3 vấn đề trên :
 ```python
 import base64
import urllib.parse

ID = b"admin"
PW = b"nimda"

#Mã hóa ID và PW 20 lần.
for i in range(20):
        ID = base64.b64encode(ID)
        PW = base64.b64encode(PW)

newID = ID.decode('utf-8')
newPW = PW.decode('utf-8')

#Thay thế các chứ số từ 1 tới 8 bằng các ký tự đặt biệt
newID = newID.replace('1','!').replace('2','@').replace('3','$').replace('4','^').replace('5','&').replace('6','*').replace('7','(').replace('8',')')
newPW = newPW.replace('1','!').replace('2','@').replace('3','$').replace('4','^').replace('5','&').replace('6','*').replace('7','(').replace('8',')')

#Mã hóa base64 thành URLlen 
newID = urllib.parse.quote(newID)
newPW = urllib.parse.quote(PW)
print("ID: ")
print(newID)
print("PW: ")
print(newPW)
```
Chạy Code ta được ID và PW mới

![image](https://user-images.githubusercontent.com/89735990/179358739-af5177a7-2f46-46c6-b9e3-700e75a88fc3.png)

Giờ thì gửi giá trị ID và PW mới lên và reload lại web thôi 

![image](https://user-images.githubusercontent.com/89735990/179358833-50e5b309-fc78-4d15-995e-8fd12865af00.png)

![image](https://user-images.githubusercontent.com/89735990/179358895-cef3ac17-262a-4505-a13d-cc3758696677.png)



