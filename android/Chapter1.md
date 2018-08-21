안드로이드 앱 프로그래밍 정리
====

##### 본 내용은 안드로이드 스튜디오-> JAVA, Do it 안드로이드 앱프로그래밍 교재를 기준으로 정리되었습니다.

-------------------------------

* mainactivity.java열어보기
    * mainactivity.java를 열어서 자바 파일의 내용을 보면, main함수가 없는 것을 알 수 있다. 그 이유는 안드로이드에서는 main함수가 아닌 onCreate함수가 애플리케이션의 시작점이되기 때문이다.
    * super.onCreate(savedInstanceState); 는 super가 상속을 받은 클래스가 부모 클래스를 가리킬 때 사용되므로, 부모클래스에 있는 동일한 이름의 onCreate함수를 호출한 다는 것을 알 수 있다.
    * 그 이후에 있는 setContentView(R.layout.activity_main);은 activity_main.xml의 내용을 보여주는 역할을 하고, 이는 src의 layout폴더 안에서 찾을 수 있다.

-------------------------------

* xml에 onclick 속성값 넣기

우선 xml 에 button을 넣어보자
```javascript
<Button
	...
	android:onClick="onButton1Clicked"
	android:text="버튼"
    />
    ...
```

이렇게 xml을 구성하고 mainactivity.java로 돌아가 보자.
mainactivity.java에 저 버튼이 눌렸을 때 행할 코드를 넣어야 할 것이다.

```java
....
public void onButton1Clicked(View v){
	Toast.makeText(getApplicationContext(),"버튼이 눌렸습니다.",Toast.LENGTH_LONG).show();
}
.....
```
###### 여기서 toast는 토스트기 처럼 메시지를 불쑥 나오게 하는 방법이다.


>다른 액티비티로 이동하기
다른 액티비티로 이동하려면 우선 다른 activity가 필요하다. 당연히 xml도 필요할 것이고, 
매니패스트에도 추가되어야할 것이다. 자신만의 커스텀된 새로운 xml과 NextActivity을 구성하고, 
위의 코드를 그대로 사용해 보자. 
Toat 대신에 
```java
Intent intent = new Intent(getApplicationContext(), NextActivity.class);
startActivity(intent);
```
를 넣으면 NextActivity로 이동할 수 있을 것이다.


* 안드로이드 프로젝트에 들어있는 폴더들
    * /java --> 자바 소스 파일이 들어있는 폴더
    * /res --> 리소스 파일이 들어있는 폴더로 레이아웃, 사진 그림, 문자열, 스타일 등을 정의한 xml 파일 등이 들어있다
    * /build --> 개발환경이 자동으로 만들어내는 소스파일이 들어가는 폴더

###### * 추가>> 다른 사람의 프로젝트를 안드로이드에서 열 때 소스파일에 에러가 없는데도 빌드가 안되는 경우, gradle script 폴더 안에 있는 build gradle파일의 설정의 차이 일 수 있다. 또한 gradle에는 다양한 버전정보 등도 들어있다.


* 뷰 이해하기
뷰(view)는 일반적으로 컨트롤이나 위젯으로 불리는 구성요소로써 사용자의 눈에 보이는 화면의 구성요소들이 바로 뷰이다. 뷰는 뷰 안에 뷰를 끼고 상속의 형식을 취할 수 있다. 뷰그룹은 여러개의 뷰를 담고있는 또하나의 뷰이고, 담겨져있는 뷰가 또 뷰그룹이 되어 계속 중첩된 형태를 가질 수 있다.
뷰의 예시로, textview는 button을 상속하고있으며, viewGroup은 linearlayout을 상속하는 관계에 있다
* 뷰에서 xmls:android 는 안드로이드 기본 SDK에 포함되어있는 속성을 사용한다. 
* xmls:app은 프로젝트에서 사용하는 외부라이브러리에 포함되어있는 속성을 사용하고, 
* xmls:tools는 안드로이드 스튜디오의 디자이너도구 등에서 화면에 보여줄 때 사용되는 것으로 실제로 앱이 실행 될 때는 적용되지 않고 안드로이드 스튜디오에서만 적용된다


-----------


* 대표적인 레이아웃
    * ConstraintLayout: 제약조건을 사용해 화면을 구성
    * LinearLayout: 박스 모델로써 한쪽방향으로 차례대로 뷰를 추가하며 화면을 구성한다
        * 세로, 가로를 지정할 수 있다
        * layout_gravity: 부모 컨테이너의 여유 공간에 뷰가 모두 채워지지 않아 여유공간이 생겼을 때 여유공간 안에서 뷰를 정렬/여유공간이 없으면 의미 없음
        * gravity: 뷰 안에 표시하는 내용물을 정렬할 때 사용
    * RelativeLayout: 부모 컨테이너나 다른 뷰와의 상대적 위치로 화면을 구성하는 방법
    * FrameLayout: 가장 상위에 있는 하나의 뷰 또는 그룹만보여주고, 여러 뷰가 들어가면 중첩으로 쌓이며 중첩 후에 각 뷰를 전환하여 쓰는 방식으로 자주 사용
    * TableLayout: 격자 모양의 배열을 사용해서 화면을 구성하는 방법


