## ⚙️ 사전 준비 프로그램
팀원들이 프로젝트를 실행하기 위해 필요한 프로그램들:

1. **Windows Subsystem for Linux 2 (WSL2)**
   - Microsoft Store에서 Ubuntu 설치
   - 최신 버전으로 업데이트 필요

2. **Docker Desktop**
   - Windows에서 실행
   - WSL2 기반으로 동작
   - NVIDIA Container Toolkit 연동 필요

3. **NVIDIA GPU 드라이버**
   - CUDA 지원 GPU 환경 필요
   - `nvidia-smi`로 정상 작동 확인

4. **Python 3.10+**
   - `requests`, `openai` 패키지 설치 필요

  ---------------------------------------------
## 팀원용 MySQL 설치 & DB 세팅 가이드
1. MySQL 설치
Windows

MySQL Installer
 다운로드

설치 시 Server Only 또는 Developer Default 선택

root 계정 비밀번호 설정 (꼭 기억하기!)

Mac

Homebrew로 설치:

brew install mysql


서비스 시작:

brew services start mysql


root 계정 보안 설정 실행:

mysql_secure_installation


VALIDATE PASSWORD: n (비밀번호 규칙 강제 안 함, 팀 작업 편리함)

root 비밀번호 새로 입력 → 꼭 기억하기

익명 사용자 삭제: y

원격 root 접속 차단: y

test DB 삭제: y

권한 새로고침: y

2. MySQL 접속
mysql -u root -p


👉 위 명령 실행 후 비밀번호 입력하면 MySQL 콘솔 접속됨.

3. 데이터베이스 생성
CREATE DATABASE dmu CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;

4. SQL 파일 불러오기

(같은 폴더에 dmu.sql 파일이 있다고 가정)

Windows (PowerShell / CMD)
mysql -u root -p dmu < dmu.sql

Mac (터미널)
mysql -u root -p dmu < dmu.sql

5. 확인

MySQL 접속 후:

USE dmu;
SHOW TABLES;


👉 테이블이 보이면 성공 ✅

6. 팀원 공통 주의사항

root 비밀번호를 잊지 말 것

DB 이름은 반드시 dmu

SQL 불러오기 전에 DB가 생성돼 있어야 함

dmu.sql 파일은 최신 버전을 공유해서 동일하게 맞출 것

좋아요! 그러면 아까 만든 체크리스트를 깔끔하게 **README.md** 형태로 정리해드릴게요. 바로 깃허브에 올려서 처음 보는 사람도 따라 할 수 있도록 할 수 있습니다.

---

````markdown
# Yangdong Project

> FastAPI 기반 RAG + TTS 프로젝트 실행 가이드

---

## 1. 환경 준비

### 1.1 Miniconda 설치
[Miniconda 설치 링크](https://docs.conda.io/en/latest/miniconda.html)에서 OS에 맞는 버전 설치

### 1.2 환경 생성 및 활성화
```bash
cd <프로젝트_폴더>
conda create -n rag python=3.11 -y
conda activate rag
````

### 1.3 패키지 설치

```bash
conda install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia
pip install -r requirements.txt
pip install faster-whisper mysql-connector-python uvicorn
```

---

## 2. Git 초기화 및 충돌 해결

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/USERNAME/REPO.git
git pull origin main --allow-unrelated-histories
git push -u origin main
```

> ⚠️ 이미 원격에 파일이 있으면 `pull` 후 충돌 해결 필요

---

## 3. Git 머지 마커 제거

충돌 마커(`<<<<<<< HEAD`) 확인:

```bash
findstr /s /n "<<<<<<<" *.py
```

발견 시 삭제 후 코드 수정

---

## 4. OpenMP 라이브러리 충돌 해결

```bash
set KMP_DUPLICATE_LIB_OK=TRUE
```

혹은 코드 내:

```python
import os
os.environ["KMP_DUPLICATE_LIB_OK"] = "TRUE"
```

---

## 5. FastAPI 서버 실행 전 확인

1. 필수 모듈 설치

```bash
pip install faster-whisper mysql-connector-python uvicorn
```

2. DB 연결 확인 (`mysql.connector` 사용)

   * MySQL 서버 실행
   * `.env` 또는 `config.py` 확인

---

## 6. 서버 실행

```bash
uvicorn app:app --reload
```

* 기본 주소: `http://127.0.0.1:9000`

---

## 7. 주요 오류와 해결

| 오류                       | 원인              | 해결 방법                                |
| ------------------------ | --------------- | ------------------------------------ |
| 401 Unauthorized         | 인증 토큰 없음        | 테스트 계정 생성 / 헤더에 토큰 추가                |
| 404 Not Found            | 요청 URL에 파일 없음   | `feature.html` 경로 확인, `static` 경로 설정 |
| 502 Bad Gateway          | 백엔드 처리 실패       | 모델 서버 실행 확인, 로그 확인                   |
| 422 Unprocessable Entity | 요청 데이터 구조 오류    | FastAPI Pydantic 모델 확인, 필드 누락 여부 확인  |
| ModuleNotFoundError      | 패키지 설치 안 됨      | `pip install 패키지명`                   |
| OMP Error                | OpenMP 라이브러리 중복 | `set KMP_DUPLICATE_LIB_OK=TRUE`      |

---

## 8. 테스트

### RAG 챗 테스트

```bash
curl -X POST "http://127.0.0.1:9000/rag/chat" \
-H "Content-Type: application/json" \
-d "{\"question\": \"안녕하세요\"}"
```

### TTS 테스트

```bash
curl -X POST "http://127.0.0.1:9000/tts" \
-H "Content-Type: application/json" \
-d "{\"text\": \"안녕하세요\"}"
```

> 출력이 정상적으로 나오면 서버 준비 완료

```


