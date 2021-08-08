# ORM

> 객체를 관계형 DB에 매핑해 DB의 기능들을 추상적으로 사용할 수 있게 해주는 도구

## 장점

- ORM을 통해 SQL쿼리를 추상화해 사용할 수 있기 때문에 로직에만 집중가능
- 특정 DB에 대한 종속성이 사라져 바뀌는 상황에 유연하게 대처할 수 있다.
- DB 마이그레이션을 쉽게 할 수 있다.  (한 종류의 DB를 다른 종류의 DB로 옮기는 행위)

## 단점

- Raw Query 보다는 성능이 떨어진다고 한다.



# sequelize

## models/index.js

- /config/config.json 파일의 설정값을 읽어 sequelize를 생성
- models 폴더 아래 존재하는 js 파일을 모두 로딩
- db 객체에 Model을 정의하여 반환

-> 여러 모델들을 한 객체(db)에 담아 반환하는 역할

## 참고자료

[Sequelize CLI를 사용하여 User API 만들기](https://velog.io/@jeff0720/Sequelize-CLI%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EC%97%AC-%EA%B0%84%EB%8B%A8%ED%95%9C-User-API-%EB%A7%8C%EB%93%A4%EA%B8%B0-vdjpb8nl0k)

[시퀄라이즈-cli 분석글](https://velog.io/@ywoosang/%EC%8B%9C%ED%80%84%EB%9D%BC%EC%9D%B4%EC%A6%88-%EC%97%B0%EA%B2%B0-%EB%B0%8F-%EA%B8%B0%EB%B3%B8-%EC%84%B8%ED%8C%85)



