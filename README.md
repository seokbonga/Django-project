# Team DoriDori

사용자 위치정보 군집화를 통한 셔틀버스 노선편성 서비스 입니다.

## 담당 역할
- 주소 입력창을 제외한 지도 페이지 프론트엔드  
- templates/map.html, static/assets/js/map.js에 작업하였습니다.
---
## 기술스택
- 프레임워크: Django
- 프론트엔드: HTML, CSS, Javascript, bootstrap
- 백엔드: python
- 데이터베이스: sqlite3

## 결과예시
![스크린샷 2023-01-27 오후 8 51 30](https://user-images.githubusercontent.com/81648520/215080342-fc085832-66c0-4893-94ee-2962f66e0f67.png)

[구현 부분 영상](https://youtu.be/2uMvvIf_i0A)

## 문제 상황
- 웹 애플리케이션이 1GB의 지나친 메모리를 사용하여 성능이 떨어짐

- 지도에 경로를 표시할 때 GPS 좌표의 배열을 포함하는 경로 인스턴스를 생성해야 함

- 경로 인스턴스가 좌표 배열이 아닌 좌표 하나만 담는 상태로 만들어져 선을 그리는 것이 아니라 점을 찍어 선을 그리는 형태로 구현된 것을 발견함.

- 함수에 전달될 인자가 GPS좌표들을 담는 배열로 전달되도록 자료구조의 형태를 통일하고 알고리즘을 다시 설계하고 구현
  - 도보를 표시하기 위한 좌표 배열의 경우 출발지-탑승지, 하차지-목적지로 두 개의 경로 배열을 담는 자료구조로 표현<br>
  [[[출발지 위도, 출발지 경도] ... [탑승지 위도, 탑승지 경도]], [[하차지 위도, 하차지 경도] ... [목적지 위도, 목적지 경도]]]
  - 버스의 경로는 탑승지~하차지로 하나의 경로 배열을 담는 자료구조로 표현<br>
  [[탑승지 위도, 탑승지 경도] ... [하차지 위도, 하차지 경도]]
  - 경로를 그리는 함수 `drawRoute`는 매개변수로 GPS 경로를 담는 배열을 받아 경로 인스턴스를 생성함
    - 도보의 경로는 `forEach`문을 사용해 경로 인스턴스를 출발지~탑승지, 하차지~목적지 경로로 2개 생성하고, 버스의 경로는 탑승지~하차지로 하나의 경로 인스턴스를 생성함
- 결과 적게는 800개에서 많게는 1600개의 인스턴스가 생성되었기 때문에 소되되던 1GB의 메모리가 10MB로 감소하여 애플리케이션의 성능이 향상됨

## 실행

python 3.7 이상 버전 설치 후

```
- 가상환경 생성 
python -m venv venv

- 가상환경 실행
source ./venv/Scripts/activate

- 필요 package 설치
pip install -r requirements.txt

- migrate 명령어로 DB 생성
python manage.py makemigrations
python manage.py migrate

- 서버 실행
python manage.py runserver

- 브라우져로 접속
http://127.0.0.1:8000/
```
## 주의사항
```
# 같은 지역의 사용자가 4명 이상이 모였을 경우에만 클러스터링이 실행됩니다.

# no such table 에러가 발생할 경우 
  
  python manage.py migrate --run-syncdb 
  
  명령어를 실행하고 다시 서버를 실행해 주십시오.

```
