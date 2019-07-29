```javascript
//totalcount를 받아서 페이지블락 생성
 // 리턴값으로 pages -> view_pagination 파라미터로 넘겨줘야한다.
function create_pagination(totalCount, perPage, perBlock){
		perPage = perPage !== undefined ? perPage : 10;
		perBlock = perBlock !== undefined ? perBlock : 10;
		var totalPage = totalCount%perPage !== 0 ? parseInt(totalCount/perPage)+1 : parseInt(totalCount/perPage);
		var totalBlock = totalPage%perBlock !== 0 ? parseInt(totalPage/perBlock)+1 : parseInt(totalPage/perBlock);
		var block = [];
		for(var i = 0 ; i < totalBlock ; i++){
			var pageBlock = [];
			var startNum = (i*perBlock)+1;
			var lastNum = (i+1)*perBlock >= totalPage ? totalPage : (i+1)*perBlock;
			for(var j = startNum ; j < lastNum+1; j++){
				pageBlock.push(j);
			}
			block.push(pageBlock);
		}
		return block;  // 리턴값으로 블락
	}
  //만들어진 페이지를 보여주는 함수 -> 현재페이지를 통해 현재블락의 페이지버튼을 보여줌. 
  //next , prev버튼 클릭 시 재호출
  //pages: create_pagination 함수를 통해 만들어진 페이지를 파라미터로 받는다.
  //curPage : 추후 검색과 연동하기 위해 사용. default = 1
  //className : 선택페이지 css적용하기 위한 classname 파라미터
  function view_pagination(pages, curPage, targetName, className){
		if(pages === undefined){
			throw new Error('no pages data.');
		}
		curPage = curPage !== undefined ? curPage : 1;
		className = className !== undefined ? className : 'pageOn';
		var perBlock = pages[0].length;
		var curBlock = curPage%perBlock !== 0 ? parseInt(curPage/perBlock) : parseInt(curPage/perBlock)-1
		var pageTag = '';
		pageTag += '<button class="pageNo prev pageArrow" id="prev">이전</button>';
		for(var i = 0 ; i < pages[curBlock].length; i++){
			if(curPage == pages[curBlock][i]){
				pageTag += '<button class="'+className+' pageNo" id='+pages[curBlock][i]+'>'+pages[curBlock][i]+'</button>';
			}else{
				pageTag += '<button class="pageNo" id='+pages[curBlock][i]+'>'+pages[curBlock][i]+'</button>';
			}
		}
		pageTag += '<button class="pageNo next pageArrow" id="next">다음</button>';
		$('#'+ targetName).html(pageTag);
	}
//실제 클릭시 동작하는 함수.
//callback 호출을 통해 ajax데이터 호출
function goPage(resultPage, targetName, callback){
	console.log(resultPage);
	$('#'+targetName).on('click', '.pageNo', function(e){
		var $parent = $(this).parent('#'+targetName);
		var pageNo;
		var target;
		if(e.target.id == 'prev'){
			pageNo = parseInt($parent.find('button.pageOn').attr('id'));
			if(pageNo>1){
				
				target = pageNo-1;
				callback(target);
				$parent.find('button').removeClass('pageOn');
				$parent.find('button#'+target).addClass('pageOn')
			}else{
				alert('첫 페이지입니다.')
			}
		}else if(e.target.id == 'next'){
			pageNo = parseInt($parent.find('button.pageOn').attr('id'));	
			var lastPage = resultPage[resultPage.length-1][resultPage[resultPage.length-1].length-1]
			if(pageNo<lastPage){
				view_pagination(resultPage, pageNo+1, targetName);
				target = pageNo+1;
				callback(target);
				$parent.find('button').removeClass('pageOn');
				$parent.find('button#'+target).addClass('pageOn')
			}else{
				alert('마지막 페이지입니다.')
			}
		}else{
			pageNo = parseInt(e.target.id);
			$parent.find('button').removeClass('pageOn');
			$(this).addClass('pageOn');
			callback(pageNo);
		}
	})
}

```

