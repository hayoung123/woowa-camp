# 0720

## AWS ubuntu와 로컬 work-bench  mysql 연동하기

> tcp 포트가 로컬에 한정돼 있어서 문제 발생

1. `sudo vim /etc/mysql/my.cnf`

2. 마지막 줄에

   `[mysqld] `

   ``bind-address=0.0.0.0`

   위의 2줄 추가

3. `sudo netstat -antp` 을 쳐보면 

   아래처럼 출력된 글을 볼 수 있다. 

   - tcp        0      0 127.0.0.1:3306          0.0.0.0:*               LISTEN      34394/mysqld

   위처럼 돼있음 restart 필요

4. `sudo service mysql restart`

5. `sudo netstat -antp` 다시 확인

   위의 구문이 아래처럼 변경되면 끝!

   - tcp        0      0 0.0.0.0:3306            0.0.0.0:*               LISTEN      34937/mysqld 

끝!



 