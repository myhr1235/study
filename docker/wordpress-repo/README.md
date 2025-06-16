프로젝트 개요

-----------------------------------

목적
1. EC2 환경에서 Docker container설치하여 Gitops 연동
2. Git action을 통한 배포 환경 구성
3. deploy.yml, docker-compese.yml 등 의 파일 직접 생성하여 관리

-----------------------------------

기본 구성
1. Wordpress는 Dokcer Compose로 배포
2. 내부 설정값(wp-config.php or .env)은 GitHub에 전장
3. CI/CD로 Wordpress 컨테이너에 변경된 설정값 자동 반영


--------------------------------

📂 study/docker/wordpress-repo
├── docker-compose.yml
├── wp-config.php             ← GitHub로 버전관리
├── .env                      ← 비밀키 분리 (GitHub Actions에서 참조)
├── .github/
│   └── workflows/
│       └── deploy.yml        ← CI/CD 스크립트
