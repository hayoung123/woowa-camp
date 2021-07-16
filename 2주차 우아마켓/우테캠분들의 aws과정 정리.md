- 관리자로 로그인 
  - 이름 : baedal2 
  - 비밀번호 : 호눅스가 공유한 csv에서 공유한 문자열

- console의 IAM로 이동

- 사용자 추가
  - 이름은 tc_~~~로
  - 자동 생성된 비밀번호 체크
  - ❌ 비밀번호 재할당 체크 해제 ❌
  - student 그룹으로 추가
  - csv 다운로드 받아둘 것!!
    - 이 파일에 있는 password로 이후에 로그인 할 수 있음.

---

1. Ubuntu 20.04LTS 64bit arm
2. t4g micro memory 1gb
3. 인스턴스 세부 정보 구성
    1. 다 기본
    2. 다음 스토리지
4. 스토리지 추가
    1. 옵션 그대로 하고 다음
5. 태그 추가
    1. 키: Name, Value: cothis(아이디? 팀이름?)
6. 보안그룹 구성
    1. 아이디-web-sg
    2. web sg for 아이디 server
    3. ssh tcp 22번 포트 개방
7. 인스턴스 시작 검토
    1. 경고창 무시
    2. 새 키페어 생성, wootech-아이디-key, 키페어 다운로드!
    3. 인스턴스 시작

---

1. sudo apt update (일종의 앱스토어 업데이트 같은느낌)
2. 아래는 아직 해보라고 안하심
3. sudo apt install nodejs (이거 구버전임)
4. sudo apt install apache2 (데모로 보여주시는거)
5. cd /var/www/html/
6. ls (파일 목록 보기)
7. sudo service apache2 status (서비스 상태 확인)
8. sudo systemctl status apache2 (서비스 상태 확인 요즘 더 많이 씀)
9. 내 인스턴스 퍼블릭 IPv4 주소 복사해서 주소창에 입력해보면 → 안된다.(보안그룹 설정안했기때문에)
10. 보안그룹 규칙 추가
    1. 인바운드 규칙 편집에서 추가
    2. http(80포트) 소스(any ip v4)