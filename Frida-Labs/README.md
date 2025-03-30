# Frida-Labs
## 0x1
```
console.log("start Frida");
setTimeout(function () {
    function solve() {
        Java.perform(function () {
            var hook = Java.use("com.ad2001.frida0x1.MainActivity");
            hook.check.overload("int", "int").implementation = function (a, b) {
                this.check(4, 12);
            }
            console.log("Complete");
        });
    }
    solve();
}, 1000);


//FRIDA{BABY_HOOK_0x1}
```
## 0x2
```
console.log("starting");
setTimeout(function () {
    function solve() {
        Java.perform(function () {
            var hook = Java.use("com.ad2001.frida0x2.MainActivity")
            hook.get_flag(4919);
        });
    }
    solve();
}, 1000);


//FRIDA{BABY_HOOK_0x2}
```
## 0x3
```
console.log("starting");
setTimeout(function () {
    function solve() {
        Java.perform(function () {
            var hook = Java.use("com.ad2001.frida0x3.Checker")
            // hook.code.value = 512;
            for (var i = 1; i <= 256; i++) {
                console.log("Code value:" + hook.code.value);
                hook.increase();
            }
        });
    }
    solve();
}, 1000);


//FRIDA{INJECT_AND_INSPECT}
```
## 0x4
```
console.log("starting");
setTimeout(function () {
    function solve() {
        Java.perform(function () {
            var hook = Java.use("com.ad2001.frida0x4.Check");
            //Boi vi o day class khong duoc khai bao duoi dang static, nen ta se phai khoi tao mot obj cho no
            var hook_obj = hook.$new();
            //sau khi khai bao xong chung ta goi ham nhu binh thuong la duoc
            //hay de y ki cach class duoc dinh nghia truoc khi tien hanh hook
            console.log(hook_obj.get_flag(1337));
        });
    }
    solve();
}, 1000);

//FRIDA{XORED_INSTANCE}
```
## 0x5
```
console.log("Starting");
setTimeout(function () {
    function solve() {
        Java.performNow(function () {
            // trong bai nay, doi tuong da duoc khoi tao nen dung choose de tim va su dung thi nhanh hon
            Java.choose('com.ad2001.frida0x5.MainActivity', {
                onMatch: function (instance) {
                    console.log("Instance found");
                    instance.flag(1337);
                },
                onComplete: function () { }
            });
        });
    }
    solve();
}, 1000);

//FRIDA{ON_MATCH_THIS_INSTANCE}

// O day chung ta se phan biet su khac nhau o cach su dung va khi nao su dung Choose and Use
// Java.use
//Mục đích: Java.use được sử dụng để truy cập vào lớp Java tĩnh(static class) hoặc tạo một đối tượng mới của một lớp.
//Sử dụng: Khi bạn muốn thay đổi phương thức hoặc thuộc tính tĩnh, hoặc tạo một đối tượng mới và gọi các phương thức của đối tượng đó.   

// Java.choose
//Mục đích: Java.choose được sử dụng để tìm các đối tượng hiện có của một lớp đang chạy trong quá trình ứng dụng.Nó rất hữu ích khi bạn muốn thao tác với các đối tượng hiện có thay vì tạo đối tượng mới
//Sử dụng: Khi bạn muốn tìm và thao tác với các đối tượng hiện có mà có thể đã được khởi tạo và có trạng thái cụ thể.\
```
## 0x6
```
console.log("Starting");
setTimeout(function () {
    function solve() {
        Java.performNow(function () {
            Java.choose('com.ad2001.frida0x6.MainActivity', {
                onMatch: function (instance) {
                    console.log("Instance found");

                    //gọi khởi tạo một obj mới trong CHECKer và gọi nó ở get_glag bằng choose
                    var hook = Java.use('com.ad2001.frida0x6.Checker');
                    var Check_obj = hook.$new();
                    Check_obj.num1.value = 1234;
                    Check_obj.num2.value = 4321;
                    instance.get_flag(Check_obj);
                },
                onComplete: function () { }
            });
        });
    }
    solve();
}, 1000);

//FRIDA{Instances_AND_Instances}
```
## 0x7
```
console.log("Starting");
setTimeout(function () {
    function solve() {
        Java.performNow(function () {
            Java.choose('com.ad2001.frida0x7.MainActivity', {
                onMatch: function (instance) {
                    console.log("Instance found");

                    //src đã có hàm để set dử liệu của checker nên chúng ta sẽ tận dụng nó để mỗi lần khởi tạo có thể set giá trị tùy ý 
                    var hook = Java.use('com.ad2001.frida0x7.Checker');
                    var Check_obj = hook.$new(513, 513);
                    instance.flag(Check_obj);
                },
                onComplete: function () { }
            });
        });
    }
    solve();
}, 1000);

//FRIDA{HOOKING_CONSTRUCTORS}
```
## 0x8
```
console.log("Starting");
setTimeout(function () {
    function solve() {
        Java.performNow(function () {
            var strcmp_adr = Module.findExportByName("libfrida0x8.so", "strcmp");
            Interceptor.attach(strcmp_adr, {
                onEnter: function (args) {
                    var arg0 = Memory.readUtf8String(args[0]);
                    if (arg0.includes("Hello")) {

                        console.log("Hookin the strcmp function");
                        console.log(Memory.readUtf8String(args[1]))
                    }
                },
                onLeave: function (retval) {
                }
            });
        });
    }
    solve();
}, 1000);


//FRIDA{ NATIVE_LAND } 
```

## 0x9 
```
console.log("Starting");
setTimeout(function () {
    function solve() {
        Java.performNow(function () {
            var check_flag = Module.enumerateExports("liba0x9.so")[0]['address'];
            //var check_flag = Module.findExportByName("liba0x9.so", "check_1flag");
            console.log(check_flag);
            Interceptor.attach(check_flag, {
                onEnter: function (args) {
                },
                onLeave: function (retval) {
                    retval.replace(1337);
                }
            });
        });
    }
    solve();
}, 1000);

//FRIDA{ NATIVE_LAND_0x2}
```

## 0xA
```
console.log("Starting");
setTimeout(function () {
    function solve() {
        Java.performNow(function () {
            //ida tìm được hàm get_flag, nó trả về giá trị của _Z8get_flagii

            let get_flag_address = Module.findExportByName("libfrida0xa.so", "_Z8get_flagii");
            console.log(get_flag_address);
            var get_flag = new NativeFunction(ptr(get_flag_address), "void", ["int", "int"]);
            get_flag(1, 2);
        });
    }
    solve();
}, 1000);

//FRIDA{DONT_CALL_ME}
```

## 0xB
```
console.log("Starting");
setTimeout(function () {
    function solve() {
        Java.performNow(function () {

            var jnz = Module.getBaseAddress("libfrida0xb.so").add(0x0000170ce - 0x00010000);
            Memory.protect(jnz, 0x1000, "rwx");
            var writer = new X86Writer(jnz);

            try {

                writer.putNop()
                writer.putNop()
                writer.putNop()
                writer.putNop()
                writer.putNop()
                writer.putNop()

                writer.flush();

            } finally {

                writer.dispose();
            }
        });
    }
    solve();
}, 1000);

//FRIDA{DONT_CALL_ME}
```
