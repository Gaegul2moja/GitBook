안드로이드 앱 프로그래밍 정리
====

##### 본 내용은 안드로이드 스튜디오-> JAVA, Do it 안드로이드 앱프로그래밍 교재를 기준으로 정리되었습니다.

* 그래픽을 구현하는 5단계
    * 1단계: 뷰 상속하기
    * 2단계: 페인트 객체 초기화하기
        <br/> 만든 뷰에 다음 코드로써 객체를 초기화 합니다
        ```java
        paint = new Paint();
        paint.setColor(Color.RED);
        ```
    * 3단계: onDraw()메소드 구현하기
        * onDraw메소드 안에서 캔버스 개체에 정의된 메소드들 중 원하는 걸 호출(재정의)해서 그린다.
        ```java
        protected void onDraw(Canvas canvas){
            super.onDraw(canvas);
            canvas.drawRect(100,100,200,200,paint);
        }
        ```
    * 4단계: onTouchEvent()메소드 구현하기
    * 5단계: 메인 액티비티에 추가하기
        * 메인 액티비티에 만든 CustomView 클래스를 추가할 때는 new연산자를 이용하여 객체를 하나 만든 후, setContentView()를 이용해 화면 전체에 보여준다.
        ```java
        CustomView myView = new CustomView(this);//만든 클래스가 CustomView
        setContentView(myView);
        ```
        * xml레이아웃에 추가하려면 그냥 추가
        ```xml
        <CustomView
            ...
        />
        ```
<br/><br/><br/>
* 그래픽을 그릴 때 필요한 클래스
    * 캔버스(Canvas): 뷰의 표면에 직접 그릴 수 있게 만들어 주는 객체로, 그래픽 그리기를 위한 메소드
        * 점, 선, 사각형, 둥근모서리 사각형, 원, 타원, 아크, 패스, 비트맵 등을 그릴 수 있음
    * 페인트(Paint): 그래픽 그리기를 위해 필요한 색상 등의 속성을 담고 있음
    * 비트맵(Bitmap): 픽셀로 구성된 이미지로 메모리에 그래픽을 그리는데 사용
    * 그리기 객체(Drawable): 사각형, 이미지 등의 그래픽 요소가 객체로 정의

* 그래픽을 위한 카메라 객체
    * 사진을 찍기 위한 하드웨어 카메라 클래스가 아닌, android.graphics.Camera는 보는 시각에 따라 달라지는 입체 효과를 주는데 사용
<br/>
-----------------------
<br/><br/>
* 핸들러 사용하기
    *  스레드는 동시 수행이 가능한 작업 단위이며, 현재 수행되는 작업 이외의 기능을 동시에 처리하고자 할 때 새로운 스레드를 만드렁 처리할 수 있기에, 효율적인 처리가 가능하지만 동시에 리로스를 접근 할 경우, 데드락(Deadlock)이 발생해서 시스템이 비정상적으로 동작할 수 있음<br/><br/>
    *  지연 시간이 길어질 수 있는 애플리케이션의 경우, 오래걸리는 코드를 분리한 후, UI에 응답을 보내주는 형식을 사용하는데, 이를 위해 두가지 시나리오를 사용할 수 있다
        *  서비스 사용하기: 백그라운드 작업은 서비스로 실행하고 사용자에게는 알림 서비스를 이용해서 알려준다
        *  스레드 사용하기: 스스레드는 동일프로세스에 있기에 작업 수행의 결과를 바로 처리할 수 있으나, UI객체는 직접 접근 할 수 없으므로, 핸들러 객체를 사용해서 순서대로 접근하여 데드락 문제를 해결한다
<br/><br/>
* 스레드로 메시지 전송하기
    * 핸들러의 기능이 스레드에서 메인 스레드로 메시지를 전달하는 것인데, 별도의 스레드에서 메인 스레드가 관리하는 UI객체에 직접 접근할 수 없기 때문이었다. 그런데, 메인스레드에서 스레드로 메시지를 전달하는 경우, 변수를 통해 데이터를 전달할 수도 있지만, 핸들러가 사용하는 메시지 큐를 이용해서 순차적으로 메시지를 실행하는 방식이 필요한 경우, 다음과 같은 방식을 이용한다. 
        * 핸들러가 처리하는 메시지 큐는 루퍼(Looper)를 통해 처리된다. 루퍼는 메시지 큐에 들어오는 메시지를 지속적으로 보면서 하나씩 처리한다.메인스레드의 경우, UI객체의 처리를 위해 메시지 큐와 루퍼를 내부적으로 처리하고 있어서 핸들러를 이용한 메시지 전송이 가능하지만, 별도의 스레드는 루퍼가 없으므로 만든 후 실행해야 한다.
<br/><br/>
* AsyncTask 사용하기
    * 새로 만든 스레드에서 UI 객체에 직접 접근하는 것이 불가능 하기에 핸들러를 사용하는 것이지만, 핸들러를 사용하면서 코드를 조금 더 복잡하게 만드므로, 더 쉽게 하고 싶다면 AsncTask 클래스를 사용할 수 있다.
<br/><br/>
* 스레드로 애니메이션 만들기
    * 애니메이션을 여러 이미지를 연속으로 바꾸면서 만들고 싶을 때 스레드를 이용하는 경우가 많다. 
        * 진짜로 배열에 이미지 넣어서 순차적으로 돌리는 방법이다
    * 트윈 애니메이션(Tweened Animation): ㅂ애니메이션 대상과 변환 방식을 지정하면 애니메이션 효과를 낼 수 있도록 만들어주기에, 대상만 지정하면 시스템이 적절하게 연산해 준다.<br/>
    scale 증가 예시>
    ```xml
    <!--scale.xml-->
    <set>
        <scale
            android:duration="2500"
            android:pivotX="50%"
            android:pivotY="50%"
            android:fromXScales="1.0"
            android:fromYScales="1.0"
            android:toXScale="2.0"
            android:toYScale="2.0"
        />
    </set>
    ```

    ```java
    //mainactivity.java
    ...
    Animation anim = AnimationUtils.loadAnimation(getApplicationContext(), R.anim.scale);
    v.startAnimation(anim);
    ...
    ```
-----------------------
* 네트워킹
    * 인터넷 망에 연결되어 있는 원격지의 서버 또는 단말 통신을 통해 데이터를 주고받는 일반적인 일들을 포함한다. 많이 사용하는 네트워킹 방식은 대부분 클라이언트가 서버로 연결하여 데이터를 요청하고 응답을 받는다는 개념이다. 그러나, 실제로는 문제가 생겨 부하 분산 등의 목적으로 N-tier모델 등을 사용한다.<br/>클라이언트와 서버 간의 관계는 단말간의 통신이 일반화되며, 서버를 두지 않고도 단말끼리 서버와 클라이언트 역할을 하면서 통신할 수 있는 피어-투-피어(Peer-to-Peer)통신으로 불리는 P2P 모델로도 변형될 수 있다.
 <br/><br/>
* 소켓 사용하기
    * IP 주소를 이용해 목적지 호스트를 찾아내고 포트를 이용해 통신 접속점을 찾아내는 방식이 소켓 연결이고 이는 TCP와 UDP 방식으로 나눌 수 있다. <br/>HTTP 프로토콜은 소켓으로 연결 한 수에 웹서버로 요청을 전송하고 응답을 받는 과정을 거치나, 비연결성 등의 특성으로 인해 실시간 데이터 처리의 경우, 소켓 연결을 선호한다. 그러나, 최근에는 환경이 좋아져, 앱에서도 HTTP프로토콜을 이용한 데이터 송수신에 문제가 없다.
-----------------------------
* 데이터 베이스
    * 안드로이드에서 많은 데이터를 저장하고 체계적으로 관리하기 위해서 데이터베이스를 사용하게 된다. 특히, 표준 SQL문을 이용해 데이터를 조회하는 관계형 데이터베이스가 효율적인 접근이 가능하여 더 유리하다. 안드로이드에 포함된 것은 SQLite라는 데이터베이스이다. 
    * 데이터베이스 활용 순서
        * 데이터베이스 만들기->테이블 만들기->레코드 추가하기->데이터 조회하기
    ```java
    SQLiteDatabase db;
    ...
    databaseName = dataaseNameInput.getText().toString();
    createDatabase(databaseName);
    //데이터 베이스 생성 메소드 호출
    ...
    tableName = tableNameInput.getText().toString();
    createDatabase(tableName);
    ...
    public void CcreateDatabase(String name){
        dp = openOrCreateDatabase(name, MODE_WORLD_WRITEABLE,null);
        databaseCreated = true;
    }
    private void createTable(String name){
        db.exeSQL("create table "+name+"("+"_id integer PRIMARY KEY autoincrement,"+"name text, "+"age integer, ","phone text);"); 
        tableCreated = true;
    }
    ...
    private int insertRecord(){
    ...
    db.execSQL("insert into employee(name, age, phone)values('John',20,'010-7788-1234');");
    ...

    }
    ```
* 헬퍼 클래스를 이용해 업그레이드 지원하기
    * 테이블의 정의가 바뀌거나 만들어져서 스키마를 업그레이드 해야 할 경우, api에서 제공하는 헬퍼(Helper)클래스를 사용한다. 이 클래스의 구조는 바뀔 수 있으나, 바뀌면 문제가 생길 수 있으므로, 이미 사용하고 있는 상태인지 구별한 다음 처리해야 한다. 이 클래스를 이용해 조회 또한 할 수 있다.
  <br/>
* SQL을 하나의 문자열이 아닌 여러개의 파라미터로 나누어 구분하고 각각을 파라미터로 전달하여 실행하는 방법 또한 있음