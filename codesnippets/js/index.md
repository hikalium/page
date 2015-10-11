JavaScriptコード断片集
====

<pre class="brush: js;">
Array.prototype.removeAllObject = function(anObject){
	//Array中にある全てのanObjectを削除し、空いた部分は前につめる。
	//戻り値は削除が一回でも実行されたかどうか
	var ret = false;
	for(var i = 0; i < this.length; i++){
		if(this[i] == anObject){
			this.splice(i, 1);
			ret = true;
			i--;
		}
	}
	return ret;
}
Array.prototype.removeAnObject = function(anObject){
	//Array中にある最初のanObjectを削除し、空いた部分は前につめる。
	//戻り値は削除が実行されたかどうか
	for(var i = 0; i < this.length; i++){
		if(this[i] == anObject){
			this.splice(i, 1);
			return true;
		}
	}
	return false;
}
Array.prototype.removeByIndex = function(index, length){
	//Array[index]を削除し、空いた部分は前につめる。
	if(length === undefined){
		length = 1;
	}
	this.splice(index, length);
	return;
}
Array.prototype.insertAtIndex = function(index, data){
	return this.splice(index, 0, data);
}
Array.prototype.symmetricDifferenceWith = function(b, fEqualTo){
	// 対称差(XOR)集合を求める
	// fEqualToは省略可能で、評価関数fEqualTo(a[i], b[j])を設定する。
	var a = this.copy();
	var ei;
	for(var i = 0, len = b.length; i < len; i++){
		ei = a.getIndex(b[i], fEqualTo)
		if(ei != -1){
			a.removeByIndex(ei);
		} else{
			a.push(b[i]);
		}
	}
	return a;
}
Array.prototype.intersectionWith = function(b, fEqualTo){
	//積集合（AND）を求める
	//fEqualToは省略可能で、評価関数fEqualTo(a[i], b[j])を設定する。
	var r = new Array();
	for(var i = 0, len = b.length; i < len; i++){
		if(this.includes(b[i], fEqualTo)){
			r.push(b[i]);
		}
	}
	return r;
}
Array.prototype.unionWith = function(b, fEqualTo){
	//和集合（OR）を求める
	//fEqualToは省略可能で、評価関数fEqualTo(a[i], b[j])を設定する。
	var r = new Array();
	for(var i = 0, len = b.length; i < len; i++){
		if(!this.includes(b[i], fEqualTo)){
			r.push(b[i]);
		}
	}
	return this.concat(r);
}
Array.prototype.isEqualTo = function(b, fEqualTo){
	//retv: false or true.
	//二つの配列が互いに同じ要素を同じ個数だけ持つか調べる。
	//fEqualToは省略可能で、評価関数fEqualTo(a[i], b[i])を設定する。
	//fEqualToが省略された場合、二要素が全く同一のオブジェクトかどうかによって評価される。
	var i, iLen;
	if(!(b instanceof Array) || this.length !== b.length){
		return false;
	}
	iLen = this.length;
	if(fEqualTo == undefined){
		for(i = 0; i < iLen; i++){
			if(this[i] !== b[i]){
				break;
			}
		}
	} else{
		for(i = 0; i < iLen; i++){
			if(fEqualTo(this[i], b[i])){
				break;
			}
		}
	}
	if(i === iLen){
		return true;
	}
	return false;
}
Array.prototype.includes = function(obj, fEqualTo){
	//含まれている場合は配列内のそのオブジェクトを返す
	//fEqualToは省略可能で、評価関数fEqualTo(array[i], obj)を設定する。
	if(fEqualTo == undefined){
		for(var i = 0, len = this.length; i < len; i++){
			if(this[i] == obj){
				return this[i];
			}
		}
	} else{
		for(var i = 0, len = this.length; i < len; i++){
			if(fEqualTo(this[i], obj)){
				return this[i];
			}
		}
	}
	return false;
}
Array.prototype.getIndex = function(obj, fEqualTo){
	// 含まれている場合は配列内におけるそのオブジェクトの添字を返す。
	// 見つからなかった場合、-1を返す。
	//fEqualToは省略可能で、評価関数fEqualTo(array[i], obj)を設定する。
	if(fEqualTo == undefined){
		for(var i = 0, len = this.length; i < len; i++){
			if(this[i] == obj){
				return i;
			}
		}
	} else{
		for(var i = 0, len = this.length; i < len; i++){
			if(fEqualTo(this[i], obj)){
				return i;
			}
		}
	}
	return -1;
}
Array.prototype.getAllMatched = function(obj, fEqualTo){
	// 評価関数が真となる要素をすべて含んだ配列を返す。
	// 返すべき要素がない場合は空配列を返す。
	// fEqualToは省略可能で、評価関数fEqualTo(array[i], obj)を設定する。
	var retArray = new Array();
	if(fEqualTo == undefined){
		for(var i = 0, len = this.length; i < len; i++){
			if(this[i] == obj){
				retArray.push(this[i]);
			}
		}
	} else{
		for(var i = 0, len = this.length; i < len; i++){
			if(fEqualTo(this[i], obj)){
				retArray.push(this[i]);
			}
		}
	}
	return retArray;
}
Array.prototype.last = function(n){
	var n = (n === undefined) ? 1 : n;
	return this[this.length - n];
}
Array.prototype.search2DLineIndex = function(column, obj, fEqualTo){
	//与えられた配列を二次元配列として解釈し
	//array[n][column]がobjと等価になる最初の行nを返す。
	//fEqualToは省略可能で、評価関数fEqualTo(array[n][column], obj)を設定する。
	//該当する行がなかった場合、戻り値はundefinedとなる。
	if(fEqualTo == undefined){
		for(var i = 0, iLen = this.length; i < iLen; i++){
			if(this[i] instanceof Array && this[i][column] == obj){
				return i;
			}
		}
	} else{
		for(var i = 0, iLen = this.length; i < iLen; i++){
			if(this[i] instanceof Array && fEqualTo(this[i][column], obj)){
				return i;
			}
		}
	}
	return undefined;
}
Array.prototype.search2DObject = function(searchColumn, retvColumn, obj, fEqualTo){
	//与えられた配列を二次元配列として解釈し
	//array[n][searchColumn]がobjと等価になる最初の行のオブジェクトarray[n][retvColumn]を返す。
	//fEqualToは省略可能で、評価関数fEqualTo(array[n][searchColumn], obj)を設定する。
	//該当する行がなかった場合、戻り値はundefinedとなる。
	if(fEqualTo == undefined){
		for(var i = 0, iLen = this.length; i < iLen; i++){
			if(this[i] instanceof Array && this[i][searchColumn] == obj){
				return this[i][retvColumn];
			}
		}
	} else{
		for(var i = 0, iLen = this.length; i < iLen; i++){
			if(this[i] instanceof Array && fEqualTo(this[i][searchColumn], obj)){
				return this[i][retvColumn];
			}
		}
	}
	return undefined;
}
Array.prototype.pushUnique = function(obj, fEqualTo){
	//値が既に存在する場合は追加しない。評価関数fEqualTo(array[i], obj)を設定することができる。
	//結果的に配列内にあるオブジェクトが返される。
	var o = this.includes(obj, fEqualTo);
	if(!o){
		this.push(obj);
		return obj;
	}
	return o;
}
Array.prototype.stableSort = function(f){
	// http://blog.livedoor.jp/netomemo/archives/24688861.html
	// Chrome等ではソートが必ずしも安定ではないので、この関数を利用する。
	if(f == undefined){
		f = function(a,b){ return a - b; };
	}
	for(var i = 0; i < this.length; i++){
		this[i].__id__ = i;
	}
	this.sort.call(this, function(a,b){
		var ret = f(a, b);
		if(ret == 0){
			return (a.__id__ > b.__id__) ? 1 : -1;
		} else{
			return ret;
		}
	});
	for(var i = 0;i < this.length;i++){
		delete this[i].__id__;
	}
}
Array.prototype.splitByArray = function(separatorList){
	//Array中の文字列をseparatorList内の文字列でそれぞれで分割し、それらの文字列が含まれた配列を返す。
	var retArray = new Array();
	
	for(var i = 0, iLen = this.length; i < iLen; i++){
		retArray = retArray.concat(this[i].splitByArray(separatorList));
	}
	
	return retArray;
}
Array.prototype.propertiesNamed = function(pName){
	//Array内の各要素のプロパティpNameのリストを返す。
	var retArray = new Array();
	for(var i = 0, iLen = this.length; i < iLen; i++){
		retArray.push(this[i][pName]);
	}
	return retArray;
}
Array.prototype.logAsHexByte = function(logfunc){
	//十六進バイト列としてデバッグ出力する。
	//logfuncは省略時はconsole.logとなる。
	if(logfunc === undefined){
		logfunc = function(s){ console.log(s); };
	}
	var ds = "";
	for(var i = 0, iLen = this.length; i < iLen; i++){
		ds += ("00" + this[i].toString(16).toUpperCase()).slice(-2);
	}
	logfunc(ds);
}
Array.prototype.stringAsHexByte = function(){
	//十六進バイト列として文字列を得る
	var ds = "";
	for(var i = 0, iLen = this.length; i < iLen; i++){
		ds += ("00" + this[i].toString(16).toUpperCase()).slice(-2);
	}
	return ds;
}
Array.prototype.logEachPropertyNamed = function(pname, logfunc, suffix){
	//Arrayのすべての各要素pについて、プロパティp[pname]を文字列としてlogfuncの引数に渡して呼び出す。
	//suffixは各文字列の末尾に追加する文字列。省略時は改行文字となる。
	//logfuncは省略時はconsole.logとなる。
	if(logfunc === undefined){
		logfunc = function(s){ console.log(s); };
	}
	if(suffix === undefined){
		suffix = "\n";
	}
	for(var i = 0, iLen = this.length; i < iLen; i++){
		logfunc(this[i][pname] + suffix);
	}
}

Array.prototype.logEachPropertiesNamed = function(pnames, logfunc,　separator, suffix){
	//Arrayのすべての各要素pについて、プロパティp[pnames[n]]を文字列としてlogfuncの引数に渡して呼び出す。
	//suffixは各文字列の末尾に追加する文字列。省略時は改行文字となる。
	//separatorは各項目の間に置かれる文字列。省略時は",\t"となる。
	//logfuncは省略時はconsole.logとなる。
	if(logfunc === undefined){
		logfunc = function(s){ console.log(s); };
	}
	if(suffix === undefined){
		suffix = "\n";
	}
	if(separator === undefined){
		separator = ",\t";
	}
	var kLen = pnames.length - 1;
	for(var i = 0, iLen = this.length; i < iLen; i++){
		var s = "";
		for(var k = 0; k < kLen; k++){
			s += this[i][pnames[k]] + separator;
		}
		if(kLen != -1){
			s += this[i][pnames[k]] + suffix;
		}
		logfunc(s);
	}
}

Array.prototype.copy = function(){
	return (new Array()).concat(this);
}
</pre>
