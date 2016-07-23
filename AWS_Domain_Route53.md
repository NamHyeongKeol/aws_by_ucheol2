# Route 53 : Domain 구입 #

1. Domain - Registered domains 클릭

1. Register Domain 클릭

1. 원하는 Domain keywoard 검색 후 선택

1. 2, 3 과정 수행 후 구입 완료

1. Domain - Pending requests 구매 진행 현황 확인 가능  
   (대기 시간 필요)


# Route 53 : Domain 연결 #

1. Hosted zones 클릭

1. 구매한 Domain 클릭

1. Create Record Set 클릭

     1) Name : 빈 칸도 OK

     2) Type : A - IPv4 address

     3) TTL (Seconds) : 그대로

     4) Value : 연결하고자 하는 EC2 Instance IP 주소

     5) Routing Policy : Simple (그대로)

1. Name Server (NS)

    1) Dashboard - Domain registration - 구입한 Domain 클릭 - Name Servers (4개) 확인

    2) Route53을 통해 구입한 경우는 설정 그대로

    3) 다른 업체를 통해 구입한 경우 해당 싸이트의 Name Server 입력란에 4개 모두 기입

1. ```sudo vi /opt/nginx/conf/nginx.conf```

	 	server {  
			listen 80;   
			server_name 연결 시킬 도메인; 
			passenger_enabled on; 
			root /home/ec2-user/해당폴더/public; 
		}

    '연결시킬 도메인'에 '사용할 도메인' 입력