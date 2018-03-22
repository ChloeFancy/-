# Array类型
-------------------
### 1. 检测数组
```javascript
var a = [1,2];
console.log(Array.isArray(a));//true
console.log(a instanceof Array);//true
```
两种检测方法：第一种是ES5新增的方法，第二种就是直接检测是否为Array实例（缺点：有可能有两种以上的Array构造函数）
### 2.转换方法
```javascript
var a = [1,2];
console.log(a.valueOf());//[ 1, 2 ]返回数组本身
console.log(a.toString());//1,2
console.log(a.join("|||"));//1|||2
var b;//undefined
a = [1,b,2];
console.log(a.toString());//1,,2
```
在数组的每一项调用toString()方法后返回组成的字符串,如果是undefined或null为数据项，则返回""空字符串。
### 3. 栈、队列方法
```javascript
var a = [1,2,3,4,5];
a.pop();
a.push(6,7);
a.shift();
console.log(a.unshift(0));//6
console.log(a.toString());//0,2,3,4,6,7
```
这四个方法的返回值是数组变化后的长度，并且会对数组本身进行操作。
### 4. 排序方法
```javascript
function compare(obj1,obj2){
	if(obj1.age>obj2.age){
		return 2;
		//正数——arguments[0]在arguments[1]之后
	}else{
		return -6;
	}
}

var array = [{age:15},{age:13}];
array.sort(compare);
console.log(array);//[ { age: 13 }, { age: 15 } ]
array.reverse(compare);
console.log(array);//[ { age: 15 }, { age: 13 } ]
```
sort、reverse两个方法可以接受一个函数作为参数，并且会对数组本身进行操作。
### 5. 操作方法
```javascript
var arr = [1,2,3,4,5,6];
console.log(arr.concat(7,8,['a','b']));//[ 1, 2, 3, 4, 5, 6, 7, 8, 'a', 'b' ]
console.log(arr.slice(2,5));//[3,4,5]
console.log(arr.splice(0,1));//[2,3,4,5,6]
console.log(arr);//[1,2,3,4,5,6]
console.log(arr.splice(0,2));//[1,2]
console.log(arr);//[3,4,5,6]
```
concat、slice不对原来的数组造成影响，返回值是新的数组。
concat方法可以对数组进行降维。
splice方法返回值是删除的项的数组，会操作原来的数组。
### 6. 位置方法
```javascript
var arr = [1,2,3,4,5,6,5];
console.log(arr.indexOf(2));//1
console.log(arr.lastIndexOf(5));//6
```
这两个方法唯一区别就是查询的方向不同。
### 7. 迭代方法
```javascript
var arr = [1,2,3,4,5,6,5];
console.log(arr.every(function(item,index,array){
    if(item>5){
        return true;
    }
}));//false;
console.log(arr.every(function(item,index,array){}));//false
console.log(arr.some(function(item,index,array){
    if(item>5){
        return true;
    }
}));//true;
console.log(arr.filter(function(item,index,array){
    if(item>5){
        return true;
    }
}));//[6];
arr.forEach(function(item,index,array){array[index]=array[index]+1;}));
console.log(arr);//[2,3,4,5,6,7,6]
console.log(arr.map(function(item,index,array){return 0}));//[0,0,0,0,0,0,0]
```
五个迭代方法中，只有forEach没有返回值。
### 8. 归并方法
```javascript
var arr = [1,2,3,4,5,6];
console.log(arr.reduce(function(pre,cur,index,array){
    return pre+cur;
}));//21
console.log(arr.reduce(function(pre,cur,index,array){
    return pre+cur;
},10));//31
console.log(arr.reduceRight(function(pre,cur,index,array){
    return pre+cur;
}));//21
```
两个方法的不同之处在于归并的方向。
并且以reduce为例，第一次迭代发生在数组的第2项上。

## 方法的应用
### 1. 数组的降维
```javascript
var arr = [1,2,[3,3,3],4,5,6];
function decline(array){
	var result = [];
	var stack = array;
    while(stack.length!=0){
    	var t=stack.pop();
    	if(Array.isArray(t)){
    		stack = stack.concat(t);
    		//这一步重复地减少一维
    	}else{
    		result.push(t);
    	}
    }
    return result;
}
console.log(decline(arr));//[ 6, 5, 4, 3, 3, 3, 2, 1 ]
var arr1 = [1,2,3,[0,9,8,[7,6]]];
console.log(decline(arr1));//[ 6, 7, 8, 9, 0, 3, 2, 1 ]
```
### 2. 数组的去重
```javascript
//一维
var arr = [1,2,2,3,3,4,4,5,5,5,6];
function simplify(arr){
    var result = [];
    arr.forEach(function(item){
        if(result.indexOf(item)<0){
            result.push(item);
        }
    });
    return result;
}
console.log(simplify(arr));//[ 1, 2, 3, 4, 5, 6 ]
////////////////////
Array.prototype.simplify_1 = function(){
	let result = [...new Set(this)];
	return result;
}
////////////////////
Array.prototype.simplify_2 = function(){
	var arr = this.sort();
	var res = [arr[0]];
	for(var i=1;i<arr.length;i++){
		if(arr[i]!==res[res.length-1]){
			res.push(arr[i]);
		}
	}
	return res;
}
////////////////////
Array.prototype.simplify_3 = function(){
	// 缺点：如果数组中有值相同的数字和字符串什么的，就会只留下一个
	var obj={};
	for(var i=0, len=this.length;i<len;i++){
		obj[this[i]]=1;
	}
	var r = Object.keys(obj);
	r.map(function(item, index, array){
		return +item;
	});
	return r;
}

```

```javascript
//二维
var arr = [1,[2,2,3],[3,4,4,5],5,5,6];
function simplify(arr){
    var result = [];
    var find = function(item1){
        if(result.indexOf(item1)<0)
            result.push(item1);
    }
    arr.forEach(function(item){
        if(Array.isArray(item)){
            item.forEach(find);
        }else{
            find(item);
        }
    });
    return result;
}
console.log(simplify(arr));//[ 1, 2, 3, 4, 5, 6 ]
```

```javascript
//n维
var arr = [1,[[[2,2,3],[3,4,4,5],5],5],6];
function simplify(arr){
    var result = [];
    var find = function(item1){
        if(result.indexOf(item1)<0)
            result.push(item1);
    }
    arr.forEach(function(item){
        if(Array.isArray(item)){
            item.forEach(arguments.callee);
        }else{
            find(item);
        }
    });
    return result;
}
console.log(simplify(arr));//[ 1, 2, 3, 4, 5, 6 ]
```
