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
            if(arr.indexOf(i) !== i){  //利用数组的下标判断，如果数组当前项的索引值不是它的下标，说明重复了。
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
>     在循环体中遇到break和continue，循环体后边男的代码都不再执行，但二者的区别是:
break是跳出整个循环，真哥哥循环结束。
>     conti是本次循环结束，后边的代码不再执行，但是会继续执行累加，继续进行下一次的循环。
>     这题的i的值变化过程：i=0 -> i=3 -> i=4 -> i=7 -> i=8 -> i=10
>     所以最后再控制台纸打印一次 结果是10;