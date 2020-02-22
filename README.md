# LEMP_docker

1. docker 와 docker-compose 를 설치합니다.

2. 소스를 다운받습니다. git clone https://github.com/minotech/LEMP_docker.git 

3. LEMP_docker 폴더 밑에 docker-compose.yml 파일과 4개의 하위 폴더가 있습니다.

   - www 폴더는 웹루트 폴더입니다. 웹소스를 복사해 넣으세요..
   - nginx 폴더 아래 conf.d/default.conf 파일을 이용해서 설정을 변경할 수 있습니다. 
   - php-fpm 폴더 아래에 설정파일 두개와 dockerfile 폴더가 있습니다. 
     docker-compose.yml 에서 설정파일 두개중 하나를 선택하여 입력하면 됩니다.
     dockerfile 폴더 밑에 dockerfile이 있습니다.  
     php-fpm 에 module을 넣었는데 추가로 넣으시려면 이파일을 수정하여 도커파일을 만드시면 됩니다. 일반적으로는 건드리지 않아도 됩니다.
   - mariadb 폴더 아래에는 데이타가 저장되는 data 폴더와 설정파일이 my.cnf가 있는 config 폴더가 있습니다.
     log 폴더는 작동하지 않습니다.
     
4. docker-compose.yml 파일을 잘 보시면 volumes 부분의 앞부분이 host 컴퓨터의 폴더이고, :뒷부분이 컨테이너 폴더입니다.
   컨테이너 폴더는 건드릴 필요없고, host 컴퓨터 부분만 변경하면 됩니다.
  
  volumes:
  
              - ./mariadb/data:/var/lib/mysql
              
  예를 들어 이부분을 보면 ./mariadb/data 현재 디렉토리밑의 mariadb/data 폴더를 의미합니다. 
  :/var/lib/mysql 이것은 컨테이너의 폴더입니다.
  즉 컨테이너에서 /var/lib/mysql에 저장될 데이타를 
  host 컴퓨터의 ./mariadb/data로 저장시킨다는 의미입니다.
  
5. db: 그누보드등에서 db서버의 host를 주소나 localhost로 넣으면 안됩니다. 여기에 적여있는 db를 host이름에 넣어주세요..

6. 실행할때 docker-compose.yml 이 있는 폴더에 가서 다음 명령어로 실행시킵니다.
    $docker-compose up 

7. ctrl-z 를 누르면 종료없이 빠져나올수 있으며 백그라운드로 실행됩니다.

8. ctrl-c 를 누르면 종료됩니다. 또는 docker-compose stop 과 같은 명령어입니다.

9. 그누보드 신규 설치는 테스트하였습니다. 그런데, 기존 소스를 복사해와서 실행 시킬 때, 
   자동등록방지 캡챠가 안나오고 있습니다. 해결책을 알려주시면 감사하겠습니다.
   
   
10. 참고로 DB를 백업, 복원하는 법입니다.

   일반백업 : 
   $sudo mysqldump -u root --databases DB명 > /backup.sql      
   DB명 데이타베이스를 루트폴더 backpu.sql로 백업 
   
   $sudo mysqldump -u root -p비밀번호 --databases DB명 > /backup.sql   
   비밀번호 있는경우 -p비밀번호 추가 (p와 비밀번호사이 공백없음)

   도커백업 : $docker exec -i 컨테이너명  mysqldump -u root -p비밀번호  --databases DB명 > /backup.sql 
   
   일반복원 : mysql -u root -p비밀번호  < ./backup.sql
   
   도커복원 : $docker exec -i 컨테이너명  mysql -u root -p비밀번호  < ./backup.sql
