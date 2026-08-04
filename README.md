# BO Outbound List Prototype

IIC Combined Global Back Office — Outbound List 화면 프로토타입

## Overview

출고(Outbound) 리스트 및 출고 요청 리스트 화면의 UI 프로토타입입니다.
단일 HTML 파일(inline CSS/JS)로 구성되어 있으며, 별도 빌드 없이 브라우저에서 바로 확인 가능합니다.

## Features

- **탭 구분**: Outbound List / Outbound Request List
- **검색 필터**: BP, Store (멀티 셀렉트), Outbound/Request Status (드롭다운), Registration Date (기간), Keyword
- **Outbound List 컬럼**: Registration Date, Outbound Status, Type, Outbound Tag, I/V No., BP Info, From/To Store·Location, Request Date, Created By
- **Request List 컬럼**: Checkbox, Registration Date, Request Status, Type, BP Info, From/To Store·Location, Request Date, Request Account, Approval Date, Approved By
- **Request Detail 컬럼**: #, Product Info (Code/Name/Barcode), Product Category, Product Subcategory, Available Qty, Request Qty, Outbound Request Date, Request Account, Approval Date, Approved By
- **Outbound Detail**: 기본정보 + Outbound Information (Method/Carrier/Tracking No. 입력) + WMS 자동수신 필드 + 제품 목록
- **Request Detail**: 기본정보 + 제품 목록 (PENDING 시 수량 수정 가능) + Save → Confirm/Reject 플로우
- **가용재고 검증**: Save 시 요청 수량이 Available Qty 초과하면 저장 차단 + 에러 toast (초과 아이템 코드·수량 표시, 실시간 빨간 테두리 하이라이트)
- **수량 재수정**: Save 후 Edit Qty 버튼으로 다시 수량 편집 모드 진입 가능
- **Confirm 시 출고 생성**: 요청 Confirm 시 Outbound 탭에 I/V No. 포함 신규 출고 건 자동 생성
- **플로팅 액션 바**: Request 탭에서 PENDING 행 체크박스 선택 시 일괄 Confirm/Reject
- **상태 변경 이력**: Status History Inquiry 버튼으로 상태 변경 타임라인 확인
- **페이지네이션**: Rows per page (30/50/100/300)

## How to Run

```bash
cd bo-inv-outbound
python3 -m http.server 8080
# http://localhost:8080 에서 확인
```

## Tech Stack

- Single HTML file (inline CSS/JS)
- Pretendard Variable font
- Primary color: `#ff6b35`
