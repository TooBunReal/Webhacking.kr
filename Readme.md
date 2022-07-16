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

![image](https://user-images.githubusercontent.com/89735990/179356642-3a290016-1dc1-44a2-a86e-12dde9782b32.png)
Mình dùng Edit This Cookie để gán giá trị ```user_lv = 3.1``` và gửi lên server.
Reload lại Web.

![image](https://user-images.githubusercontent.com/89735990/179356722-dfff0db7-64ef-4814-aabe-103d76029796.png)


