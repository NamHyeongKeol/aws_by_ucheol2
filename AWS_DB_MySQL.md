# AWS RDS #

1. Getting Start Now / Launch DB Instance

1. MySQL - Select

1. Production - MySQL - Next Step

1. **Instance Specifications**  
   DB Engine : mysql  
   License Model : general-public-license (그대로)  
   DB Engine Version : 5.6.27 (그대로)  
   DB Instance Class : db.t2.micro (Free)  
   Multi-AZ Deployment : No (Free)  
   Storage Type : General Purpose (SSD)  
   Allocated Storage : 5GB  

      **Settings**  
   DB Instance Idetifier : DB 인스턴스 이름  
   Master Username : 관리자 이름 (admin)
   Master Password : 비밀번 호설정  
   Confirm Password : 비밀번호 확인  

1. **Network & Security**  
   VPC : Default VPC (vpc-2ca90b45) (그대로)  
   Subnet Group : default (그대로)  
   Publicly Accessible : Yes (그대로)
   Availability Zone : No Preference (그대로)
   VPC Security Group(s) : Create new Security Group (처음에는)

     **Database Options**  
   Database Name : DB 이름  
   Database Port : 3306 (그대로)  
   DB parameter Group : default mysql5.6 (그대로)  
   Option Group : default mysql-5-6 (그대로)
   
     **Backup**  
   Backup Retention Period : 7 days (그대로)  
   Backup Window : No Preference (그대로)  

     **Monitoring**  
   Enable Enhanced Monitoring : No   
   Monitoring Role : Default (그대로)  
   Granularity : 60 (그대로)  
   
     **Maintenance**  
   Auto Minor Version Upgrade : Yes (그대로)  
   Maintenance Window : No Preference (그대로)
   
1. Endpoint : " ~~ MySQL 접속 주소 ~~ "

     1) Security Group : rds Instance 선택

     2) 아래쪽 Tab 'Inbound' 선택

     3) Edit 클릭 & Add Rule 클릭

     4) Type : Mysql/Auara,  Source : Custom  
 
     5) 마지막 칸 : EC2 Instance IP 주소 / Group ID(추천) 입력


# Mysql Install #

1. ```sudo yum install mysql-server```  
   ```sudo yum install mysql-devel```  
**: yum으로 mysql 서버 설치**

1. ```sudo service mysqld start```  
**: mysqld 실행**

1. ```mysqladmin -u root password '비밀번호'```  
**: 루트 비밀번호 변경**  

1. ```mysql_secure_installation```  
**: mysql 안전설치**

1. ```mysql -u root -p```  
**: 접속 확인**

1. ```sudo cp /usr/share/mysql/my-huge.cnf /etc/my.cnf```  
**: 설정 파일 복사**

1. ```sudo vi /etc/my.cnf```  
**: UTF8 인코딩 셋을 사용하기 위해 파일 내용 변경**

    **[client]**  
```default-character-set = utf8```

    **[mysqld]**  
```init_connect = SET collation_connection = utf8_general_ci```
```init_connect = SET NAMES utf8```  
```character-set-server = utf8```  
```collation-server = utf8_general_ci```

    **[mysqldump]**  
```default-character-set=utf8```

    **[mysql]**  
```default-character-set=utf8```

1. ```sudo service mysqld restart```  
**: 설정 변경 후 mysql 재시작**

1. ```sudo chkconfig mysqld on```  
```chkconfig --list mysqld```  
**: 부팅 시 자동 시작 설정**

1. **mysql gem file 수정**

		group :production do
			gem 'mysql2'
		end

1. ```bundle install```

1. **config/database.yml 수정**

		production:
			adapter: mysql2
			database: humming_baby
			username: admin
			host: hummingbaby.caaytpdn8rwl.ap-northeast-2.rds.amazonaws.com
 			password: <%= ENV['RDS_ADMIN_PASSWORD'] %>
			port: 3306 
			pool: 5 
			timeout: 5000

1. **config/application.yml 수정**  
```RDS_ADMIN_PASSWORD: '비밀번호'```

1. ```mysql -h hummingbaby.caaytpdn8rwl.ap-northeast-2.rds.amazonaws.com -P 3306 -u admin -p```  
**: mysql remote 접속, MySQL 연결 완료**

* mysql 새로운 계정 만들기
```grant all privileges on 데이터베이스이름.* to 사용자이름@localhost identified by '비밀번호' with grant option;```