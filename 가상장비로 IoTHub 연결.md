Device Explorer
----------
<p>
IoTHub의 장비관리와 데이터관련 테스트를 할 수 있는 Device Explorer를 다운로드 한다.<br>
https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer<br>
SetupDeviceExplorer.msi를 찾아서 다운로드 받아 설치하면 된다.

윈도우가 아니라면 아래 링크를 확인한다. <br>
https://github.com/Azure/iothub-explorer#iothub-explorer

<img width="686" alt="device explorer" src="https://user-images.githubusercontent.com/6082076/46904710-abec5f80-cf23-11e8-9ef0-bffc7a3a9a51.PNG">
<br>
실행되면 IoT Hub에서 복사해둔 iothubowner의 연결 문자열을 입력하고 Update를 클릭한다. <br>
Successfully 메시지가 떴으면 연결 성공이다.
</p>

<p>
Management로 이동해서 장비를 추가한다.<br>
<img width="686" alt="device explorer" src="https://user-images.githubusercontent.com/6082076/46904712-abec5f80-cf23-11e8-8aba-3ae1fcb53d21.PNG">
<br>
장비이름을 입력하고 Create를 클릭한다. 
</p>

<p>
추가 됐으면 연결문자열을 메모장에 복사해둔다.<br>
<img width="686" alt="device explorer" src="https://user-images.githubusercontent.com/6082076/46904711-abec5f80-cf23-11e8-8079-550945b4cbd9.PNG">
</p>

<p>
위 장비추가는 애저 포탈에서도 당연히 사용 가능하다.<br>
<img width="845" alt="device explorer _" src="https://user-images.githubusercontent.com/6082076/46904713-abec5f80-cf23-11e8-9c84-9f9839d75f7d.PNG">
<br>
IoT devices에서 Add를 누르면 된다.

장치를 연결할 준비가 되었으니 가상장비를 통해 데이터를 보내보자.<br>
Device Explorer는 계속 사용할 예정이니 그대로 실행해둔다.
</p>

가상장비에서 데이터 보내기
---------

<p>
마이크로소프트에서 기본으로 제공하는 라즈베리파이 가상 온라인 장비 시뮬레이션이 있다. 이를 사용해서 간단하게 IoTHub로 데이터를 보내보자.<br>
https://azure-samples.github.io/raspberry-pi-web-simulator/<br>
사이트에 접속해보면 왼쪽에는 라즈베리파이에 Led와 온도센서가 연결되어 있고<br>
오른쪽에는 코드와 출력창이 있다.<br>
<img width="722" alt="default" src="https://user-images.githubusercontent.com/6082076/46904716-ac84f600-cf23-11e8-9913-3dfa7218d62f.PNG">
<br>
코드에서 위쪽 연결문자열 변수에 ‘[Your IoT hub…..]’라고 되어 있는 부분을 위에서 추가한 장비 문자열로 바꾼다. 

getMeassage 안에 <br>
messageId : messageId, <br>
deviceId : “Raspberry Pi….” <br>
두줄을 삭제한다.

바로 Run을 눌러 실행한다.

Led가 2초마다 깜빡 거리는 걸 확인할 수 있다.
</p>

<p>
Device Explorer로 가서 Data 탭으로 이동한다.<br>  
<img width="686" alt="device explorer" src="https://user-images.githubusercontent.com/6082076/46904709-ab53c900-cf23-11e8-84f5-777f7f48d0b9.PNG"> <br>
Monitor를 클릭하면 데이터가 들어오는 걸 확인할 수 있다.<br>
가상장비사이트에서 Stop을 눌러 일단 멈춰 둔다.
</p>

<p>
애저의 Storage의 blob에 실제 저장된 값을 확인하자.<br>
<img width="845" alt="default" src="https://user-images.githubusercontent.com/6082076/46904717-ac84f600-cf23-11e8-9702-cb649feb0047.PNG"> <br>
Storage에 Blob에서 추가한 컨테이너를 클릭한다.<br>
csv파일을 클릭한다. Blob편집으로 이동한다.<br>
데이터가 쌓여 있는 걸 확인할 수 있다.
</p>

가상장비로 메시지 보내기 
-------

이번에는 Device Explorer를 통해서 가상장비로 메시지를 보내 보자<br>
먼저 가상장비 사이트로 이동해서 코드를 수정한다.<br>
현재 Led가 데이터를 보낼 때 마다 On되고 메시지가 왔을 때도 On이 되고 있다.<br>
데이터 보낼 때 On되는 코드를 삭제한다.

sendMessage함수에서<br>
else { <br>
        blinkLED(); <br>
        console.log('Message sent to Azure IoT Hub'); <br>
} <br>
이부분에서 blinkLED();를 삭제한다. <br>

<p>
Run을 클릭해서 실행한다.
<img width="686" alt="device explorer" src="https://user-images.githubusercontent.com/6082076/46904714-ac84f600-cf23-11e8-8ba6-d6024dd517d4.PNG"> <br>
Device Explorer에서 Messages To Device로 이동한다.<br>
Message를 입력하고 Send를 클릭한다.
</p>

<p>
가상장비사이트의 출력창에 메시지가 오고 LED가 깜빡거리는 걸 확인할 수 있다.
<img width="951" alt="default" src="https://user-images.githubusercontent.com/6082076/46904715-ac84f600-cf23-11e8-8846-a0bc5eae6fb0.PNG">
  
추가로 메시지 보내는 시간을 바꾸고 싶으면 <br>
open 함수에서 setInterval(sendMessage, 2000);  <br>
이부분에서 2000이 밀리세컨드 값이므로 이걸 바꾸면 보내는 시간 또한 변경 가능하다. <br>
<p/>

