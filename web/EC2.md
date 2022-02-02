## Amazon Elastic Compute Cloud(Amazon EC2)

- Amazon Web Services(AWS) 클라우드에서 확장 가능 컴퓨팅 용량을 제공
- 원하는 수의 가상 서버를 구축하고 보안 및 네트워킹을 구성하며 스토리지 관리 가능
- 장 또는 축소를 통해 요구 사항 변경 또는 사용량 스파이크를 처리할 수 있으므로 트래픽을 예측할 필요성이 줄어듬

### EC2 연결

- SSH 연결
    - 연결인증방식 : SSH
    - 연결인증방식 : SSH 키 페어 인증
    - 감사방법 : 직접 로깅 혹은 기타 3rd Party 어플리케이션 필요
    - 연결 요구사항 : 인터넷 연결 필요
    - 주요 기능 : 기본적인 SSH 통신
    
- EC2 인스턴스 연결
    - 동작방식 : 임시 SSH 키를 생성하여 EC2 로 밀어넣어 연결하는 방식
    - 연결인증방식 : IAM 인증
    - 감사방법 : 연결기록만 감사가능(CloudTrail)
    - 연결 요구사항
        - 인터넷 연결 필요
        - EC2-instance-connent 에이전트 설치 필요
    - 주요 기능 : 기본적인 SSH 통신
    
- Systems Manager Session Manager
    - 동작 방식 : AWS API 기반으로 EC2와 통신
    - 연결인증방식 : IAM 인증
    - 감사방법 :  연결기록 및 세션사용기록을 CloudTrail/CloudWatch 과 연동해 확인가능
    - 연결요구사항 : SSM 에이전트 설치
    - 주요기능
        - 기본적인 SSH 통신 + SCP 터널링(파일전송)
        - 기타 EC2에 명령 전송
    
- EC2 직렬 콘솔
    - 동작 방식 : EC2 시리얼 포트로 연결
    - 연결인증방식 : IAM 인증 + Root Password
    - 감사방법 :  연결기록만 감사가능(CloudTrail)
    - 연결요구사항
        - 계정 단위 활성화
        - OS Password 설정
        - 특정 인스턴스 타입/리전에서만 사용 가능
    - 주요기능
        - 직접 컴퓨터에 모니터 키보드를 붙인 것 같이 동작
        - 부팅 및 네트워크 문제 해결
