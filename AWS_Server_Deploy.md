# AWS Instance Create

1. INSTANCES - instances / Launch Instance 클릭
2. Amazon Linux / Select 클릭
3. t2.micro (Free) / Review and Launch 클릭
4. 하단의 Security Groups에서 Edit security groups 클릭  
   : Add Rule - HTTP - 80 Port 생성 후 Review and Launch 클릭

# Putty 실행

1. puttygen 실행
2. Conversions - Import key 클릭
3. AWS key 선택 - Save private key 클릭
4. 경고창 '예' 선택 - 파일 생성 완료
5. putty 실행
6. Host Name : ec2-user@"aws public IP"
7. 왼쪽 Category에서 Connection - SSH - Auth에서 Private key file for authentication에 앞서 만든 key 입력


# AWS Server Deploy


1. ```chmod 400 Downloads/key_0601.pem```  
**: chmod - 파일 권한 지정 (-rwxrwxrwx / 읽기 쓰기 실행)**

2. ```ssh -i Downloads/key_0601.pem ***ec2-user@***52.78.23.188```  
**: AWS key file 등록**

3. ```sudo yum update```  
**: 기존 파일 업데이트**

4. ```sudo yum search git```  
**: git 관련 설치 파일 목록 검색**

    ```sudo yum install git-all.noarch```  
    **: git-all.noarch 파일 설치**

    ```git clone http://github.com/"github 주소"```  
    **: github file을 불러와 해당 이름 디렉토리에 설치**

5. ```ruby --version```  
**: ruby 버전 확인**

    ```gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3```
    ```curl -sSL https://get.rvm.io | bash```  
    **: RVM 사이트에서 복사 후 붙여넣기**

    ```source ~/.profile```  
    **: 이전 코드를 입력하고 나면 위 코드를 입력하라는 지시가 나온다.**

    ```rvm install 2.2.5(버전)```  
    **: ruby 2.2.5 버전 설치 ( 2.3.0 버전은 문제가 있다고 한다. )**

6. ```gem install bundle```  
**: bundle 기능을 사용하기 위한 젬 설치**

   ```bundle install```  
   **: gem 파일 설치**
    
7. ```rake assets:precompile```  
**: css/js 등 assets 파일 수정 시 실행 필요**

8. ```gem install passenger```  
    **: server 가동을 위한 젬 설치(C9 run project 기능)**  
	**: nginx도 같이 설치 필요**

    ```sudo passwd```  
    **: 새 비밀번호 설정**

    ```su```  
    **: root 권한으로 전환 (비밀번호 입력)**  
    **: ec2-user가 root로 전환**

	```passenger-install-nginx-module```  
	**: 언어 Ruby 지정**
  
		오류 발생 시 (It is recommended that you relax permissions as follows)
		sudo chmod p+x "/home/ec2-user"

		또 다시 오류 발생 시 (Some  required software is not installed)
		sudo yum search curl (curl 관련 설치 파일 검색)
		sudo yum install libcurl-devel.x86_64

		aws instance에서 무료인 t2.micro로 설치하여, 메모리 부족과 관련된 메세지가 뜰 경우 (복사 + 붙여넣기) 
		sudo dd if=/dev/zero of=/swap bs=1M count=1024
		sudo mkswap /swap
		sudo swapon /swap

	**선택지 : 1번 선택 (다운로드 받아서 설치)**

	```exit```  
	**root → ec2-user**

	```sudo vi /opt/nginx/conf/nginx.conf```  
	**: 환경 설정 파일 실행**
	
	```
	user ec2-user #최상단에 작성
	```

	```
	server {
		listen 80; 
		server_name 연결시킬 도메인; 
		passenger_enabled on; 
		root /home/ec2-user/해당폴더/public; 
		}
	```
	**: server 윗부분에 위 코드 입력**

		pwd (root 경로 확인하여 참고)

1. ```rake db:migrate RAILS_ENV=production```  
**: db 등록**

		오류 발생 시
		'Gemfile'에서 gem 'therubyracer'라는 부분을 주석 해제
		bundle install

		c9 작업시, terminal에서 git pull 필요

1. ```rake secret```  
**: 실행 후 뜨는 코드 복사**

1. ```export SECRET_KEY_BASE=(복사한 코드)```  
**: 환경 변수 설정**

		1) config/secrets.yml 파일 내 SECRET_KEY_BASE 변수에 복사한 코드 직접 작성
		2) Figaro gem 설치 후 적용 (추천)
        2-1) config/application.yml 파일에  
             AWS_ACCESS_KEY_ID  
             AWS_SECRET_ACCESS_KEY  
             SECRET_KEY_BASE  
             값을 모두 설정한다.

1. ```sudo /opt/nginx/sbin/nginx```  
**: 서버 실행**

		오류 발생 시, route 확인
		도메인 연결 : aws 'Route 53' 서비스 활용

1. ```mkdir tmp```  
**: tmp 디렉토리 생성 (tmp 디렉토리가 없을 경우)**

    ```touch tmp/restart.txt```  
**: 서버 재부팅**

1. ```sudo /opt/nginx/sbin/nginx```  
**: nginx 서버 실행**

    ```sudo /opt/nginx/sbin/nginx -s reload```  
**: nginx 서버 conf를 수정할 경우 리로드**

**1. 로그파일 보기 (tail 명령어 사용)**

Rails 로그파일 위치: log/production.log
	
Nginx 로그파일 위치: /opt/nginx/logs/error.log

	$ tails log/production.log # 파일의 끝에서 부터 읽어서 띄움
	$ tail -n 100 log/production.log # 100줄까지 읽음
	$ tail -f log/production.log # rails s 했을 때 처럼 실시간으로 띄움
