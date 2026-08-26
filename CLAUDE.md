# 클라우드 ISMS 점검 매트릭스

AWS·Azure 클라우드 인프라에 대한 ISMS(-P) 인증심사 준비용 참고 자료. 순수 정적 HTML 한 페이지(빌드 도구 없음)로, GitHub Pages로 배포된다.

- GitHub 저장소: `leeyonsei78/isms-cloud-audit-matrix` (origin, `master` 브랜치)
- 배포: GitHub Pages — `index.html`이 `isms-cloud-audit-matrix.html`로 즉시 리다이렉트
- Live URL: https://leeyonsei78.github.io/isms-cloud-audit-matrix/ (master에 push하면 자동 재배포)

## 파일 구성

- `isms-cloud-audit-matrix.html` — 실제 콘텐츠와 로직이 모두 담긴 단일 파일(HTML+CSS+JS 인라인). 이 프로젝트의 유일한 "프로그램" 파일.
- `index.html` — GitHub Pages 진입점. `<meta http-equiv="refresh">`로 위 파일로 이동만 시킴.
- `README.md` — 사용자용 소개/사용법.
- 빌드 스텝, package.json, 외부 의존성 없음. 브라우저에서 파일을 열면 바로 동작.

## 콘텐츠 구조

ISMS-P 2022 개정 인증기준 영역 2 중 7개 영역(2.5, 2.6, 2.7, 2.9, 2.10, 2.11, 2.12) · 17개 통제항목(`<article class="card" data-id="2.x.x">`)을 다룬다. 각 카드는 AWS/Azure를 나란히 비교하며 항목마다:
- 점검 명령어 (`.field-cmd .code`)
- 실행 예시 및 결과 (`.field-example .code`)
- 문제 판단 기준 (`.field-crit .field-text`)
- 개선 방안 (`.field-fix .field-text`)

모든 텍스트 필드는 `contenteditable="true"` + `data-key`를 가지며, `localStorage`(`ismsCloudMatrix:v1:` 프리픽스)에 자동 저장/복원된다.

## 주요 기능 (스크립트, 파일 하단 `<script>` 블록)

- 검색창 + 영역 필터 칩으로 카드 표시/숨김
- 코드 블록 복사 버튼
- 편집 초기화 버튼(2단계 확인 후 localStorage 편집분 삭제)
- **CSV 내보내기** (`exportBtn` / `buildCSV()`) — 전체 17개 항목을 Excel용 CSV로 다운로드
- **Word 내보내기** (`exportWordBtn` / `buildWordDocument()`) — 전체 17개 항목을 Microsoft Word 호환 `.doc` 파일로 다운로드. Word가 열 수 있는 MHTML 스타일 HTML(`xmlns:w="urn:schemas-microsoft-com:office:word"`)을 생성해 `application/msword` MIME으로 Blob 다운로드한다. CSS 변수·flex/grid는 Word 렌더러가 지원하지 않으므로, 표(`<table>`)와 고정 hex 색상만 사용한 별도 스타일(`w-*` 클래스)로 새로 구성했다 — 화면 표시용 CSS를 그대로 재사용하지 않음.
- 두 내보내기 모두 검색/필터 상태와 무관하게 항상 전체 17개 항목을 포함하며, `window.claude.use('downloads')` 캡셔빌리티가 있으면 그것을 우선 사용하고, 없으면(일반 브라우저) `Blob` + `<a download>`로 폴백한다.

## 새 통제항목/필드 추가 시

카드 마크업을 복사해 새 `<article class="card" data-id="...">`를 추가하고 각 필드에 고유 `data-key`를 부여하면, `buildCSV()`와 `buildWordDocument()`는 DOM을 클래스 셀렉터로 순회하므로 **수정 없이 자동으로 새 항목을 포함**한다.

## 로컬 작업 ↔ GitHub 동기화

```bash
git pull origin master      # GitHub의 최신 내용을 로컬 폴더에 반영
# ... 수정 ...
git add -A
git commit -m "..."
git push origin master      # 로컬 수정을 GitHub에 반영 → GitHub Pages 자동 재배포
```

## 실행 방법

파일 탐색기에서 `isms-cloud-audit-matrix.html`을 더블클릭해 브라우저로 열거나, 배포된 GitHub Pages URL로 접속하면 된다. 별도 서버·빌드 과정 불필요.
