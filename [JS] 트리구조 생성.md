# 하위코드 호출을 통한 트리구조 생성

```javascript

$(function(){
		function get_child_dept(dept_cd){
				var dept_arr;
				$.ajax({
					url : '/getDept?dept_cd='+dept_cd,
					type : 'get',
					async : false,
					success : function(result){
						dept_arr = result
					},
					error : function(err){

					}				
				})	
				return dept_arr;
			}
		function get_admin_info(){
			var admin_info;
			$.ajax({
				url : '/getAdmin',
				type : 'get',
				async : false,
				dataType : 'json',
				success : function(data){
					admin_info = data;
				},
				error : function(err){

				}
			})
			return admin_info;
		}
		
		var admin_info = get_admin_info();
		$('#my_dept').html('<li><label for="'+admin_info.dept_cd+'" class="plus"/><input type="checkbox" id="'+admin_info.dept_cd+'"/><span id="'+admin_info.dept_cd+'">'+admin_info.dept_nm+'</span></li>');
		$('#my_dept').on('click', 'input', function(e){
			var $parent = $(this).closest('ul');
			if($(this).is(":checked")){
				$(this).prevAll('label').removeClass().addClass('minus');
				var dept_cd = $(this).attr('id');
				var dept_arr = get_child_dept(dept_cd);;
				if(dept_arr.length <= 2){
					alert('하위부대가 없습니다.');
				}else{
					var ul_tag = '<ul>';
					for(var i = 0; i < dept_arr.length; i=i+2){
						ul_tag += '<li><label for="'+dept_arr[i]+'" class="plus"/><input type="checkbox" id="'+dept_arr[i]+'"/><span id="'+dept_arr[i]+'">'+dept_arr[i+1]+'</span></li>';
					}
					ul_tag += '</ul>';
					$(this).parent().after(ul_tag);
				}
			}else{
				$(this).parent().next('ul').remove();
				$(this).prevAll('label').removeClass().addClass('plus');
			}
		})
		$("#my_dept").on('click', 'span', function(){
			var sel_dept_cd = $(this).attr('id');
			var sel_dept_nm = $(this).text();
			if(confirm(sel_dept_nm+'를 선택하시겠습니까?')){
				opener.parent.get_dept(sel_dept_cd);
				window.close();
			}
		})
	})
```



