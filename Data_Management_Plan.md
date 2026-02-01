# 사용자 데이터 기록 및 보존 계획 (Data Management Plan)

TogetherChise 플랫폼의 사용자 데이터를 안정적으로 기록하고 보존하기 위한 향후 운영 및 기술 계획입니다. 본 계획은 **Firebase(Firestore, Auth, Storage)**의 기능을 최대한 활용하여 데이터의 무결성과 가용성을 보장하는 데 초점을 맞춥니다.

---

## 🏗️ 1. 데이터 기록 (Recording Strategy)

사용자의 모든 활동과 비즈니스 데이터는 다음과 같은 원칙에 따라 기록됩니다.

### 📍 주요 데이터 포인트
- **사용자 프로필:** 가맹점 정보(업체명, 주소, 전화번호), 담당자 연락처, 권한 등 (Firestore `users` 컬렉션)
- **비즈니스 데이터:** 원가 내역, 장부 기록, AI 탐지 히스토리 (Firestore `franchise_costs`, `franchise_ledger` 등)
- **소셜 데이터:** Google Auth 연동을 통한 기본 사용자 정보 (이메일, 프로필 사진)

### ⚙️ 기록 방식
- **실시간 쓰기 (Real-time Write):** Firestore SDK를 통해 클라이언트에서 발생하는 모든 비즈니스 이벤트를 즉각적으로 DB에 반영합니다.
- **트랜잭션 보장 (Atomic Transactions):** 연관된 데이터(예: 장부 입력과 재고 업데이트)는 `writeBatch`나 트랜잭션을 사용하여 데이터 불일치를 방지합니다.
- **타임스탬프 표준화:** 모든 데이터에는 `serverTimestamp()`를 사용하여 클라이언트 기기 시간과 상관없이 신뢰할 수 있는 기록 시점을 확보합니다.

---

## 💾 2. 데이터 보존 및 관리 (Preservation & Management)

기록된 데이터를 안전하게 보존하고 효율적으로 관리하기 위한 전략입니다.

### 📦 저장 구조 (Storage Hierarchy)
- **Hot Data (Firestore):** 현재 운영 중인 최신 데이터. 빠른 접근성을 위해 인덱싱 최적화 수행.
- **Media Assets (Cloud Storage):** 영수증 사진, 증빙 서류 등 대용량 파일은 Firebase Storage에 저장하고, DB에는 해당 파일의 URL만 기록합니다.

### 🔄 백업 및 복구 (Backup & Recovery)
- **자동 백업:** Google Cloud (GCP) 기능을 연동하여 Firestore 데이터를 일별/주별로 자동 백업(Scheduled Export)합니다.
- **Point-in-Time Recovery (PITR):** 실수로 인한 데이터 삭제 시 특정 시점으로 복구할 수 있는 PITR 기능을 활성화하여 운영 리스크를 최소화합니다.

### 🗑️ 데이터 라이프사이클 관리
- **보존 기간 설정:** 법적 의무(회계 자료 5년 등)에 따라 데이터 보존 기간을 설정하고, 만료된 데이터는 별도의 Cold Storage(BigQuery 등)로 아카이빙하거나 자동 삭제 처리합니다.

---

## 🛡️ 3. 보안 및 프라이버시 (Security & Privacy)

사용자 데이터의 안전한 보호를 위한 핵심 장치입니다.

- **접근 제어 (Security Rules):** Firebase Security Rules를 정의하여 "자신의 데이터만 읽고 쓸 수 있는" 강력한 권한 제어를 시행합니다.
- **데이터 암호화:** Firestore 및 Storage의 데이터는 저장 시(At-rest) 및 전송 시(In-transit) Google 관리 암호화 키를 통해 자동 암호화됩니다.
- **개인정보 익명화:** 통계 분석 및 AI 학습 시에는 개인 식별 정보(PII)를 자동으로 익명화하거나 마스킹 처리하여 보존합니다.

---

## 📈 4. 향후 확장 계획 (Scalability)

- **BigQuery 연동:** 가맹점 수가 늘어남에 따라 발생하는 대규모 트렌드 분석을 위해 Firestore 데이터를 BigQuery로 실시간 스트리밍하여 분석 인프라를 확장합니다.
- **엣지 로깅:** 전 세계 어디서든 빠른 기록을 위해 Firebase의 글로벌 분산 인프라를 적극 활용합니다.

---
**작성일:** 2026-02-01  
**상태:** 데이터 설계 가이드라인 확정 (TogetherChise DevOps Team)
