# 고객 안내 콘텐츠 프로젝트

이 저장소는 운영팀이 고객 안내 HTML과 정적 리소스를 관리하는 원본 저장소다. 챗봇 저장소는 이 저장소의 `manuals/` 디렉터리를 그대로 복제하여 서비스하며, FAQ·RAG 연동 로직은 챗봇 개발팀이 별도로 관리한다.

## 작업 원칙

- 작업을 시작하기 전에 대상 파일과 연결된 이미지, PDF, 링크를 먼저 확인한다.
- 요청 범위에 포함된 파일만 최소한으로 수정한다.
- 기존 HTML 파일명은 외부 연동 계약이므로 변경하지 않는다.
- `manuals/`만 복사해도 모든 로컬 링크가 동작하도록 구성한다.
- 검증 실패나 정책 충돌을 숨기지 말고 정확한 파일과 원인을 보고한다.
- 관련 없는 사용자 변경 사항을 되돌리거나 덮어쓰지 않는다.
- 명시적 요청 없이 파일 삭제, Git 커밋, 푸시를 수행하지 않는다.

## 소유권 경계

운영팀이 관리하는 범위:

- 고객 안내 HTML 본문과 화면 구성
- 이미지·GIF 등 정적 리소스
- 고객 배포용 PDF
- `content-manifest.json`의 콘텐츠 메타데이터와 버전

챗봇 개발팀이 관리하는 범위:

- `faq_code`, 제품, 조건과 HTML 파일의 매핑
- RAG 검색 및 top-1 판정 로직
- 응답에 매뉴얼 링크를 강제로 포함하는 로직
- 정적 파일 서비스 URL과 배포 설정

운영팀 저장소에는 FAQ 코드, RAG 프롬프트, 검색 조건 또는 챗봇 분기 로직을 넣지 않는다.

## 표준 파일 구조

```text
manuals/
├── leaf-customer-guide.html
├── caps-customer-guide.html
├── holiday-setting-guide.html
├── smart-reservation-add-schedule-guide.html
├── smart-reservation-edit-schedule-guide.html
├── smart-reservation-duplicate-check-guide.html
├── assets/
│   ├── *.jpg
│   ├── *.png
│   └── *.gif
├── files/
│   ├── leaf-app-manual.pdf
│   └── caps-app-manual.pdf
└── content-manifest.json
```

기존 HTML 6개의 역할:

- `leaf-customer-guide.html`: 리프 제품의 연결 해제, Wi-Fi 변경, 허브 전원 재연결 안내
- `caps-customer-guide.html`: 캡스 제품의 연결 해제, Wi-Fi 변경, 허브 전원 재연결 안내
- `holiday-setting-guide.html`: 휴일 설정 안내
- `smart-reservation-add-schedule-guide.html`: 스마트 예약 일정 추가 안내
- `smart-reservation-edit-schedule-guide.html`: 스마트 예약 일정 수정 안내
- `smart-reservation-duplicate-check-guide.html`: 스마트 예약 중복 확인 안내

## 파일 및 링크 계약

- HTML 파일명은 유지한다. 이름 변경이 필요하면 먼저 챗봇 개발팀과 영향 범위를 합의한다.
- HTML에서 이미지는 `assets/<파일명>` 형태의 상대 경로로 참조한다.
- HTML에서 PDF는 `files/<파일명>` 형태의 상대 경로로 참조한다.
- HTML 간 이동은 같은 디렉터리를 기준으로 상대 경로를 사용한다.
- `manuals/` 바깥의 로컬 파일을 참조하지 않는다.
- 외부 URL과 `tel:` 링크는 요청 없이 변경하지 않는다.
- 인라인 CSS와 JavaScript를 외부 파일로 분리하는 구조 변경은 별도 요청이 있을 때만 수행한다.

## `content-manifest.json` 계약

매니페스트는 챗봇 매핑 규칙이 아니라 운영 콘텐츠의 배포 목록이다. 최소한 다음 정보를 관리한다.

```json
{
  "version": "1.0.0",
  "entrypoints": [
    "leaf-customer-guide.html",
    "caps-customer-guide.html",
    "holiday-setting-guide.html",
    "smart-reservation-add-schedule-guide.html",
    "smart-reservation-edit-schedule-guide.html",
    "smart-reservation-duplicate-check-guide.html"
  ]
}
```

버전 규칙:

- 기존 콘텐츠 문구·이미지·스타일 수정: patch 증가
- 새 HTML 진입점 추가: minor 증가
- 기존 경로나 파일 계약을 깨는 변경: major 증가 및 챗봇 개발팀 사전 협의

## 작업 하네스

### 기존 콘텐츠 수정

1. 요청된 HTML과 연결된 정적 파일을 확인한다.
2. 고객에게 보이는 결과와 변경 범위를 짧게 정리한다.
3. 대상 파일만 수정하고 기존 파일명과 상대 경로를 유지한다.
4. `content-manifest.json`의 patch 버전을 증가시킨다.
5. 아래 자동 검증과 브라우저 검증을 수행한다.
6. 변경 파일, 검증 결과, 남은 주의사항을 보고한다.

### 새 콘텐츠 추가

1. 기존 6개 HTML과 목적이 중복되는지 먼저 확인한다.
2. 신규 파일명은 영문 소문자 kebab-case로 정한다.
3. 이미지와 PDF는 각각 `assets/`, `files/`에 둔다.
4. 신규 HTML을 `manuals/` 바로 아래에 추가한다.
5. `content-manifest.json`의 `entrypoints`에 추가하고 minor 버전을 증가시킨다.
6. 자동 검증과 브라우저 검증 후 챗봇 개발팀에 파일명과 적용 제품을 전달한다.

### 폴더 구조 변경

1. 현재 파일과 모든 로컬 참조 경로를 목록화한다.
2. 이동 계획과 변경될 링크를 먼저 제시한다.
3. `manuals/` 내부로 파일을 이동하고 상대 경로를 함께 수정한다.
4. 매니페스트와 실제 진입점이 일치하는지 확인한다.
5. 모든 HTML을 브라우저에서 열어 링크와 화면을 검증한다.

## 검증 명령어

저장소 루트에서 실행한다.

```bash
# 배포 대상 파일 확인
find manuals -type f | sort

# 매니페스트 JSON 문법 확인
python3 -m json.tool manuals/content-manifest.json >/dev/null

# 로컬 미리보기 서버
python3 -m http.server 8000 --directory manuals
```

미리보기 서버 실행 후 `http://localhost:8000/<HTML 파일명>`에서 각 진입점을 확인한다.

완료 전 필수 확인 사항:

- `content-manifest.json`의 모든 `entrypoints`가 실제 파일로 존재하는가
- HTML에서 참조하는 이미지, GIF, PDF, 내부 HTML이 모두 존재하는가
- 로컬 참조가 `manuals/` 바깥을 가리키지 않는가
- 데스크톱과 모바일 너비에서 주요 문구와 버튼이 잘리지 않는가
- 뒤로 가기, 탭, 모달, PDF 열기, 전화 및 외부 링크가 의도대로 동작하는가
- 기존 6개 HTML 파일명이 유지되는가

검증 도구나 스크립트가 저장소에 추가되면 수동 확인보다 해당 도구를 우선 사용한다. 실패한 검증이 있으면 작업을 완료로 보고하지 않는다.

## 고객 안내 안전 규칙

- 리프와 캡스 안내를 섞지 않는다.
- 고객이 직접 수행하는 전원 차단, 케이블 분리, 초기화 같은 고영향 절차는 운영팀이 승인한 원문만 사용한다.
- 기존 HTML과 챗봇 정책이 충돌하면 임의로 한쪽에 맞추지 않는다. 충돌하는 파일과 문구를 운영 책임자 및 챗봇 개발팀에 함께 보고한다.
- 특히 공유기 재부팅 금지 정책과 HTML의 전원·케이블 재연결 안내가 충돌할 수 있으므로 변경 전에 반드시 확인한다.
- API 키, 토큰, 비밀번호, 고객 개인정보 또는 내부 시스템 주소를 공개 콘텐츠에 넣지 않는다.

## Git 작업 규칙

- 작업 전후 `git status --short`로 변경 범위를 확인한다.
- 관련 없는 변경 파일은 수정하거나 정리하지 않는다.
- 커밋 또는 푸시는 사용자가 명시적으로 요청한 경우에만 수행한다.
- 커밋 또는 푸시 요청을 받으면 현재 브랜치를 먼저 제시하고 사용자 확인 후 진행한다.

### Git 가드

- 커밋하기 전에 항상 `git fetch origin`으로 원격 상태를 확인하고, `main`이 origin보다 뒤처져 있으면 먼저 `git pull`로 최신화한다.
- pull 결과 충돌이 발생하면 임의로 해결하지 말고 충돌 파일과 내용을 사용자에게 보고한다.
- `--force`/`--force-with-lease` 푸시, `git reset --hard`, `git clean`, `--no-verify` 등 되돌리기 어렵거나 훅을 건너뛰는 명령은 사용자가 명시적으로 요청하지 않는 한 사용하지 않는다.
- `main` 브랜치에는 직접 강제 푸시하지 않는다.
- 푸시 직전에는 반영될 커밋 목록(`git log origin/main..HEAD`)을 사용자에게 보여주고 최종 확인을 받는다.

## 완료 보고 형식

```text
- 작업 유형: 기존 수정 | 신규 추가 | 구조 변경
- 변경 파일:
  - <파일 경로>
- 매니페스트 버전: <이전> → <이후>
- 검증 결과:
  - JSON 문법: 통과 | 실패
  - 로컬 참조: 통과 | 실패
  - 브라우저 확인: 통과 | 실패
- 챗봇팀 전달 사항:
  - <신규/변경 HTML 파일명과 적용 제품>
- 주의사항:
  - <정책 충돌 또는 미확인 항목, 없으면 없음>
```
