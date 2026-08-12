# PRD: 개인 개발 블로그 (Notion CMS 기반)

## 1. 개요

| 항목 | 내용 |
| --- | --- |
| 프로젝트명 | 개인 개발 블로그 |
| 목적 | Notion을 CMS로 활용한 개인 기술 블로그 구축 |
| CMS 선택 이유 | Notion 데이터베이스에 글을 작성하면 별도 배포 작업 없이 블로그에 자동 반영됨 |
| 작성일 | 2026-08-12 |

### 1.1 배경 및 문제 정의
- 기존 블로그 플랫폼(velog, tistory 등)은 콘텐츠 소유권과 커스터마이징에 제약이 있음
- 별도의 CMS 어드민을 직접 구축/운영하는 것은 비용 대비 효율이 낮음
- Notion은 이미 개인 노트/문서 작성 도구로 사용 중이므로, 별도의 글쓰기 툴 학습 없이 바로 콘텐츠를 생산할 수 있음

### 1.2 목표
- Notion 데이터베이스를 단일 진실 공급원(Source of Truth)으로 사용하는 블로그 구축
- 글 작성 후 별도의 빌드/배포 절차 없이(또는 최소한의 재배포로) 콘텐츠가 반영되는 경험 제공
- 카테고리/검색을 통해 방문자가 원하는 글을 빠르게 탐색할 수 있도록 지원

## 2. 주요 기능

| # | 기능 | 설명 |
| --- | --- | --- |
| 1 | 글 목록 조회 | Notion 데이터베이스에서 발행된(Status: 발행됨) 글 목록을 가져와 표시 |
| 2 | 글 상세 페이지 | 개별 글의 본문(Content)을 렌더링하는 상세 페이지 |
| 3 | 카테고리 필터링 | Category(select) 속성 기준으로 글을 필터링하여 조회 |
| 4 | 검색 기능 | 제목/태그 기준 키워드 검색 |
| 5 | 반응형 디자인 | 모바일/태블릿/데스크톱 환경에서 모두 사용 가능한 레이아웃 |

## 3. 기술 스택

| 영역 | 기술 |
| --- | --- |
| Frontend | Next.js 15, TypeScript |
| CMS | Notion API (`@notionhq/client`) |
| Styling | Tailwind CSS, shadcn/ui |
| Icons | Lucide React |
| Deployment | Vercel |

## 4. Notion 데이터베이스 구조

| 속성명 | 타입 | 설명 |
| --- | --- | --- |
| Title | title | 글 제목 |
| Category | select | 카테고리 (예: Frontend, Backend, DevOps 등) |
| Tags | multi_select | 태그 (다중 선택) |
| Published | date | 발행일 |
| Status | select | 상태 (`초안` / `발행됨`) |
| Content | page content | 글 본문 (Notion 페이지 블록) |

> 목록/필터링 API 호출 시 `Status = 발행됨` 조건을 기본 필터로 사용하여 초안이 외부에 노출되지 않도록 한다.

## 5. 화면 구성 (IA)

| 화면 | 경로(예시) | 설명 |
| --- | --- | --- |
| 홈 | `/` | 최근 발행된 글 목록 표시 (최신순) |
| 글 상세 | `/posts/[slug]` | 개별 글의 본문, 카테고리, 태그, 발행일 표시 |
| 카테고리 | `/category/[category]` | 선택한 카테고리에 해당하는 글 목록 표시 |

## 6. MVP 범위

- Notion API 연동 (데이터베이스 조회, 페이지 본문 조회)
- 글 목록 페이지 및 글 상세 페이지
- 기본 스타일링 (Tailwind CSS, shadcn/ui 컴포넌트 활용)
- 반응형 디자인 적용

### MVP 제외 범위 (추후 검토)
- 댓글 기능
- 조회수/좋아요 등 인터랙션 기능
- 다크 모드
- 다국어(i18n) 지원
- 관리자 대시보드 (Notion을 어드민으로 대체하므로 별도 구축하지 않음)

## 7. 구현 단계

1. **환경 설정**: Next.js 15 + TypeScript 프로젝트에 `@notionhq/client` 등 필요 패키지 설치, 환경 변수(`NOTION_API_KEY`, `NOTION_DATABASE_ID`) 설정
2. **Notion 연동 준비**: Notion 데이터베이스 생성(위 4번 구조에 맞춰 속성 구성) 및 Integration 생성 후 API 키 발급, 데이터베이스에 Integration 연결
3. **글 목록 페이지 구현**: 홈 화면에서 Notion 데이터베이스 조회 API를 호출하여 발행된 글 목록 렌더링
4. **글 상세 페이지 구현**: 개별 페이지의 블록(Content) 조회 API를 호출하여 본문 렌더링, 카테고리/검색 기능 구현
5. **스타일링 및 최적화**: Tailwind CSS/shadcn/ui 기반 UI 정리, 반응형 대응, 이미지 최적화 및 성능 점검

## 8. 성공 지표 (참고용)

- Notion에서 글 작성 후 블로그 반영까지 소요 시간
- 초기 페이지 로딩 속도 (Core Web Vitals 기준)
- 모바일 환경에서의 정상 동작 여부

## 9. 리스크 및 고려사항

| 리스크 | 대응 방안 |
| --- | --- |
| Notion API Rate Limit | 캐싱(ISR/재검증 주기 설정) 적용으로 API 호출 빈도 최소화 |
| Notion 블록 → HTML 변환 복잡도 | 텍스트, 이미지, 코드 블록 등 주요 블록 타입 위주로 우선 지원, 이후 점진적 확장 |
| 콘텐츠 실시간 반영 지연 | ISR(Incremental Static Regeneration) 또는 On-demand Revalidation 적용 검토 |
