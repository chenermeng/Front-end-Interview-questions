# Front-end-Interview-questions
## 收集一些自己平时遇到的一些算法题或者面试题，不定期添加。
## 数组去重的几种方法
### 双循环去重
```
    function unique1(arr){
        for(var i = 0; i < arr.length - 1; i++){
            for(var j = i + 1; j < arr.length; j++){  //拿数组的每一项和它后边的所有项进行比较， 如果有重复的就把重复的那项给删除了
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