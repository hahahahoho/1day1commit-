```javascript

//count데이터를 가져와서 callback함수를 통해 변수에 담을수있음
function getCount(urlKey, callback){
	$.ajax({
		async: false,
		type : 'GET',
		url : 'http://localhost:8080/'+urlKey,
		success : function(data){
			callback(data);
		},
		error : function(err){
			console.log(err)
		}
	});
}	
// testJsonObjectArray : json데이터,
// tableName : 타켓명 -> html상의 데이터 타겟 명칭을 인자로 보내준다. table명과 동일하면 구분이 더욱 명확해지기 때문 tableName이라 칭함.
function call_data_object(testJsonObjectArray, tableName){
	var $target = $('[data-target="'+tableName+'"]');
	var $repeat = $($target).find('[data-repeat]');
	$target.empty();
	for(var i = 0 ; i < testJsonObjectArray.length ; i++){
		var $clone = $($repeat[0]).clone();
		Object.keys(testJsonObjectArray[i]).forEach(function(key, index){
			if($clone.find('[data-type="'+key+'"]').length === 1){
				$clone.find('[data-type="'+key+'"]').text(testJsonObjectArray[i][key]);
				$target.append($clone);
			}
		})
		
	};
}
// testJsonObjectArray : array데이터,
// tableName : 타켓명 -> html상의 데이터 타겟 명칭을 인자로 보내준다. table명과 동일하면 구분이 더욱 명확해지기 때문 tableName이라 칭함.
function call_data_array(testJsonArrayArray, tableName){
	var $target = $('[data-target="'+tableName+'"]');
	var $repeat = $($target).find('[data-repeat]');
	$target.empty();
	for(var i = 0 ; i < testJsonArrayArray.length ; i++){
		var $clone = $($repeat[0]).clone();
		for(var j = 0 ; j <testJsonArrayArray[i].length; j++){
			if($clone.find('[data-type="'+key+'"]').length === 1){
				$clone.find('[data-type="'+key+'"]').text(testJsonObjectArray[i][key]);
				$target.append($clone);
			}
		}
		$target.append($clone);
	}
};
//실제 데이터 불러오는 함수
function getData(urlKey, perPage, pageNo, type, targetName){
	perPage = perPage !== undefined ? perPage : 10;
	pageNo = pageNo !== undefined ? pageNo : 1;
	if(type == undefined || targetName == undefined){
		throw new Error('no data params type or targetName.');
	}
	$.ajax({
		type : 'GET',
		url : 'http://localhost:8080/'+urlKey+'/'+perPage+'/'+pageNo,
		dataType : 'json',
		success : function(data){
			if(type === 'object'){
				call_data_object(data, targetName); //json데이터 자동 태그입력
			}else if(type === 'array'){
				call_data_array(data, targetName);
			}else{
				throw new Error('type invalid.');
			}
		},
		error : function(err){
			console.log(err)
		}
	})
}
```

