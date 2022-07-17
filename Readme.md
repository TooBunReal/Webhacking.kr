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
  ```php
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

![image](https://user-images.githubusercontent.com/89735990/179358895-cef3ac17-262a-4505-a13d-cc3758696677.png)'

# old-014
 Link Challenge: ```https://webhacking.kr/challenge/js-1/```
 
 ![image](https://user-images.githubusercontent.com/89735990/179385821-f212fa43-09fb-452a-8a4d-bcad8f5da9d7.png)

 
 Source Code:
 
 ```js
 <form name=pw><input type=text name=input_pwd><input type=button value="check" onclick=ck()></form>
<script>
function ck(){
  var ul=document.URL;
  ul=ul.indexOf(".kr");
  ul=ul*30;
  if(ul==pw.input_pwd.value) { location.href="?"+ul*pw.input_pwd.value; }
  else { alert("Wrong"); }
}
</script>
```
 Hàm ```indexOf(".kr")``` sẽ trả về giá trị của String tính từ đoạn ``kr`` và giá trị của nó trả về độ dài của đoạn ```kr/challenge/js-1/``` là 18. Vây biến ``ul`` lúc này có giá trị là ```18```.
 
 Vậy ``ul*30=18*30=540`` 
 
 ![image](https://user-images.githubusercontent.com/89735990/179386418-2893fea5-d190-46b2-87bb-722b6e792ffb.png)
 
 Submit giá trị trên URL và hoàn thành.
 
 ``https://webhacking.kr/challenge/web-06/?291600``
 
 # old-015
  Link Challenge: ``https://webhacking.kr/challenge/js-2/``
  
  ![image](https://user-images.githubusercontent.com/89735990/179386655-fefa1923-4e40-4ebe-8026-039a24df6875.png)

  Oh no :< có vẻ nhưng trang web này không chào đoán chúng ta rồi.
  Để thử ```window+shift+c``` đọc Source Code xem nào.
  
  ![image](https://user-images.githubusercontent.com/89735990/179386740-92206a2a-8754-4c0e-87ec-1ceacb1dde6d.png)

  Hoàn toàn không có gì để xem luôn :< vậy thì làm sao bây giờ nhỉ.
  
  Qua tìm hiểu thì mình biết được lệnh ``Curl`` của linux. Nó sẽ kết xuất (render) cho bạn mã nguồn của trang chủ website. Nếu không xác định protocol thì Curl sẽ mặc định dịch nó thành HTTP.
  Oke mình sẽ thử với Web này.
  
  ![image](https://user-images.githubusercontent.com/89735990/179386818-24bceab6-0b51-4aa5-aef9-30ca99c4cb5d.png)

  Mình nhận về một được Source của Web rồi. Xem sơ thì mình thấy dòng ``"<a href=?getFlag>[Get Flag]</a>"``
  Việc của mình bây giờ đơn giản là gửi ``?getFlag`` lên url để Hoàn Thành thôi hehe.
  
  ![image](https://user-images.githubusercontent.com/89735990/179386873-a4345606-5a9a-4f46-bdfe-a87ef0bc79fb.png)

  # old-016
  Link Challenge: ```https://webhacking.kr/challenge/js-3/```
  
  ![image](https://user-images.githubusercontent.com/89735990/179389269-86ff8d7e-56c8-4a88-8a29-879456fe9f4e.png)
  
  Source Code:
  
   ```js
   <script> 
document.body.innerHTML+="<font color=yellow id=aa style=position:relative;left:0;top:0>*</font>";
function mv(cd){
  kk(star.style.left-50,star.style.top-50);
  if(cd==100) star.style.left=parseInt(star.style.left+0,10)+50+"px";
  if(cd==97) star.style.left=parseInt(star.style.left+0,10)-50+"px";
  if(cd==119) star.style.top=parseInt(star.style.top+0,10)-50+"px";
  if(cd==115) star.style.top=parseInt(star.style.top+0,10)+50+"px";
  if(cd==124) location.href=String.fromCharCode(cd)+".php"; // do it!
}
function kk(x,y){
  rndc=Math.floor(Math.random()*9000000);
  document.body.innerHTML+="<font color=#"+rndc+" id=aa style=position:relative;left:"+x+";top:"+y+" onmouseover=this.innerHTML=''>*</font>";
}
</script>
```
Trên giao diện web xuất hiện 3 dấu sao có kích thước khác nhau và khi ta click vào thì dấu sao nhỏ nhất biến mất.

Thử mò trên Source thì ta phát hiện một dòng kì lạ là dòng ```if(cd==124) location.href=String.fromCharCode(cd)+".php"; // do it!```

Hmm nó kêu chúng ta thử với dòng này à, Vậy thì chúng ta phải coi xem ``124`` là ký tự nào đã.

![image](https://user-images.githubusercontent.com/89735990/179389432-6aa32def-9579-43bc-b04e-e0a03ad29553.png)

Sau khi tra thì mình biết được rằng ```124``` là ký tự ``|``, Nào giờ thì hãy đi tới đường dẫn ``|.php`` thôi.

![image](https://user-images.githubusercontent.com/89735990/179389478-dd63477d-84b5-49b7-aff6-b2d2268a90d5.png)


