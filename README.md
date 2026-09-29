# IIC BO Planning Prototype

IIC Combined Global Back Office — 기획용 통합 프로토타입

## Overview

BO 신규/변경 기능의 기획 검토용 UI 프로토타입입니다. 여러 메뉴를 하나의 프로토타입에서 관리하며, 메뉴·상세 진입 시 URL(hash route)이 변경됩니다.
단일 HTML 파일(inline CSS/JS)로 구성되어 있으며, 별도 빌드 없이 브라우저에서 바로 확인 가능합니다.

- **URL**: https://iic-bo-planning.vercel.app (기존 https://bo-inv-outbound.vercel.app)
- **Repository**: https://github.com/wooju-lee/iic-bo-planning

## Menus (URL)

| Menu | URL (hash route) |
|---|---|
| Inventory > Outbound List | `#/inventory/outbound` |
| Inventory > Outbound Request List | `#/inventory/outbound?tab=request` |
| Outbound Detail | `#/inventory/outbound/{I/V No.}` |
| Outbound Request Detail | `#/inventory/outbound-request/{Request No.}` |
| Master Information > Price > Discount Price | `#/master/price/discount-price` |
| Sales > Daily Record View | `#/sales/daily-record-view` |

그 외 사이드바 메뉴는 "Coming Soon" 화면으로 표시됩니다.

## Features

### Inventory > Outbound
- **탭 구분**: Outbound List / Outbound Request List
- **검색 필터**: BP, Store (멀티 셀렉트), Outbound/Request Status (드롭다운), Registration Date (기간), Keyword
- **Outbound List 컬럼**: Registration Date, Outbound Status, Type, Outbound Tag, I/V No., BP Info, From/To Store·Location, Request Date, Created By
- **Request List 컬럼**: Checkbox, Registration Date, Request Status, Type, BP Info, From/To Store·Location, Request Date, Request Account, Approval Date, Approved By
- **Request Detail 컬럼**: #, Product Info (Code/Name/Barcode), Product Category, Product Subcategory, Available Qty, Request Qty, Outbound Request Date, Request Account, Approval Date, Approved By
- **Outbound Detail**: 기본정보 + Outbound Information (Method/Carrier/Tracking No. 입력) + WMS 자동수신 필드 + 제품 목록
- **Request Detail**: 기본정보 + 제품 목록 (PENDING 시 수량 수정 가능) + Save → Confirm/Reject 플로우
- **가용재고 검증**: Save 시 요청 수량이 Available Qty 초과하면 저장 차단 + 에러 toast
- **Confirm 시 출고 생성**: 요청 Confirm 시 Outbound 탭에 I/V No. 포함 신규 출고 건 자동 생성
- **플로팅 액션 바**: Request 탭에서 PENDING 행 체크박스 선택 시 일괄 Confirm/Reject
- **상태 변경 이력**: Status History Inquiry 버튼으로 상태 변경 타임라인 확인
- **출고 등록 모달**: 4-Step (출고 스토어 → 입고 스토어 → 제품 선택 → 출고 정보 입력), 재오픈 시 신규 상태로 초기화
- **Outbound Order Tag**: General 탭 + L2S(WH → Store)일 때만 선택 (NORMAL / SEEDING / TIKTOK / PS / PREORDER / GIFT / RX)
- **페이지네이션**: Rows per page (30/50/100/300)

### Master Information > Price > Discount Price
- SAP에서 설정한 브랜드 > 스토어별 할인 가격 조회 (POS 매출 시 판매가보다 우선 적용되는 마스터)
- **검색 필터**: Brand, Store (멀티 셀렉트), Product Category 1/2, Currency, Keyword (Product Code, Product Name, Discount Price)
- **컬럼**: Brand, Store, Product Category 1/2, Product Info, Currency, Discount Price, Update Date
- **Excel Export**: 페이지네이션과 관계없이 조회 결과 전체 출력
- **페이지네이션**: Rows per page (30/50/100/300)

### Sales > Daily Record View
- **검색 필터**: BP, Store (BP 선택 후 활성화), Sales Type, Currency, Sales Date (기간), Keyword (Receipt No., Original Receipt No., Product Code, Product Name)
- **Total Sum**: Total / Sales Total / Return Total (Currency 단일 선택 시에만 합계 표시)
- **Excel Export**, **페이지네이션**: Discount Price와 동일

## Master Data

- **Store**: C1002 / GM_미국법인 (US1001 ~ US1022, WH: US1006, US1022), AU1001 / GM_Sydney_DFS_AirportT1 (Discount Price)
- **Product**: 11000000 ~ 11000024 (GENTLE MONSTER)
- 가격·수량·매출 값은 더미 데이터

## How to Run

```bash
cd iic-bo-planning
python3 -m http.server 8080
# http://localhost:8080 에서 확인
```

## Tech Stack

- Single HTML file (inline CSS/JS)
- Pretendard Variable font, GentleSans (logo)
- SheetJS (Excel Export, CDN)
- Primary color: `#ff6b35`
