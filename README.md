## Front-end-Interview-questions
#### 收集一些自己平时遇到的一些算法题或者面试题，不定期添加。
##### 如有错误，欢迎指正。
## 数组去重的几种方法
### 双循环去重
```
    function unique1(arr){
        for(var i = 0; i < arr.length - 1; i++){
            for(var j = i + 1; j < arr.length; j++){
            //拿数组的每一项和它后边的所有项进行比较， 如果有重复的就把重复的那项给删除了
                if(arr[i] === arr[j]){
                    arr[j] = arr[arr.length - 1];  //把数组的重复那项替换为数组的最后一项
                    arr.length--; // 删除数组的最后一项
                    j--;  //因为把最后一项放到j位置上了 所以需要重新再判断一次
                }
            }
        }
        return arr;
    }
```
### sort()去重法
```
    function unique2(arr){
        arr.sort(function(a, b){
            return a - b;
        });
        // 运用数组自带的sort()方法先从小到大按顺序排好
        for(var i = 0; i < arr.length - 1; i++){
            if(arr[i] === arr[i + 1]){  //把数组当前项和它下一项进行比较，如果相同就删除其中一个
                arr.splice(i + 1, 1)
                i--;
            }
        }
        return arr;
    }
```
### indexOf去重
```
    function unique3(arr){
        var ary = [];  //创建一个新数组来实现数组去重
        for(var i = 0; i < arr.length; i++){
            if(ary.indexOf(arr[i]) == -1){  //查看新数组中是否有当前项，如果没有就放入新数组
                ary.push(arr[i])
            }
        }
        return ary;
    }
```
### 对象法去重
```
    function unique4(arr){
        var obj = {};//创建一个对象 利用对象的属性名不重复的特性来实现数组去重。
        for(var i = 0; i < arr.length; i++){
            var cur = arr[i];
            if(obj[cur]){  //如果对象中有这个以数字为属性名的属性，就删除当前数组中的那一项。
                arr[i] = arr[arr.length - 1];
                arr.length--;
                i--;
                continue;  //结束本次循环，i的值继续累加进行下一次循环。
            }
            obj[cur] = true;
        }
        return arr;
    }
```
### 下标判断法
```
    function unique5(arr){
        for(var i = 0; i < arr.length; i++){
            if(arr.indexOf(arr[i]) !== i){  //利用数组的下标判断，如果数组当前项的索引值不是它的下标，说明重复了。
                arr.splice(i, 1);
                i--;
            }
        }
        return arr;
    }
```
## 几种常见的排序
### 冒泡排序

    把数组中相邻两项依次相互比较，如果a>b，a和b就颠倒位置，拿a跟a后边的那项继续比较。
    如果a<b，就拿b跟b后边的那项继续比较，每一轮都会排序好一个数。
         如[4,3,2,1]  length = 4
         第一轮 i=0
         第一次 比较4和3，4>3 => [3,4,2,1];
         第二次 比较4和2，4>2 => [3,2,4,1];
         第三次 比较4和1，4>1 => [3,2,1,4];
         第一轮 比较3次 每轮比较length-1-0  并且把数组中最大的项放到末尾了
         第二轮 i=1
         第一次 比较3和2，3>2 => [2,3,1,4];
         第二次 比较3和1，3>1 => [2,1,3,4];
         第二轮 比较2次 每轮比较length-1-1  把数组中第二大的项放到倒数第二位
         第三轮 i=2
         第一次 比较2和1，2>1 => [1,2,3,4]
         第三轮 比较1次 每轮比较length-1-2 把数组中第三大的项放到倒数第三位

        数组长度为length的话，一共比较length-1轮，每轮比较length-1-i次

        对于数组中两项颠倒位置的方法
        1) 用第三方变量的方法
        a>b a和b颠倒位置
        tmp = a
        a = b
        b = tmp
        2)不用第三方变量的方法
        a>b a和b颠倒位置
        a = a+b
        b = a-b
        a = a-b
```
    function bubbleSort(arr){
        var flag = false; //设置一个开关
        for(var i = 0; i < arr.length - 1; i++){  //一共比较的轮数
            for(var j = 0; j < arr.length - 1 - i; j++){ //每轮比较的次数
                if(arr[j] > arr[j + 1]){
                    arr[j] = arr[j] + arr[j + 1];
                    arr[j + 1] = arr[j] - arr[j + 1];
                    arr[j] = arr[j] - arr[j + 1];
                    flag = true
                }
            }
            /*
             如果当前这轮已经把整个数组排序好了的话，就直接结束循环，不再进行下一轮排序了。
             如[2,1,3,4]这种只需要一轮就可以排序好，就没必要再进行好几轮了。
             */
            if(flag){
                flag = false;
            } else {
                break;
            }
        }
        return arr;
    }
```
### 插入排序
把数组中的每一项和新数组中的每一项相比较，如果比新数组中的当前项大就放到当前项后边，否则就接着跟下一项比较，如果比新数组的每一项都小的话，就直接放到新数组的开头
```
   function insertSort(arr){
        var newArr = []; //存放排序好的数组
        newArr.push(arr[0]); //把数组的第一项拿来当作新数组初始的对照比较项
        for(var i = 1; i < arr.length; i++){
            var cur = arr[i]; // 数组中的当前项
            for(var j = newArr.length - 1; j >= 0; j--){
                if(cur < newArr[j]){  // cur如果比新数组中的当前项小的话 就继续跟下一项比较
                    if(j == 0){  //如果cur比新数组最小的项还小，就直接把cur放到新数组开头
                        newArr.unshift(cur);
                    }
                } else {
                    newArr.splice(j + 1, 0, cur);//如果cur比新数组当前项大的话就放到当前项后边。
                    break; // 如果cur已经放好位置了就没必要再继续比较了。
                }
            }
        }
        return newArr;
    }
```
### 快速排序
以数组中的中间项为基准点，数组中的每一项跟它比较，小的放到左边，大的放到右边。把拆开的数组继续用这种思想再拆，直到拆的左边或者右边的部分只剩下1项或0项为止。最后把所有拆分的数组拼接在一起
```
    function quickSort(arr){
        if(arr.length <= 1) return arr;  //设置一个停止条件
        var left = [];
        var right = [];
        var middleIndex = Math.floor(arr.length / 2);  //中间项的索引
        var middleItem = arr.splice(middleIndex, 1)[0];//中间项
        for(var i = 0; i < arr.length; i++){
            arr[i] < middleItem ? left.push(arr[i]) : right.push(arr[i]); //拆分数组，小的放左边，大的放右边
        }
        return quickSort(left).concat(middleItem, quickSort(right));
        // 不断调用自身，直到拆的左边或者右边的部分只剩下1项或0项为止。 最后把所有拆分的数组拼接在一起
    }
```
### 求出现次数最多的字母,并求出出现的次数(不区分大小写)
```
    //使用正则的方法
    var str = 'bbbbbaaaacccDDDAAABBCCCBBAacbeeeEE';
    str = str.toLowerCase().split('').sort(function(a, b){
        return a.localeCompare(b);
    }).join('');

    var max = 0;
    var maxStr = ''
    var strTmp = str.replace(/(\w)\1+/g, function($0, $1){
        var length = $0.length;
        if(length > max){
            max = length;
            maxStr = $1
        } else if(length == max){
            maxStr += $1;
        }
    })
    console.log('出现最多的字母是'+maxStr+'，一共出现了'+max+'次')
```
```
    //使用对象的方法
    var str = 'bbbbaaaacccDDDAAABBCCCBBAacbeeeEEggggggggg';
    str = str.toLowerCase().split('').join('');
    var obj = {};
    var max = 0;
    var maxStr = '';
    for(var i = 0; i < str.length; i++){
        var cur = str[i];
        if(obj[cur]){
            obj[cur]++;
        } else {
            obj[cur] = 1;
        }
    }
    for(var attr in obj){
        if(obj.hasOwnProperty(attr)){
           if(max<obj[attr]){
               max = obj[attr]
               maxStr = attr;
           }else if(max == obj[attr]){
               maxStr +=attr;
           }
        }
    }
    console.log('出现最多的字母是'+maxStr+'，一共出现了'+max+'次')
```
### continue和break
```
    for(var i = 0; i < 10; i++){
        if(i < 6){
            i += 3;
            continue;
            console.log(i);
        }
        i += 2;
        break;
        console.log(i)
    }
    console.log(i)  // 10
```

> 这题主要是对break和continue的考察
>     在循环体中遇到break和continue，循环体后边的代码都不再执行，但二者的区别是:
break是跳出整个循环，循环完全结束。
>     continue是本次循环结束，后边的代码不再执行，但是会继续执行累加，继续进行下一次的循环。
>     这题的i的值变化过程：i=0 -> i=3 -> i=4 -> i=7 -> i=8 -> i=10
>     所以最后控制台打印一次 结果是10;

### 作用域，this,闭包相关的题
```
    var num = 10;
    var obj = {
        num : 20,
        fn : (function(num){
            this.num *= 2;
            num += 10;
            return function(){
                this.num *= 3;
                num += 1;
                console.log(num);
            }
        })(num)
    };
    var fn = obj.fn;
    fn();
    obj.fn();
    console.log(window.num, obj.num);
```
如图所示(作用域1.jpg)：
![作用域](http://cm0818.com/tc/%E4%BD%9C%E7%94%A8%E5%9F%9F1.jpg)

> 这题主要是考作用域,this，闭包等知识点。 主要注意以下几点就可以了：
>  1. 自执行函数中的this指向window
>  2. obj.fn的this指向obj
>  3. obj.fn 和 fn都指向同一个内存地址
>  4. 自执行函数的返回值被外部占用，内存不施放。
>  5. fn()和obj.fn()中num+=1都是修改的自执行函数中的num值(相当于局部变量)


### 从浏览器地址栏输入url到显示页面的步骤(以HTTP为例)
1. 在浏览器地址栏输入URL
2. 浏览器查看**缓存**，如果请求资源在缓存中并且新鲜，跳转到转码步骤
    1. 如果资源未缓存，发起新请求
    2. 如果已缓存，检验是否足够新鲜，足够新鲜直接提供给客户端，否则与服务器进行验证。
    3. 检验新鲜通常有两个HTTP头进行控制`Expires`和`Cache-Control`：
        - HTTP1.0提供Expires，值为一个绝对时间表示缓存新鲜日期
        - HTTP1.1增加了Cache-Control: max-age=,值为以秒为单位的最大新鲜时间
3. 浏览器**解析URL**获取协议，主机，端口，path
4. 浏览器**组装一个HTTP（GET）请求报文**
5. 浏览器**获取主机ip地址**，过程如下：
    1. 浏览器缓存
    2. 本机缓存
    3. hosts文件
    4. 路由器缓存
    5. ISP DNS缓存
    6. DNS递归查询（可能存在负载均衡导致每次IP不一样）
6. **打开一个socket与目标IP地址，端口建立TCP链接**，三次握手如下：
    1. 客户端发送一个TCP的**SYN=1，Seq=X**的包到服务器端口
    2. 服务器发回**SYN=1， ACK=X+1， Seq=Y**的响应包
    3. 客户端发送**ACK=Y+1， Seq=Z**
7. TCP链接建立后**发送HTTP请求**
8. 服务器接受请求并解析，将请求转发到服务程序，如虚拟主机使用HTTP Host头部判断请求的服务程序
9. 服务器检查**HTTP请求头是否包含缓存验证信息**如果验证缓存新鲜，返回**304**等对应状态码
10. 处理程序读取完整请求并准备HTTP响应，可能需要查询数据库等操作
11. 服务器将**响应报文通过TCP连接发送回浏览器**
12. 浏览器接收HTTP响应，然后根据情况选择**关闭TCP连接或者保留重用，关闭TCP连接的四次握手如下**：
    1. 主动方发送**Fin=1， Ack=Z， Seq= X**报文
    2. 被动方发送**ACK=X+1， Seq=Z**报文
    3. 被动方发送**Fin=1， ACK=X， Seq=Y**报文
    4. 主动方发送**ACK=Y， Seq=X**报文
13. 浏览器检查响应状态吗：是否为1XX，3XX， 4XX， 5XX，这些情况处理与2XX不同
14. 如果资源可缓存，**进行缓存**
15. 对响应进行**解码**（例如gzip压缩）
16. 根据资源类型决定如何处理（假设资源为HTML文档）
17. **解析HTML文档，构件DOM树，下载资源，构造CSSOM树，执行js脚本**，这些操作没有严格的先后顺序，以下分别解释
18. **构建DOM树**：
    1. **Tokenizing**：根据HTML规范将字符流解析为标记
    2. **Lexing**：词法分析将标记转换为对象并定义属性和规则
    3. **DOM construction**：根据HTML标记关系将对象组成DOM树
19. 解析过程中遇到图片、样式表、js文件，**启动下载**
20. 构建**CSSOM树**：
    1. **Tokenizing**：字符流转换为标记流
    2. **Node**：根据标记创建节点
    3. **CSSOM**：节点创建CSSOM树
21. **[根据DOM树和CSSOM树构建渲染树](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-tree-construction)**:
    1. 从DOM树的根节点遍历所有**可见节点**，不可见节点包括：1）`script`,`meta`这样本身不可见的标签。2)被css隐藏的节点，如`display: none`
    2. 对每一个可见节点，找到恰当的CSSOM规则并应用
    3. 发布可视节点的内容和计算样式
22. **js解析如下**：
    1. 浏览器创建Document对象并解析HTML，将解析到的元素和文本节点添加到文档中，此时**document.readystate为loading**
    2. HTML解析器遇到**没有async和defer的script时**，将他们添加到文档中，然后执行行内或外部脚本。这些脚本会同步执行，并且在脚本下载和执行时解析器会暂停。这样就可以用document.write()把文本插入到输入流中。**同步脚本经常简单定义函数和注册事件处理程序，他们可以遍历和操作script和他们之前的文档内容**
    3. 当解析器遇到设置了**async**属性的script时，开始下载脚本并继续解析文档。脚本会在它**下载完成后尽快执行**，但是**解析器不会停下来等它下载**。异步脚本**禁止使用document.write()**，它们可以访问自己script和之前的文档元素
    4. 当文档完成解析，document.readState变成interactive
    5. 所有**defer**脚本会**按照在文档出现的顺序执行**，延迟脚本**能访问完整文档树**，禁止使用document.write()
    6. 浏览器**在Document对象上触发DOMContentLoaded事件**
    7. 此时文档完全解析完成，浏览器可能还在等待如图片等内容加载，等这些**内容完成载入并且所有异步脚本完成载入和执行**，document.readState变为complete,window触发load事件
23. **显示页面**（HTML解析过程中会逐步显示页面）

--end--
