좋아요! 그럼 지금까지 우리가 한 과정을 바탕으로 **Windows 기준, 바로 따라할 수 있는 README 스타일 튜토리얼**을 만들어 드릴게요. 코드 블록과 주석까지 넣어서 그대로 복사해서 실행할 수 있도록 했습니다.

---

# 🛠 STT-TTS 샘플 설치 및 실행 가이드 (Windows)

## 1️⃣ 준비물

* **Python 3.11 이상**
* **Conda** (Miniconda 또는 Anaconda)
* 인터넷 연결

---

## 2️⃣ 프로젝트 다운로드

```bash
# 바탕화면에 프로젝트 폴더 생성 후 이동
cd yangdong

# 프로젝트 깃에서 클론
git clone <프로젝트_깃_주소> stt-tts-sample

# 프로젝트 폴더로 이동
cd stt-tts-sample
```

> 이미 다운로드되어 있으면 이 단계는 건너뛰세요.

---

## 3️⃣ Conda 환경 생성

```bash
# 환경 생성
conda env create -f environment.yml

# 환경 활성화
conda activate rag
```

> ⚠️ `pytorch=2.9.0` 등 일부 패키지가 없으면 다음 단계에서 수동 설치 필요.

---

## 4️⃣ PyTorch 및 CUDA 설치 (수동)

```bash
# CUDA 12.1 기준 설치
conda install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia
```

> 설치 완료 후 GPU 환경에서 PyTorch를 사용할 수 있습니다.

---

## 5️⃣ OpenMP 충돌 문제 해결

```bash
# OpenMP 충돌 방지
set KMP_DUPLICATE_LIB_OK=TRUE
```

> 테스트용 임시 해결 방법입니다. 안전하지 않을 수 있음.

---

## 6️⃣ 추가 필수 패키지 설치

```bash
# Whisper 모델, MySQL 커넥터 설치
pip install faster-whisper mysql-connector-python

# 만약 requirements.txt가 있다면
pip install -r requirements.txt
```

---

## 7️⃣ Git 머지 충돌 확인 및 수정

```bash
# 머지 충돌 표식 찾기
findstr /s /n /c:"<<<<<" app.py
```

* 발견 시 해당 라인 삭제 또는 적절히 수정
* 예: `<<<<<<< HEAD`, `=======`, `>>>>>>> branch-name`
* 파일 저장 후 다시 실행

---

## 8️⃣ 앱 실행

```bash
# 환경 활성화 확인 후 실행
conda activate rag
python app.py
```

> FastAPI 사용 시 `DeprecationWarning` 경고가 뜰 수 있으나 실행에는 영향 없음.

---

## 9️⃣ 브라우저 접속

앱 실행 후 브라우저에서 접속:

```
http://127.0.0.1:8000
```

---

## 🔟 요약 체크리스트

1. Conda 환경 생성 & 활성화
2. PyTorch + CUDA 설치
3. OpenMP 문제 해결
4. 추가 패키지 설치 (`faster-whisper`, `mysql-connector-python`)
5. Git 머지 충돌 수정
6. 앱 실행 및 브라우저 접속

---

원하면 제가 이 README를 **그대로 프로젝트 루트에 넣고 바로 복사해서 쓸 수 있는 `.md` 파일 형태**로 만들어서 드릴 수도 있어요.

그렇게 해드릴까요?
