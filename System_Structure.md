# TogetherChise 시스템 구조 설명서 (System Structure)

본 문서는 **TogetherChise(공공형 프랜차이즈 운영 플랫폼)**의 전체 시스템 구조와 주요 구성 요소를 설명합니다. 2026년 1월 31일 기준, Firebase 단일 백엔드 체제로 최적화되었습니다.

---

## 🏗️ 1. 전체 아키텍처 개요

본 프로젝트는 **Next.js (App Router)**를 기반으로 한 풀스택 웹 애플리케이션이며, 서버리스 아키텍처를 채택하고 있습니다.

- **Frontend:** Next.js 15+, React 19, Tailwind CSS (Glassmorphism UI)
- **Backend (Serverless):** Firebase (Authentication, Firestore NoSQL, Storage)
- **State Management:** React Hooks & Firebase Real-time Sync
- **AI Logic / Defensive Coding:** 
  - 내부 비즈니스 로직을 통한 이상 징후 탐지 (마진율 및 시장가 비교)
  - 모든 외부 연동(Firebase 등)에 **8~10초 타임아웃** 및 네트워크 예외 처리 적용

---

## 📂 2. 디렉토리 구조 및 상세 설명

### 📁 Root Directory
- `0131DevNote.md`: 최신 개발 이력 및 아키텍처 결정 사항 기록.
- `System_Structure.md`: [현재 파일] 시스템 구조에 대한 종합 가이드.
- `Data_Management_Plan.md`: 데이터 기록, 보존 및 프라이버시 관리 계획.
- `DLP_Strategy.md`: 데이터 손실 방지(DLP)를 위한 세부 운영 전략.
- `README.md`: 프로젝트 설치 및 실행 가이드.
- `data/`: 시스템 설계 및 데이터 모델 관련 문서 보관.

### 📁 frontend/ (메인 애플리케이션 코드)

#### 📁 `src/app/` (Next.js App Router)
- **`Layout.tsx`**: 시스템 전체의 레이아웃 (수화 에러 방지 및 폰트 설정).
- **`dashboard/`**: [체크 ✅] 점주용 대시보드 페이지. 실시간 원가 통계 및 AI 탐지 결과 노출.
- **`admin/seed/`**: [체크 ✅] 관리자용 데이터 초기화 툴. Batch 작업을 통한 샘플 데이터 적재.
- **`globals.css`**: Tailwind CSS 및 글로벌 글래스모피즘 스타일 정의.

#### 📁 `src/services/` (비즈니스 로직 layer)
- **`auth.ts`**: [체크 ✅] Firebase Auth 연동. 이메일/비밀번호 가입(상세 업체 정보 포함) 및 구글 소셜 로그인 지원.
- **`data.ts`**: [체크 ✅] Firestore 연동. 품목 리스트 조회 및 **AI 이상 탐지(Anomaly Detection)** 알고리즘 포함. 타임아웃 방어 코드 적용.

#### 📁 `src/lib/` (설정 및 유틸리티)
- **`firebase.ts`**: [체크 ✅] Firebase SDK 초기화 및 서비스 객체(auth, db, storage) export.
- **`seed.ts`**: [체크 ✅] `writeBatch` 및 `Timeout(15초)` 로직이 적용된 데이터 시딩 엔진.

---

## 🚀 3. 핵심 기능 체크리스트

- [x] **Firebase 단일 통합:** Supabase 의존성 제거 및 Firestore 완전 전환 완료.
- [x] **실시간 대시보드:** Firestore 데이터를 실시간으로 반영하는 점주용 모니터링 UI 구축.
- [x] **AI 이상 징후 탐지:** 
  - 본사 마진율 20% 초과 시 경고 발생.
  - 시장 평균가보다 매입 원가가 높을 경우 불투명 마진 의심 품목 분류.
- [x] **안정적인 시딩 툴:** 네트워크 타임아웃 방지 로직 및 트랜잭션(Batch) 무결성 확보.
- [x] **프리미엄 UI/UX:** Glassmorphism 디자인 시스템 및 다국어(한글) 주석 완비.

---

## 🔐 4. 보안 및 환경 설정

- 모든 민감 정보는 `frontend/.env.local`에서 관리합니다.
- Firebase 보안 규칙(Security Rules)을 통해 인가된 사용자만 데이터에 접근하도록 구성되어 있습니다.

---
**작성일:** 2026-02-01  
**상태:** 점검 완료 및 최신화 완료 (Antigravity AI Architect)
