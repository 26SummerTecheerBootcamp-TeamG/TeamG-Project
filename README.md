# TravelHelper

MCP 기반 여행 플래너 에이전트

## 기술 스택

- **Backend**: Django + Django REST Framework
- **Frontend**: React + Vite (TypeScript)
- **DB**: PostgreSQL (예정)
- **비동기 처리**: Celery + RabbitMQ + Redis (예정)

## 사전 요구사항

- Python **3.12 이상**
- Node.js 22+

> Python 버전을 3.12+로 두는 이유: 에이전트 코드(PoC 이식분)가 3.10+ 문법(`str | None` 등)을 사용하고,
> 앞으로 올릴 Django 버전의 공식 지원 범위와 맞추기 위함입니다. 3.9에서는 동작하지 않습니다.

## 개발 환경 세팅

### Backend

```bash
cd backend

# 1. 가상환경 생성 (이 프로젝트 전용 파이썬 공간)
python -m venv venv           # Mac/Linux에서 python이 없다고 나오면: python3 -m venv venv

# 2. 가상환경 켜기 (앞에 (venv) 표시가 뜸)
# Windows (PowerShell)
venv\Scripts\activate
# Mac/Linux
source venv/bin/activate

# 3. 패키지 설치 (requirements.txt에 적힌 라이브러리 전부 설치)
pip install -r requirements.txt

# 4. DB 테이블 생성 (Django 기본 테이블 만들기)
python manage.py migrate

# 5. 개발 서버 실행
python manage.py runserver
```
→ http://127.0.0.1:8000

> **Windows에서 activate가 막힐 때**: "이 시스템에서 스크립트를 실행할 수 없으므로..." 오류가 나면
> PowerShell에서 아래 한 줄을 실행한 뒤 다시 시도하세요. (현재 사용자에게만 스크립트 실행 허용)
> ```powershell
> Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
> ```

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

### Backend (수동 갱신 — requirements.txt에 직접 한 줄 추가)

```bash
pip install 패키지이름

# 설치된 버전 확인
pip show 패키지이름

# requirements.txt에 "직접 설치한 패키지만" 버전과 함께 한 줄 추가
# 예) celery==5.4.0
```
→ 변경된 requirements.txt를 commit

> ⚠️ **`pip freeze > requirements.txt`는 사용하지 않습니다.**
> freeze는 딸려 온 하위 의존성 전부와 OS 전용 패키지(예: Windows에서만 설치되는 것)까지
> 통째로 박제해서, 다른 OS를 쓰는 팀원의 `pip install`이 깨질 수 있습니다.
> 우리가 직접 고른 패키지만 기록하면 하위 의존성은 pip이 알아서 맞춰줍니다.

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
- `docs/문서이름` — 문서 작업 (예: `docs/readme-dev-setup`)

### 작업 흐름

1. 최신 develop 받기: `git checkout develop` 후 `git pull`
2. 새 브랜치 만들기: `git checkout -b feature/기능이름`
3. 작업 후 커밋: `git add .` 후 `git commit -m "작업 내용"`
4. 브랜치 올리기: `git push -u origin feature/기능이름`
5. GitHub에서 develop으로 PR 생성 → 리뷰 → 병합

### 규칙

- PR은 최소 1명의 승인을 받아야 병합됩니다.
- `develop → main` 병합은 배포 시점에 별도 PR로 진행합니다.
- 커밋 전 `git status`로 의도하지 않은 파일(venv, db.sqlite3 등)이 안 딸려가는지 확인합니다.
