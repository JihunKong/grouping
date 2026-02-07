# 🎲 코칭아카데미 팀매칭 설문 — 설정 가이드

GitHub Pages에서 설문을 받고, 응답이 자동으로 구글 스프레드시트에 쌓이는 구조입니다.

---

## 📌 전체 흐름

```
선생님이 설문 작성 (GitHub Pages)
        ↓
Google Apps Script가 데이터 수신
        ↓
구글 스프레드시트에 자동 저장
        ↓
CSV 다운로드 → Claude에게 그룹 매칭 요청
```

---

## 1단계: 구글 스프레드시트 만들기

1. [Google Sheets](https://sheets.google.com) 접속
2. **빈 스프레드시트** 새로 만들기
3. 이름을 `코칭아카데미 팀매칭 설문` 등으로 변경
4. 시트 이름은 기본값(`시트1`) 그대로 두면 됩니다

---

## 2단계: Google Apps Script 설정

1. 스프레드시트 상단 메뉴 → **확장 프로그램** → **Apps Script** 클릭
2. 기존 코드를 모두 지우고, `google_apps_script.js` 파일 내용을 붙여넣기
3. **저장** (Ctrl+S)
4. 배포하기:
   - 우측 상단 **배포** → **새 배포**
   - ⚙️ 유형 선택 → **웹 앱**
   - 설정:
     - 설명: `코칭아카데미 설문`
     - 실행 계정: **나(본인 이메일)**
     - 액세스 권한: **모든 사용자** ⬅️ 중요!
   - **배포** 클릭
5. **웹 앱 URL** 이 나타나면 복사 → 이 URL이 핵심입니다!
   - 예: `https://script.google.com/macros/s/AKfycb.../exec`

---

## 3단계: HTML에 URL 연결

`index.html` 파일을 열고 상단의 이 부분을 찾아 URL을 교체합니다:

```javascript
const GOOGLE_SCRIPT_URL = 'YOUR_GOOGLE_APPS_SCRIPT_URL_HERE';
```

↓ 이렇게 변경:

```javascript
const GOOGLE_SCRIPT_URL = 'https://script.google.com/macros/s/AKfycb.../exec';
```

그리고 `<div class="setup-notice" ...>` 블록은 삭제해도 됩니다 (관리자 안내 박스).

---

## 4단계: GitHub Pages에 배포

### 방법 A — GitHub 웹에서 직접 업로드

1. [github.com](https://github.com) 로그인
2. **New repository** 생성 (예: `coaching-survey`)
   - Public으로 설정
3. **Add file** → **Upload files** → `index.html` 업로드
4. **Settings** → **Pages** → Source를 **main** 브랜치로 선택 → **Save**
5. 1~2분 후 URL 생성됨:
   - `https://[사용자명].github.io/coaching-survey/`

### 방법 B — Git 명령어 사용

```bash
git init
git add index.html
git commit -m "코칭아카데미 팀매칭 설문"
git branch -M main
git remote add origin https://github.com/[사용자명]/coaching-survey.git
git push -u origin main
```

---

## 5단계: 테스트

1. GitHub Pages URL로 접속
2. 설문 작성 후 제출
3. 구글 스프레드시트에 데이터가 들어오는지 확인

---

## 6단계: 선생님들에게 공유

설문 URL을 패들렛이나 카카오톡으로 공유하면 됩니다.
19명 모두 응답하면 스프레드시트에서 CSV 다운로드 후 Claude에게 전달하면 그룹 매칭을 해드립니다.

---

## ❓ 문제 해결

| 증상 | 해결 |
|------|------|
| 제출했는데 시트에 안 쌓임 | Apps Script 배포 시 **액세스 권한: 모든 사용자** 확인 |
| 403 에러 | Apps Script 재배포 (새 배포 → 버전 업데이트) |
| CORS 에러 | `mode: 'no-cors'` 가 이미 적용되어 있으므로 정상입니다 |
| 스프레드시트 헤더 없음 | 첫 번째 응답 제출 시 자동 생성됩니다 |

---

## 📁 파일 구성

```
coaching-survey/
├── index.html              ← GitHub Pages에 업로드 (설문 페이지)
├── google_apps_script.js   ← Apps Script 에디터에 붙여넣기 (배포 안 함)
└── README.md               ← 이 가이드 (선택)
```
