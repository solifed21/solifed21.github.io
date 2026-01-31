# So Life Dev Log — Plan

## 1) PRD 요약
- 목표: 개발 기록/프로젝트 로그용 개인 블로그
- 플랫폼: GitHub Pages
- 스택: Astro (SSG)
- 톤: 음슴체
- 디자인: 개발자 스타일 + 다크그레이/시안 + Pretendard
- 슬로건: 재밋는걸 만드는게 목적
- 로고: 이니셜 S

## 2) 정보구조(IA)
- `/` 홈 (최신 글 카드)
- `/posts/` 전체 글
- `/posts/[slug]/` 글 상세
- `/categories/` 카테고리
- `/categories/[category]/` 카테고리별
- `/tags/[tag]/` 태그별
- `/search/` 검색
- `/about/` 소개
- `/rss.xml` RSS

## 3) 핵심 기능
### MVP
- 카드형 목록 + 태그 뱃지
- 글 상세 (Markdown, Notion 느낌)
- 카테고리/태그 페이지
- 다크모드 토글
- 반응형 (모바일 1열/데스크탑 2열)
- RSS 생성
- GitHub Actions 배포

### Nice-to-have
- 검색(Pagefind)
- Giscus 댓글
- TOC 자동 생성
- OG 이미지/읽기시간
- 이전/다음 글 네비
- 404 커스텀

## 4) 기술 스택
- Astro + Markdown(Content Collections)
- 스타일: Tailwind 또는 vanilla CSS
- 검색: Pagefind (빌드타임)
- 댓글: Giscus
- 배포: GitHub Pages + Actions

## 5) 구현 단계
1) Astro 프로젝트 초기화
2) 레이아웃/테마 구축
3) 글 목록/상세/카테고리/태그
4) 다크모드 + 반응형
5) RSS + 배포 설정
6) (선택) 검색/댓글/고급 기능
