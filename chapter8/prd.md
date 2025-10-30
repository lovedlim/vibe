# 재무 데이터 시각화 분석 서비스 개발 계획

## 기술 스택

- **프레임워크**: Next.js 14 (App Router)
- **스타일링**: Tailwind CSS
- **차트**: Recharts
- **AI**: Google Gemini API (gemini-2.0-flash)
- **배포**: Vercel

## 단계별 구현

### 1단계: 프로젝트 초기 설정 및 회사 검색 기능

- Next.js 프로젝트 생성 및 기본 설정
- `.env.local` 파일 생성하여 API 키 관리
- corp.xml을 JSON으로 변환하는 스크립트 작성 (`scripts/convert-corp-data.js`)
- 변환된 데이터를 `public/corp-data.json`에 저장
- 회사명 검색 API 엔드포인트 생성 (`app/api/search-company/route.js`)
- 검색 UI 컴포넌트 구현 (자동완성 기능 포함)

**핵심 파일**:

- `next.config.js`: Next.js 설정
- `.env.local`: API 키 저장 (gitignore 처리)
- `.env.example`: 환경변수 템플릿
- `scripts/convert-corp-data.js`: XML → JSON 변환
- `app/api/search-company/route.js`: 검색 API
- `app/page.js`: 메인 검색 페이지
- `app/components/CompanySearch.js`: 검색 컴포넌트

### 2단계: OpenDart API 연동 및 재무 데이터 시각화

- OpenDart API 호출 백엔드 API 생성 (`app/api/financial-data/route.js`)
- 재무제표 데이터 파싱 및 가공 로직 구현
- 연도 및 보고서 타입 선택 UI (2015~현재, 사업보고서/분기보고서)
- Recharts를 활용한 차트 컴포넌트 구현:
- 재무상태표 (자산/부채/자본) - 막대 차트
- 손익계산서 (매출/영업이익/순이익) - 라인 차트
- 연결/개별 재무제표 비교 - 그룹 차트
- 로딩 상태 및 에러 처리

**핵심 파일**:

- `app/api/financial-data/route.js`: OpenDart API 프록시
- `app/company/[corpCode]/page.js`: 회사별 재무 데이터 페이지
- `app/components/FinancialCharts.js`: 차트 컴포넌트
- `app/components/YearSelector.js`: 연도/보고서 선택 UI

### 3단계: Gemini AI 재무 분석 기능

- Gemini API 연동 백엔드 API 생성 (`app/api/analyze-financial/route.js`)
- 재무 데이터를 자연어로 변환하여 Gemini에 전달
- 프롬프트 엔지니어링: "초보자도 이해할 수 있는 재무 분석" 유도
- AI 분석 결과를 마크다운 형식으로 렌더링
- 주요 분석 항목:
- 재무 건전성 평가
- 수익성 분석
- 전년 대비 성장률 분석
- 투자 포인트 및 주의사항
- 스트리밍 응답 처리 (점진적 렌더링)

**핵심 파일**:

- `app/api/analyze-financial/route.js`: Gemini API 호출
- `app/components/AIAnalysis.js`: AI 분석 결과 표시 컴포넌트
- `app/utils/financial-prompt.js`: 프롬프트 템플릿

## 보안 및 배포 고려사항

- `.env.local`에 API 키 저장, `.gitignore`에 추가
- Vercel 환경변수에 프로덕션 API 키 설정
- API Routes에서 서버 사이드로 API 호출 (클라이언트 노출 방지)
- API 호출 실패 시 명확한 에러 메시지 제공
- 존재하지 않는 데이터 요청 시 안내 메시지 표시

## 데이터 흐름

1. 사용자가 회사명 검색
2. corp_code 조회 후 회사 상세 페이지로 이동
3. 연도 및 보고서 타입 선택
4. OpenDart API로 재무 데이터 조회
5. 차트로 시각화
6. Gemini API로 AI 분석 요청
7. 분석 결과 표시

### To-dos

- [ ] Next.js 프로젝트 초기 설정 및 필요한 패키지 설치
- [ ] corp.xml을 JSON으로 변환하는 스크립트 작성 및 실행
- [ ] 회사 검색 UI 및 자동완성 기능 구현
- [ ] OpenDart API 연동을 위한 API Route 및 유틸 함수 구현
- [ ] 재무제표 차트 및 테이블 컴포넌트 구현
- [ ] Gemini AI 분석 API Route 및 UI 컴포넌트 구현
- [ ] 환경변수 설정 및 보안 처리 (.env.local, .env.example, .gitignore)
- [ ] 반응형 디자인, 로딩 상태, 에러 핸들링 등 UX 개선


### 별도로 붙여 넣을 내용
- api key: 
- gemini api: 
- 단일회사 주요계정 개발가이드: 