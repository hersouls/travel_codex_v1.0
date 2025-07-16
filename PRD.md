# Moonwave Travel – 초기 무료 버전 제품 요구사항 문서 (PRD)

## 개요 (Overview)

Moonwave Travel은 Claude Code & Cursor 기반 AI와 Supabase(오픈소스 Postgres 백엔드), 최신 UI/UX(모바일 퍼스트, Tailwind+Radix+Heroicons), 글로벌 표준 데이터 모델을 활용하여 **여행 일정 자동 생성, 맞춤 추천, 직관적 일정 편집, 일정 리포트 공유**를 제공하는 웹 서비스입니다. 본 문서는 무료 버전의 제품 요구사항, 시스템 구조, UX 설계, 데이터/컴포넌트/AI 정책, 품질 기준을 명확히 정의합니다.

## 사용자 가치 제안 (User Value Proposition)

- **즉각적이고 개인화된 일정 생성**: Claude Code & Cursor AI가 입력/대화 기반으로 맞춤 여행 일정을 자동 생성합니다.
- **풍부한 추천과 탐색 지원**: AI가 취향·상황·실시간 데이터(MCP) 기반으로 관광지, 음식점, 활동을 추천합니다.
- **손쉬운 일정 편집 및 커스터마이징**: 카드 기반 UI, Drag&Drop, Swipe, 챗봇 대화로 직관적 일정 관리가 가능합니다.
- **일정 리포트 및 공유**: 일정은 PDF/링크로 내보내고, 지도·상세정보·동선 시각화 등 리포트로 제공합니다.

## 핵심 기능 및 세부 설명 (Key Features & Details)

### 1. AI 여행 일정 자동 생성
- Claude Code & Cursor 기반 LLM이 입력(여행지, 기간, 동행, 선호 등)과 대화 프롬프트를 해석해 일정 초안을 생성합니다.
- Smithery MCP를 통해 실시간 POI, 날씨, 이벤트 등 외부 데이터를 결합합니다.
- 2초 이내 응답, 다중 옵션(여유/집중 등) 제공, Chain-of-Thought 및 함수 호출 기반 구조.

### 2. AI 맞춤 추천 및 동적 일정 조정
- AI가 프로필, 맥락, 실시간 데이터 기반으로 추천지를 카드 UI로 제시합니다.
- 챗봇 대화로 일정 피드백/수정, Empty Slot 자동 추천, 실시간 정보(날씨 등) 반영.

### 3. 일정 편집 및 사용자 커스터마이징
- 카드 Drag&Drop, Swipe, 챗봇 대화로 일정 순서/삭제/추가/부분 수정.
- 변경 이력/Undo, 실시간 UI 반영, AI 보조 편집, 모바일 친화적 UX.

### 4. 일정 결과 리포트 및 공유
- 일정 리포트는 카드/타임라인/지도/상세정보/링크/PDF로 제공.
- 공유, 저장, 지도 동선 시각화, 상세정보(운영시간, 리뷰, 링크 등) 포함.

## 데이터 흐름 및 시스템 구조 (Data Flow & Architecture)

- **입력→AI→MCP→일정생성→프론트 표시→수정→저장**의 흐름.
- **AI 엔진**: Claude Code & Cursor (LLM, Chain-of-Thought, 함수 호출)
- **외부 데이터**: Smithery MCP (POI, 날씨, 이벤트 등)
- **DB/인증/저장소**: Supabase (Postgres, Auth, Storage, 실시간)
- **프론트엔드**: React, Next.js, Tailwind, Radix UI, Heroicons
- **API**: Next.js API Routes, Supabase Functions, JWT 인증
- **보안/개인정보**: Supabase IAM, GDPR/CCPA 준수, SSL/TLS 암호화
- **모든 데이터/파일/리포트는 Supabase에 저장**

## 데이터 모델 및 API 정책 (Data Model & API Policy)

- **데이터 모델**: User, TripCard, TripDetailCard, PersonalizedRecommendation 등 (Supabase 기반)
- **API 명세**: RESTful, JSON, JWT 인증, 500ms 이내 응답, CRUD/추천/생성/수정/삭제 등 명확히 정의
- **보안**: 모든 API는 SSL/TLS, 권한별 접근, JWT 인증, 최소 권한 원칙

## UI/UX 및 컴포넌트 정책 (UI/UX & Component Policy)

- **모바일 퍼스트, 반응형, 카드 기반 UI, Bottom Sheet, 챗봇, FAB, 탭바, 지도, 피드백/로딩 인디케이터**
- **컴포넌트**: Radix UI, Tailwind, Heroicons, 일관된 디자인 시스템, Figma/Storybook 문서화
- **접근성**: WCAG 2.1 AA, 키보드/스크린리더, 고대비, 다국어, RTL, Alt Text, 포커스 관리
- **아이콘**: Heroicons+Radix Icons 병행, 일관된 스타일, 접근성 라벨

## 디자인/성능/품질 기준 (Design/Performance/Quality)

- **디자인**: 심플&클리어, 글로벌 인클루시브, 다크모드, 고대비, 강렬한 포인트컬러(코랄/블루), 오픈소스 폰트(Inter 등)
- **성능**: 2초 이내 로딩, 60fps 스크롤, Lazy-loading, 코드/이미지 최적화, SSR/CDN, 오프라인 지원
- **테스트/모니터링**: Supabase Analytics, A/B테스트, 실사용자 모니터링, 자동화 테스트

## AI Agent 및 자동화 정책 (AI Agent & Automation)

- **AI Agent**: Claude Code & Cursor 기반, 실시간 일정 생성/추천/오류감지/콘텐츠 큐레이션/리포트 자동화
- **자동화**: 일정 생성/추천/수정/리포트/공유 등 전 과정 자동화, Supabase Functions 활용
- **성능 목표**: 일정 생성/추천 1~2초, 오류감지 1초, 리포트 5초, 글로벌 가용성 99.99%

## 개발 원칙 및 품질 보증 (Development Principles & Quality)

- **SOLID 원칙**: 단일 책임, 개방폐쇄, 리스코프 치환, 인터페이스 분리, 의존 역전
- **유지보수성, 확장성, 재사용성, 테스트 용이성**
- **오류/불일치/구버전/불필요한 내용 삭제 및 최신화**

## 부록: 참고 자료 (References)

- UI/컴포넌트/디자인: UI컴포넌트 정의서, 디자인가이드, 화면정의서
- 데이터/아키텍처: 데이터정의서, API 명세서, Agent명세서
- 개발원칙: Solid.md
- 기타: 최신 글로벌 UI/UX/AI/DB/보안 트렌드