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

## 브랜치 전략

`main`과 `develop`은 보호되어 있어 직접 push할 수 없습니다. 반드시 브랜치를 만들어 PR로 병합합니다.

### 브랜치 구조

- **main**: 배포용. 안정된 코드만 유지
- **develop**: 개발 통합 브랜치. 평소 작업은 여기로 병합
- **feature/**: 각자 기능 작업 브랜치

흐름: `feature/기능` → (PR) → `develop` → (PR) → `main`

### 브랜치 이름 규칙

- `feature/기능이름` — 새 기능 (예: `feature/flight-agent`)
- `fix/버그이름` — 버그 수정 (예: `fix/budget-bug`)

### 작업 흐름

1. 최신 develop 받기: `git checkout develop` 후 `git pull`
2. 새 브랜치 만들기: `git checkout -b feature/기능이름`
3. 작업 후 커밋: `git add .` 후 `git commit -m "작업 내용"`
4. 브랜치 올리기: `git push -u origin feature/기능이름`
5. GitHub에서 develop으로 PR 생성 → 리뷰 → 병합

### 규칙

- PR은 최소 1명의 승인을 받아야 병합됩니다.
- `develop → main` 병합은 배포 시점에 별도 PR로 진행합니다.