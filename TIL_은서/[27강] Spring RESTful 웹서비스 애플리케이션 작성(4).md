# XML 응답을 주는 RESTful 웹서비스 개요

#### JAXB(Java Architecture for XML Binding)이란?

* Java객체 <-> XML로 변환해주는 API

  Jackson과 유사하지만 Jackson은 외부API라 따로 설치 필요 but JAXB는 *Java SE에 포함되어있기 때문에 따로 설치 필요X*

  **🚨🚨 JAXB API 의존성 추가 필요 + 클래스패스에 추가 필요🚨🚨**

* marshalling(직렬화) : 자바객체 -> XML

  unmarshalling(역직렬화) : XML -> 자바객체



#### 주요 JAXB 어노테이션

* @XmlRootElement
  * XML의 Root Element 이름 정의
  * 클래스에서 사용하는 어노테이션 -> 해당 클래스가 XML Root임을 뜻함
* @XmlElement
  * XML Element 이름을 정의할 때 사용
  * 변수 또는 setter 메서드에 사용하는 어노테이션 -> 해당 변수가 XML Element라는 것을 뜻함



#### JAXB를 사용한 RESTful 웹 서비스 구현 절차

1. Java객체를 XML로 변환하기 위해 @XMLRootElement, @XMLElement 어노테이션을 선언한 UserVOXML클래스 작성
2. RestfulController 클래스에 사용자 목록을 조회하는 getUserListXML() 메서드 작성, @RequestMapping과 @ResponseBody 어노테이션 선언
3. Postman이용하여 메서드 테스트
4. jQuery 기반 Ajax 통신하는 userList_XML.html 작성





# XMl 응답을 주는 RESTful 웹서비스 구현

#### Java 객체를 XML로 변환해주는 클래스 작성



##### UserVOXML.java

```java
@XmlRootElement(name = "users")
public class UserVOXML {
	private String status;
	private List<UserVO> userList;

	public UserVOXML() {}

	public UserVOXML(String status, List<UserVO> userList) {
		this.status = status;
		this.userList = userList;
	}
	@XmlElement
	public void setStatus(String status) { this.status = status; }
	
	@XmlElement(name="user")
	public void setUserList(List<UserVO> userList) { this.userList = userList; }
	
	public String getStatus() { return status; }
	public List<UserVO> getUserList() { return userList; }
}
```



##### 변환된 XML

```xml
<users>
	<status>success</status>
	<user>
		<city>제주</city>
		<gender>남</gender>
		<name>둘리</name>
		<userId>dooli</userId>
	</user>
    ...
</uesrs>
```



##### RestfulUserController.java

```java
@RequestMapping(value="/usersXml",
			method=RequestMethod.GET)
	@ResponseBody
	public UserVOXML getUserListXml() {
		List<UserVO> list = userService.getUserList();
		UserVOXML xml = new UserVOXML("success", list);
		return xml;
	}	
```

