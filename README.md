# TravelHelper

MCP 기반 여행 플래너 에이전트

## 기술 스택

- **Backend**: Django + Django REST Framework
- **Frontend**: React + Vite (TypeScript)
- **DB**: PostgreSQL (예정)

## 사전 요구사항

- Python 3.9+
- Node.js 22+

## 개발 환경 세팅

### Backend

```bash
cd backend

# 1. 가상환경 생성 (이 프로젝트 전용 파이썬 공간)
python3 -m venv venv

# 2. 가상환경 켜기 (앞에 (venv) 표시가 뜸)
source venv/bin/activate          # Windows: venv\Scripts\activate

# 3. 패키지 설치 (requirements.txt에 적힌 라이브러리 전부 설치)
pip install -r requirements.txt

# 4. DB 테이블 생성 (Django 기본 테이블 만들기)
python manage.py migrate

# 5. 개발 서버 실행
python manage.py runserver
```
→ http://127.0.0.1:8000

### Frontend

```bash
cd frontend

# 1. 패키지 설치 (package.json에 적힌 라이브러리 전부 설치)
npm install

# 2. 개발 서버 실행
npm run dev
```
→ http://localhost:5173

## 패키지 추가 시 규칙

새 라이브러리를 설치하면 목록을 갱신하고 커밋해야 팀원도 받을 수 있습니다.

### Backend (수동 갱신 필요)

```bash
pip install 패키지이름
pip freeze > requirements.txt    # 목록 다시 뽑기 (꼭!)
```
→ 변경된 requirements.txt를 commit

### Frontend (자동 갱신)

```bash
npm install 패키지이름           # package.json이 자동으로 갱신됨
```
→ 변경된 package.json을 commit

## 폴더 구조

```
TravelHelper/
├── backend/     # Django + DRF
└── frontend/    # React + Vite
```