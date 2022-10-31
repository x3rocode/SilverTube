# SilverTube

<p align="center"><img src="https://user-images.githubusercontent.com/51151970/198916471-09aab37b-c15e-4c1c-ae10-12d91b9d18b5.png" width="30%" height="30%"></p>
이 프로젝트는 
[황반변성](https://namu.wiki/w/황반변성)
을 앓고 계신 외할머니를 위해 만들었습니다. 
책 읽어주는 유튜브를 즐겨 들으시는데, 눈이 거의 안보이셔서 유튜브앱 실행이나 검색은 물론 play/pause 버튼 터치조차도 버거워하십니다.
그래서 할머니가 자주 사용할 기능을 최대한 크게! 잘보이게! 쉽게! 만드는것을 목표로 잡았습니다.


# Feature
### 위젯
위젯에 버튼을 만들어서 앱 실행을 할 필요조차 없게끔 만들었습니다. 그냥 앱에다가 구현하면 편했겠지만 분명 뭐 잘못누르시다가 앱 꺼지면 다시 못켜실게 뻔했거든요..

### 할머니가 쓸 기능
위젯에 다음 기능들을 버튼으로 만들어 화면 꽉곽 채워 배치했습니다.
- 시작
- 멈춤
- 이전
- 다음
- 작게
- 크게

그리고 할머니의 시야를 분석(?)하여 최대한 이 6개의 버튼들을 쉽게 구분할수 있게끔 하려고 노력했습니다. 황반변성은 단순히 시력이 낮아지는게 아니라 
<p align="left"><img src="https://user-images.githubusercontent.com/51151970/198922129-c219790b-8583-44b1-be62-6d4bae97a206.png" width="40%" height="40%"/></p> 
위와 같이 시야가 왜곡되어 보이거나 알수없는 색카만 무언가로 가려져 안보이거나 어둡게 보인다고 합니다.
할머니는 시야는 다음과 같은 상황입니다.

- 버튼 사이 간격이 좁으면 버튼이 하나로 뭉개져 보임(: 버튼 크기를 무조건 키우는게 답이 아니였다)
- 무조건 보색이라고해서 잘보이는게 아님. 채도가 비슷한 색들은 다 비슷해보임. 예를 들면 <p align="left"><img src="https://user-images.githubusercontent.com/51151970/198917815-5e287aa5-e6e1-405d-af7f-7bb18501e0c4.png" width="30%" height="30%"/></p> 
위 사진의 녹색과 적색 구분이 어렵다고 함(: 글씨와 버튼을 보색으로 사용하거나, 무조건 쨍한 색을 사용하는것도 답이 아니였따)

색 배치는 아직도 실험중에 있습니다. 여러 색 배치하여 데모를 뽑아가서 할머니께 가장 잘보이는 색 여쭤보는 과정을 좀 여러번 거쳐야 할 것 같아요. 예쁜색을 뽑는거보다
할머니가 잘 볼수 있는 색을 고르는게 더 어려웠던것 같아요.

### 엄마나 내가 쓸 기능
엄마는 매일 할머니댁에 들르시는데, 그때마다 플레이리스트를 업데이트해주는 방식입니다. 할머니가 유튜브에서 볼 영상 검색을 못하셔서, 적당한 영상들을 미리 추가해놓는 방식입니다. 
영상을 추가하는 방식은 총 세가지입니다.
- 영상 한개 추가
- 채널의 모든 영상 추가
- 재생목록 추가
- 리스트에서 선택 재생도 가능해요!

모두 링크 공유버튼을 눌러 URL을 앱에 붙혀넣는 방식으로 추가됩니다. 추가 후에는 리스크에 현재 재생목록이 업데이트됩니다. 리스트에서 영상을 삭제할 수 있고 전체삭제도 할 수 있습니다.

# Used technologies

### Background task, Service
배터리 관리 문제 등으로 OS자체에서 앱을 종료해버리면 위젯이 동작하지 않았습니다. 유튜브 플레이어가 context를 인자로 받기 때문인 것 같았습니다.
그래서 Service를 추가하여 앱이 Background에서 계속 상주하도록 했습니다.

### Shared preferences
앱과 위젯을 완전히 분리시키기 위해 Youtube Player View를 Activity에 종속되지 않게끔 했습니다. 플레이리스트에 영상을 추가하면 Shared preferences를 사용하여 내부 저장소에 바로 저장합니다.
위젯쪽에서 영상을 재생할 때 영상들에 대한 정보를 앱이 아닌 내부저장소에서 바로 가져오기 때문에, 앱 UI가 종료되어도 재생목록을 받아올 수 있습니다. SettingYoutube라는 Singleton
클래스를 만들어서 Activity든 Widget이든 이 인스턴스를 사용하도록 했습니다.

### Youtube API
유튜브에서 제공하는 공식 API를 사용했습니다. 로그인 OAuth2때문에 꽤나 어려움을 겪었지만 어찌저찌 했습니다. 아래의 오픈소스를 사용하면 로그인 없이도 영상들을 불러올수 있는데
굳이 로그인을 넣은 이유는 광고 제거를 위해 유튜브 프리미엄을 사용해야했기 때문입니다. 또한 처음엔 앱 내부에 플레이리스트를 만들지 않고 유튜브 계정의 재생목록을 불러오는 방식으로
구현하려 했었기 때문에 나의 재생목록을 가져오기 위해 로그인을 구현했습니다.

### [Android Youtube Player](https://github.com/PierfrancescoSoffritti/android-youtube-player)
최고의 플레이어!

### Retrofit
네트워킹


# TODO

### 터치를 물리버튼으로 변경하기

<p align="left"><img src="https://user-images.githubusercontent.com/51151970/198920037-8f0959e1-65ab-4fab-9084-c1d32c75162d.png" width="30%" height="30%"/></p> 
터치보다는 만져서 알수있는게 훨씬 구분하기 편할것 같다. 예전에 커스텀 키보드 찾다가 봤었던 **매크로키보드**가 번뜩 생각이 났다!! 바로 주문했고 배송오는중이다. Android OS랑 호환이 되는진
모르겠는데, 블루투스 방식으로 키입력 인식해서 연결해둔 펑션만 호출해주면 되지 않을까?!? 그럼 위젯을 빼고 서비스에 블루투스 자동연결과, 끊어지지 않게끔 하는 코드를 넣으면 될 것 같다.
저 다이얼 돌려서 음량조절하는건 꼭 넣을것이다.

### 플레이리스트 자동 업데이트

생각보다 할머니가 유튜브를 자주 들으신다고 한다. 책 읽어주는 채널들의 영상을 거의다 시청했을거라고 했다. 영상 선택이나 검색은 못하시고 들은거 계속 또듣고 한다는데,
새 영상이 업로드되면 바로 플레이리스트에 넣고, 다들은건 플레이리스트에서 자동으로 삭제하는 기능도 넣어볼까 한다. 근데 들었던거 또듣고싶으면 어쩌지..? 흠
( *이건 매크로키보드 남는 버튼으로 ㄴ따봉ㄱ키를 만들어서 할머니만의 플레이리스트를 따로 만들어둘까 한다.. 고민 좀 해봐야할듯.* )
아무튼 새영상 자동 추가 기능은  **매일 정해진 시간에 구독해둔 채널의 가장 최근영상의 제목을 가져와서 전날의 것과 비교**한 후 내부저장소에 저장해두면 될 것 같다.

### 로딩 중 표시

당연한 말이지만 새 영상을 불러올땐 시간이 좀 걸린다. 하지만 할머니가 성격이 급해가지고 다음 영상 바로 안나온다고 버튼 여러번 누르고 그럴 것 같다.
다음 영상 불러오기 전까지 버튼을 Block처리해놔도 되겠지만.. 처리중일때 버튼 색이 바뀐다던지, Progress bar같은걸 추가하던지 해야할 것 같다.
물리버튼으로 변경하면... 방법이 없긴 한데 좀 생각해봐야 할 듯. 저 버튼이 블루투스 모델은 RGB가 안되더라ㅠ 시모록..

### 코드 정리

좀 해라

