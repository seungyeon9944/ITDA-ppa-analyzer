## 📟 SoC 설계 지원을 위한 PPA 분석기 개발
> 🔒 **Notice**
> 본 프로젝트는 **잇다반도체**와의 기업연계 프로젝트로, 소스 코드는 비공개 처리되어 있습니다.
> 코드 열람이 필요하신 경우 별도로 문의해 주시면 감사하겠습니다.

SOC 설계 과정에서 발생하는 Power / Performance / Area (PPA) 수치를 통합적으로 분석·비교·시각화하는 웹 대시보드를 개발한다.

## 1. 기업명
잇다반도체

## 2. 목표
설계자가 수작업·스크립트·엑셀로 분산 수행하던 **PPA 분석**을 **자동화**하고, 설계 변경이 **PPA 수치에 주는 영향을 한 화면에서 비교**할 수 있도록 한다.

| 구분 | 내용 |
|---|---|
| 사용 대상 | SOC 설계 엔지니어 |
| 산출물 | PPA 분석기 사이트 / 개발·테스트 코드 / 사용자 매뉴얼 |
| 배포 수준 | 실서비스 수준 (Power·Performance·Area 세 파트 통합) |


## 3. 담당 역할


## 4. 주요 기능 및 화면


## 5. 시스템 아키텍처
### 프론트엔드 
- **Header**: 최상단 전체 폭에 위치하며 Power·Performance·Area 파트 전환 탭을 포함한다. 파트 전환과 관계없이 유지된다.
- **Nav-Header**: 좌측 사이드바 상단에 위치한다.
- **Page-Header**: 우측 메인 영역 상단에 위치한다.
- **Navigation**: 좌측 사이드바 본문으로 페이지 간 이동 메뉴를 제공한다.
- **Page**: 우측 메인 콘텐츠 영역이다.

<br>

- **Vue 3 SPA**로 구성되며 `home / power / performance / area` 4개 라우트로 분리된다. 
- 전역 상태는 **Pinia로 관리**한다. 
- 브라우저에서 즉시 계산 가능한 단순 값(슬라이더 미리보기, 기본 면적 등)은 프론트에서 처리하고 백엔드를 호출하지 않는다.

<br>

| 라이브러리 | 용도 |
|---|---|
| Vue 3 | SPA 프레임워크 |
| Vue Router | 파트별 라우팅 |
| Pinia | 전역 상태 관리 |
| Vuetify | 공통 UI 컴포넌트 |
| Naive UI | 테이블 컴포넌트 |
| Vue Flow | Area 다이어그램 |
| splitpanes | 분할 레이아웃 |
| TypeScript | 언어 |

<br>

### 백엔드

- **Python과 FastAPI** 단일 구성으로 운용하며, Node.js 별도 서버는 사용하지 않는다.
- FastAPI는 프론트가 보낸 입력(IP 스펙, 시나리오, 배치)을 받아 Power / Performance / Area 수치를 계산해 반환한다.
- 그래프 탐색(Performance 병목 분석), 다수 블록 합산 같은 복합 계산과 DB 연동·파일 업로드 같은 외부 시스템 접점을 담당한다.

<br>

| 라이브러리 | 용도 |
|---|---|
| FastAPI | REST API 서버 |
| uvicorn | ASGI 서버 |

<br>

### 데이터베이스 

- DB는 **MongoDB**를 사용한다.
- MongoDB 연결 시점에 저장/불러오기 지점을 API 호출로 교체할 수 있도록 입출력 경계를 분리해 둔다.

<br>

## 인프라 

- **CI/CD**: GitLab push를 트리거로 **Jenkins**에서 빌드·배포 후, **Nginx**가 정적 파일을 서빙하면서 `/api` 요청을 FastAPI로 프록시한다.
- **Nginx**: `/` 경로로 Vue SPA를 서빙하고, `/api` 경로로 FastAPI 리버스 프록시를 연결하며, CORS 헤더를 설정한다.

## 4. 개발 가이드
### 환경 설정
```
cd FE
npm install
```

<br>

### 실행 방법
- 로컬 접속 주소 http://localhost:8080 이다.
- 서버 접속 주소는 https://k14s101.p.ssafy.io/dev/home 이다.

```
cd FE
npm run serve
```

## 5. 