# GitHub Codespace + Claude Code 시작 가이드

이 가이드는 **GitHub Codespace**로 프로젝트를 열고, 서버를 구동하고, **Claude Code(CLI)**를 연동하는 방법을 단계별로 안내합니다.
로컬에 아무것도 설치하지 않아도 브라우저만으로 개발 환경을 만들 수 있습니다.

---

## 목차

1. [사전 준비사항](#1-사전-준비사항)
2. [Codespace 만들기](#2-codespace-만들기)
3. [프로젝트 구동하기](#3-프로젝트-구동하기)
4. [Claude Code 설치 및 연동](#4-claude-code-설치-및-연동)
5. [Claude Code 기본 사용법](#5-claude-code-기본-사용법)
6. [자주 묻는 질문(FAQ)](#6-자주-묻는-질문faq)

---

## 1. 사전 준비사항

아래 두 가지만 있으면 됩니다. 로컬 PC에 Python이나 Node.js를 설치할 필요가 없습니다.

| 항목 | 설명 |
|------|------|
| **GitHub 계정** | [github.com](https://github.com) 에서 무료 가입 |
| **Anthropic API Key** | [console.anthropic.com](https://console.anthropic.com) 에서 발급 (Claude Code 사용 시 필요) |

> **참고**: GitHub 무료 계정에도 매월 Codespace 무료 사용 시간(120 core-hours)이 포함되어 있습니다.

---

## 2. Codespace 만들기

### 2-1. GitHub 저장소 접속

1. 웹 브라우저에서 프로젝트의 **GitHub 저장소 페이지**로 이동합니다.

### 2-2. Codespace 생성

1. 저장소 페이지 오른쪽 위의 초록색 **`<> Code`** 버튼을 클릭합니다.
2. 탭이 두 개 보입니다. **`Codespaces`** 탭을 클릭합니다.
3. **`Create codespace on main`** 버튼을 클릭합니다.

```
[<> Code 버튼]
  ├── Local        ← 이거 아님
  └── Codespaces   ← 이것을 클릭
       └── + Create codespace on main  ← 클릭!
```

4. 잠시 기다리면 **브라우저 안에 VS Code와 동일한 편집기**가 열립니다.
5. 화면 하단에 **터미널(Terminal)**이 자동으로 열려 있습니다. 여기에 명령어를 입력하면 됩니다.

> **팁**: 터미널이 보이지 않으면 상단 메뉴에서 **Terminal > New Terminal** 을 클릭하세요.

---

## 3. 프로젝트 구동하기

Codespace 터미널이 열렸다면, 아래 명령어를 순서대로 입력합니다.

### 3-1. 백엔드(Backend) 설정 및 실행

터미널에 아래 명령어를 **한 줄씩** 복사해서 붙여넣고 Enter를 누르세요.

```bash
# 1. Python 가상환경 생성
python -m venv .venv

# 2. 가상환경 활성화 (Codespace는 Linux이므로 아래 명령어 사용)
source .venv/bin/activate

# 3. 필요한 라이브러리 설치
pip install -r requirements.txt

# 4. 환경 변수 파일 복사
cp .env.example .env
```

> **중요**: `.env` 파일을 열어서 `DATABASE_URL`을 실제 Supabase 연결 주소로 수정해야 합니다.
> Supabase 설정 방법은 [BEGINNER_QUICK_START.md](./BEGINNER_QUICK_START.md)의 "Supabase 데이터베이스 연결" 섹션을 참고하세요.

```bash
# 5. 백엔드 서버 실행
python -m server.main
```

* 성공하면 터미널에 `Uvicorn running on http://0.0.0.0:8000` 같은 메시지가 나옵니다.
* Codespace가 자동으로 **포트 포워딩** 팝업을 띄워줍니다. **"Open in Browser"** 를 클릭하면 API 문서를 확인할 수 있습니다.

### 3-2. 프론트엔드(Frontend) 설정 및 실행

백엔드 서버는 실행 중인 채로 두고, **새 터미널**을 엽니다.

* 상단 메뉴: **Terminal > New Terminal**
* 또는 단축키: `` Ctrl + Shift + ` ``

```bash
# 1. client 폴더로 이동
cd client

# 2. 패키지 설치
npm install

# 3. 프론트엔드 서버 실행
npm run dev
```

* 성공하면 `http://localhost:3000` 에서 실행 중이라는 메시지가 나옵니다.
* 마찬가지로 **포트 포워딩 팝업**이 뜨면 "Open in Browser"를 클릭합니다.

### 3-3. 동작 확인

| 서비스 | 확인 방법 | 기대 결과 |
|--------|-----------|-----------|
| 백엔드 API 문서 | 포트 8000 URL 뒤에 `/docs` 추가 | Swagger UI 화면 |
| 백엔드 상태 점검 | 포트 8000 URL 뒤에 `/core/health` 추가 | `"ok"` 응답 |
| 프론트엔드 | 포트 3000 URL 접속 | 메인 화면 |

> **포트 확인 방법**: 하단 패널에서 **PORTS** 탭을 클릭하면 현재 포워딩된 포트 목록과 접속 URL을 확인할 수 있습니다.

---

## 4. Claude Code 설치 및 연동

Claude Code는 터미널에서 직접 Claude AI와 대화하며 코드를 작성할 수 있는 CLI 도구입니다.

### 4-1. Claude Code 설치

Codespace 터미널에서 아래 명령어를 실행합니다.

```bash
# Claude Code 설치 (npm을 통해 전역 설치)
npm install -g @anthropic-ai/claude-code
```

### 4-2. API Key 설정

```bash
# Anthropic API Key 환경변수 설정
export ANTHROPIC_API_KEY="여기에_본인의_API_KEY를_붙여넣으세요"
```

> **API Key 발급 방법**:
> 1. [console.anthropic.com](https://console.anthropic.com) 접속 및 로그인
> 2. 왼쪽 메뉴에서 **API Keys** 클릭
> 3. **Create Key** 버튼 클릭
> 4. 생성된 키를 복사 (한 번만 보여주므로 반드시 저장)

> **주의**: API Key는 `sk-ant-` 로 시작하는 긴 문자열입니다. 따옴표 안에 정확히 붙여넣으세요.

### 4-3. Claude Code 실행

```bash
# 프로젝트 루트 폴더에서 실행
claude
```

실행하면 아래와 같은 대화형 화면이 나타납니다.

```
╭──────────────────────────────────────╮
│ Welcome to Claude Code!              │
│                                      │
│ /help for help                       │
╰──────────────────────────────────────╯

>
```

`>` 프롬프트에 자연어로 원하는 작업을 입력하면 됩니다.

---

## 5. Claude Code 기본 사용법

### 5-1. 코드 질문하기

```
> 이 프로젝트의 백엔드 아키텍처를 설명해줘
```

```
> server/app/core/database.py 파일이 하는 일을 알려줘
```

### 5-2. 코드 수정 요청하기

```
> sample 도메인의 목록 조회 API에 페이지네이션을 추가해줘
```

```
> client/src/domains/sample 페이지에 검색 기능을 추가해줘
```

### 5-3. 유용한 슬래시 명령어

| 명령어 | 설명 |
|--------|------|
| `/help` | 도움말 보기 |
| `/clear` | 대화 기록 초기화 |
| `/compact` | 대화 내용을 요약하여 컨텍스트 절약 |

### 5-4. 작업 흐름 예시

```
# 1. Claude Code 실행
claude

# 2. 프로젝트 파악
> 이 프로젝트의 전체 구조를 알려줘

# 3. 기능 구현 요청
> user 도메인을 새로 만들어줘. CRUD API와 프론트엔드 페이지 모두 필요해

# 4. 결과 확인 후 서버 재시작
# (Claude Code가 파일을 수정하면 서버를 재시작해서 확인)
```

---

## 6. 자주 묻는 질문(FAQ)

### Q. Codespace를 종료했다가 다시 열 수 있나요?

**네.** GitHub 저장소 페이지에서 `<> Code` > `Codespaces` 탭을 보면 이전에 만든 Codespace 목록이 나옵니다. 클릭하면 이전 상태 그대로 다시 열립니다.

### Q. Codespace에서 변경한 코드는 어떻게 저장하나요?

Codespace 안에서 **Git**을 사용합니다. 터미널에서 아래 명령어를 실행하세요.

```bash
git add .
git commit -m "작업 내용 설명"
git push
```

또는 왼쪽 사이드바의 **Source Control** 아이콘을 클릭해서 GUI로 커밋할 수도 있습니다.

### Q. 포트 포워딩 팝업이 안 뜨면 어떻게 하나요?

하단 패널의 **PORTS** 탭을 클릭합니다. 포트 목록에서 원하는 포트의 **Local Address** 링크를 클릭하면 브라우저에서 열 수 있습니다. 포트가 목록에 없으면 **Add Port** 버튼으로 수동 추가할 수 있습니다.

### Q. Claude Code에서 `ANTHROPIC_API_KEY` 오류가 나요.

터미널을 새로 열면 환경변수가 초기화됩니다. 매번 `export ANTHROPIC_API_KEY="..."` 를 다시 입력하거나, 아래처럼 `.bashrc`에 추가하세요.

```bash
echo 'export ANTHROPIC_API_KEY="여기에_본인의_API_KEY"' >> ~/.bashrc
source ~/.bashrc
```

### Q. Codespace 무료 사용 시간이 초과되면?

GitHub 무료 계정은 월 120 core-hours를 제공합니다 (2-core 머신 기준 약 60시간). 사용하지 않을 때는 Codespace를 **Stop** 해두면 시간이 소모되지 않습니다. 저장소 페이지 > `<> Code` > `Codespaces` 에서 `...` 메뉴 > **Stop codespace** 를 클릭하세요.

### Q. Codespace 대신 로컬에서 개발하고 싶어요.

[BEGINNER_QUICK_START.md](./BEGINNER_QUICK_START.md) 문서를 참고하세요. 로컬 환경(Windows/Mac)에서의 설정 방법이 안내되어 있습니다.

---

## 요약: 전체 흐름 한눈에 보기

```
GitHub 저장소 접속
       │
       ▼
<> Code 버튼 > Codespaces > Create codespace on main
       │
       ▼
  Codespace 열림 (브라우저에서 VS Code 실행)
       │
       ├── [터미널 1] 백엔드 실행
       │     python -m venv .venv
       │     source .venv/bin/activate
       │     pip install -r requirements.txt
       │     cp .env.example .env  →  .env 수정
       │     python -m server.main
       │
       ├── [터미널 2] 프론트엔드 실행
       │     cd client
       │     npm install
       │     npm run dev
       │
       └── [터미널 3] Claude Code 실행
             npm install -g @anthropic-ai/claude-code
             export ANTHROPIC_API_KEY="sk-ant-..."
             claude
```
