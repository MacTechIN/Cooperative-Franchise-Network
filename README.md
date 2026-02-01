# Cooperative Franchise Network (공공형 프랜차이즈 운영 플랫폼)

## 🎯 프로젝트 개요
본 플랫폼은 기존 프랜차이즈 산업의 불공정한 차액가맹금 구조를 혁신하고, 데이터 투명성을 기반으로 한 소상공인 친화적 공공형 생태계 조성을 목적으로 합니다.

## 🛠 기술 스택
- **Frontend**: Next.js 15+ (App Router, TailwindCSS)
- **Backend / Database**: Firebase (Auth, Cloud Functions) & Supabase (PostgreSQL, REST API)
- **Aesthetics**: Vanilla CSS, Modern Typography, Glassmorphism UI
- **Architecture**: Hybrid Rendering (SSG/SSR) & Nested Layouts 기반 다단계 메뉴 시스템

## 🚀 전제 조건 (Setup Required)
본 서비스의 완벽한 구글 생태계 연동을 위해 다음 설정이 필요합니다:
1. **Firebase Project**: Firebase Console에서 프로젝트 생성 및 `google-services.json` / `firebaseConfig` 확보
2. **Supabase Project**: 데이터베이스 및 REST API 활용을 위한 API Key 및 URL 확보
3. **Google Analytics**: 마케팅 및 운영 분석 연동

## 📂 시작하기
```bash
# Frontend
cd frontend
npm install
npm run dev
```
