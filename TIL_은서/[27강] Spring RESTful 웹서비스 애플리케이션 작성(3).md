# 사용자 정보 조회 및 등록 기능 구현

#### 사용자 목록 조회

##### Ajax 요청

```javascript
function userList() {
		$.ajax({
			url:'users',
			type:'GET',
			contentType:'application/json;charset=utf-8',
			dataType:'json',
			error:function(xhr,status,msg){
				alert("상태값 :" + status + " Http에러메시지 :"+msg);
			},
			success:userListResult
		});
	}
```

##### Ajax 응답

```javascript
function userListResult(xhr) {
		console.log(xhr.data);
		$("tbody").empty();
		$.each(xhr.data,function(idx,item){
			$('<tr>')
			.append($('<td>').html(item.userId))
			.append($('<td>').html(item.name))
			.append($('<td>').html(item.gender))
			.append($('<td>').html(item.city))
			.append($('<td>').html('<button id=\'btnSelect\'>조회</button>'))
			.append($('<td>').html('<button id=\'btnDelete\'>삭제</button>'))
			.append($('<input type=\'hidden\' id=\'hidden_userId\'>').val(item.userId))
			.appendTo('tbody');
		});
	}
```

* data는 controller에서 받아온 데이터값(UserVO객체)
* each() 반복문을 통해 user객체 출력



#### 특정 사용자 조회

##### Ajax 요청

```javascript
function userSelect() {
		//조회 버튼 클릭
		$('body').on('click','#btnSelect',function(){
			var userId = $(this).closest('tr').find('#hidden_userId').val();
			//특정 사용자 조회
			$.ajax({
				url:'users/'+userId,
				type:'GET',
				contentType:'application/json;charset=utf-8',
				dataType:'json',
				error:function(xhr,status,msg){
					alert("상태값 :" + status + " Http에러메시지 :"+msg);
				},
				success:userSelectResult
			});
		});
	}
```

##### Ajax 응답

```javascript
function userSelectResult(xhr) {
		var user = xhr.data;
		$('input:text[name="userId"]').val(user.userId);
		$('input:text[name="name"]').val(user.name);
		$('input:radio[name="gender"][value="'+user.gender+'"]').prop('checked', true);
		$('select[name="city"]').val(user.city).attr("selected", "selected");
	}
```



#### 사용자 등록

##### Ajax 요청

```javascript
function userInsert(){
		//등록 버튼 클릭
		$('#btnInsert').on('click',function(){
			var userId = $('input:text[name="userId"]').val();
			var name = $('input:text[name="name"]').val();
			var gender = $('input:radio[name="gender"]:checked').val();
			var city = $('select[name="city"]').val();		
			$.ajax({ 
			    url: "users",  
			    type: 'POST',  
			    dataType: 'json', 
			    data: JSON.stringify({ userId: userId, name:name,gender: gender, city: city }),
			    contentType: 'application/json', 
			    mimeType: 'application/json',
			    success: function(response) {
			    	if(response.result == true) {
			    		userList();
			    	}
			    }, 
			    error:function(xhr, status, message) { 
			        alert(" status: "+status+" er:"+message);
			    } 
			 });  
		});
	}
```

* `data: JSON.stringify({ userId: userId, name:name,gender: gender, city: city })`

  : `JSON.stringify()`는 폼에 입력된 데이터 값들을 **JSON 포맷으로 변환**해주는 역할

* `if(response.result == true)`: result키값이 true이면 등록이 정상적으로 이루어진 것

  ➡ 사용자 목록 요청 처리하는 userList()요청을 다시 한 번 보냄



#### 특정 사용자 수정

##### Ajax 요청

```javascript
function userUpdate() {
		//수정 버튼 클릭
		$('#btnUpdate').on('click',function(){
			var userId = $('input:text[name="userId"]').val();
			var name = $('input:text[name="name"]').val();
			var gender = $('input:radio[name="gender"]:checked').val();
			var city = $('select[name="city"]').val();	
			$.ajax({ 
			    url: "users", 
			    type: 'PUT', 
			    dataType: 'json', 
			    data: JSON.stringify({ userId: userId, name:name,gender: gender, city: city }),
			    contentType: 'application/json',
			    mimeType: 'application/json',
			    success: function(data) { 
			        userList();
			    },
			    error:function(xhr, status, message) { 
			        alert(" status: "+status+" er:"+message);
			    }
			});
		});
	}
```



#### 특정 사용자 삭제

##### Ajax 요청

```javascript
function userDelete() {
		//삭제 버튼 클릭
		$('body').on('click','#btnDelete',function(){
			var userId = $(this).closest('tr').find('#hidden_userId').val();
			var result = confirm(userId +" 사용자를 정말로 삭제하시겠습니까?");
			if(result) {
				$.ajax({
					url:'users/'+userId,  type:'DELETE',
					contentType:'application/json;charset=utf-8',
					dataType:'json',
					error:function(xhr,status,msg){
						console.log("상태값 :" + status + " Http에러메시지 :"+msg);
					}, success:function(xhr) {
						console.log(xhr.result);
						userList();
					}
				});      }//if
		});
	}
```

* 삭제 버튼은 동적으로 생성되기 때문에 onclick이벤트 처리 방법이 다름

  : body 태그 안에 id가 btnDelete인 버튼에 클릭 이벤트가 발생했을 때 처리

  

