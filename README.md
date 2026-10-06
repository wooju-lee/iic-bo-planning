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
| Inventory > Monitoring > Inventory List | `#/inventory/monitoring/inventory-list` |
| Inventory > Monitoring > Inventory Ledger | `#/inventory/monitoring/inventory-ledger` |
| Order > Order List | `#/order/order-list` |
| Order > Refund List | `#/order/refund-list` (기존 `#/order/return-list` 자동 이동) |
| Front POS > Front POS Main | `#/pos` |
| Front POS > Rx Operation List | `#/pos/rx-operation-list` |
| Front POS > Rx Operation Detail | `#/pos/rx-operation-list/{Order No.}` |
| Front POS > 그 외 탭 (Coming Soon) | `#/pos/daily-sales-summary`, `#/pos/store-pickup-list`, `#/pos/outbound-label-print` |

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

#### S2S Label (TMS)
- **대상**: Outbound Detail 중 Type = S2S 건 (OUTBOUND INFORMATION 헤더의 Label Register / Label Print 버튼)
- **Label Register**: Carrier 선택 (FedEx / UPS, 기본값 FedEx) → CONFIRM 시 TMS로 라벨 요청 → 라벨 수신 시 Carrier / Tracking No. 자동 입력, Label Print 활성화 (TMS 응답은 3초 후 수신으로 시뮬레이션)
- **라벨 상태 표시**: Not Registered / Waiting / Received
- **Label Print**: PDF 뷰어 형태 팝업 (줌 / 회전 / 인쇄, 썸네일) + 4x6 배송 라벨 미리보기 (Ship From/To, 라우팅 코드, 서비스, 송장번호 바코드, REF: I/V No.)
- **Tracking No. 형식**: UPS `1Z` + 16자리, FedEx 숫자 12자리
- **제약**: OUTBOUND_COMPLETED / OUTBOUND_CANCELLED 상태에서는 Label Register 불가
- **상태 변경 이력**: Label Registered / Label Received (TMS) / Label Printed
- 라벨의 스토어 주소, 2D 코드, 바코드는 더미 데이터 (실제 운영 시 TMS에서 PDF 라벨 수신)

### Master Information > Price > Discount Price
- SAP에서 설정한 브랜드 > 스토어별 할인 가격 조회 (POS 매출 시 판매가보다 우선 적용되는 마스터)
- **검색 필터**: Brand (단일), Store (멀티), Product Category 1 (단일), Product Category 2 (멀티), Currency, Keyword (Product Code, Product Name, Price)
- **컬럼**: Brand, Store, Product Category 1/2, Product Info, Currency, Discount Price, Update Date
- **Excel Export**: 페이지네이션과 관계없이 조회 결과 전체 출력
- **페이지네이션**: Rows per page (30/50/100/300)

### Sales > Daily Record View
- **검색 필터**: BP, Store (BP 선택 후 활성화), Sales Type, Currency, Sales Date (기간), Keyword (Receipt No., Original Receipt No., Product Code, Product Name)
- **Total Sum**: Total / Sales Total / Return Total (Currency 단일 선택 시에만 합계 표시)
- **Excel Export**, **페이지네이션**: Discount Price와 동일

### Inventory > Monitoring > Inventory List (조회 전용)
- **검색**: Brand / BP / Store / Location / Product Category 1 / Product Category 2 (Category 1 기준) / Keyword (BP, Store, Product Code / Name, Barcode)
- **컬럼**: BP / Store / Location / Product Info, Product Category 1·2, Collection, On-hand Qty, Outbound Pending Qty, Adjustment Pending Qty, Available Qty, Pending Inbound (수량 컬럼 강조, 우측 정렬)
- On-hand Qty는 Inventory Ledger 더미의 최종 After Qty와 동일, Available = On-hand - Outbound Pending - Adjustment Pending (가정)

### Inventory > Monitoring > Inventory Ledger (조회 전용)
- 메뉴 구조: 운영과 동일 (Inbound / Outbound / Outbound Order / Stocktaking / Adjustment / Movement / Monitoring > Inventory List · Ledger · Snapshot · Snapshot (WH) · In/Out History), 기존 `#/inventory/snapshot`은 Monitoring 하위로 자동 이동
- **검색**: BP / Store (BP 기준) / Location / Inventory Transaction Type (Sales, Refund, Inbound (SAP), Inbound (Manual), Outbound, Adjustment, Stocktaking, Movement) / Search Period (필수, 기본 7일) / Keyword (Invoice No., Product Code, Product Name, Barcode)
- **컬럼**: Transaction Date Time (Time Zone), BP / Store / Location / Product Info, Inventory Transaction Type (처리 유형별 라벨·색상, iic-bo InventoryOperation 기준), Before Qty, Transaction Qty, After Qty, Invoice No.
- **Exclude Reversal History**: 체크 시 원복(Reversal) 이력 제외, 컬럼 정렬, Excel Export
- 원장 데이터는 더미 (스토어·로케이션·상품별 Before / After 연속)

### Order > Order List / Refund List
- **검색 필터** (Daily Record View와 동일한 검색 UI): BP / Store (BP 선택 후 활성화, 멀티 셀렉트), Order Status / Refund Type (멀티 셀렉트), Currency, 기간 (+ TODAY / 1 WEEK / 1 MONTH / 3 MONTHS), Keyword (2자 이상), Reset / Search
- **Order List 컬럼**: Order Date (정렬), Status, Order No. #, Store Information, Location Information, Currency, Total Qty, Total
- **Refund List 컬럼**: Refund Date (정렬), Refund Type, Refund No. # (원주문번호 + 환불번호), Original Order No. #, Store Information, Location, Currency, Total Qty, Total
- **상세 모달**: Order No. / Refund No. 클릭 시 (Order: Order Information + Product List(Qty, Cancel Qty, Unit, Net, VAT, Total) / Refund: Refund Information + Refund Products(Store, Location = 반품 입고 로케이션))
- Total Qty는 패키지 제외, 금액은 VAT 포함 가격 기준 (Net = Total / 1.1), Excel Export (상품 라인 단위)
- 주문 / 환불 데이터는 더미 (US1004·US1001·AU1002~AU1004)

### Front POS
- **진입**: 사이드바 하단 Front POS 버튼 → `#/pos` (운영은 새 창, 프로토타입은 BO 레이아웃을 덮는 전체 화면). 헤더의 Back to BO로 복귀
- **헤더 / 탭**: 계정의 POS 스토어 (`[US1007] GM_CostaMesa_MALL_SCP`) + POS 메뉴 탭 5종 (Front POS Main 외 Coming Soon)
- **제품 (좌측)**: Product Barcode 자동완성 / 바코드 Enter 추가, 수량 +/−, 삭제, 재고 초과 행 경고. 하단 요약 (품목 수·수량, Customer Price, Discount Price(할인 금액), Total)
- **패키지 자동 추가**: 제품 추가 시 매핑된 패키지가 하위 행으로 세트 추가 (가격 0, 수량은 제품 수량을 따라감, 제품 삭제 시 함께 삭제, 바코드 없음, 집계·재고 제외)
- **고객 (우측)**: Customer Member Search (Email / Phone, QR) 또는 Non-Member (선택 시 멤버십 필드 비활성화). 멤버 선택 시 Customer Information 자동 입력 (Country / Continent / Customer Type / Gender, Usage Type은 수기)
- **Sales & Print**: Confirm Sales (제품 + Cashier + 고객 필수, Invoice No. 미입력 시 자동 생성) → AC Card Print → AC Card RE Print, Skip AC Card Print 선택 가능
- **Gift Pay**: Serial 입력 + FOC Check (시뮬레이션)
- **미구현 / 확인 필요**: SALES / INVENTORY 토글 동작, Manual Refund, 외부 POS 매출 조회 후 AC Card 출력, AC Card 내용
- 가격, 재고, 멤버, Cashier / Seller, 패키지 매핑은 더미 데이터

### Front POS > Rx Operation
- 기존 별도 프로토타입 (bo_pos_rx_operation, boposrxoperation.vercel.app)을 이 프로토타입으로 통합. 스토어는 POS 스토어 (US1007) 기준
- **List**: 검색 필터 (Approval Status / Processing Status / Cancel·Refund 멀티 셀렉트, Search Period (Order / Save Date), Keyword 2자 이상), 페이지네이션
- **Register Outbound**: Confirm + 미완료 + 취소·환불 아님 건만 선택 가능 → Outbound Registration 팝업 (Carrier FedEx / UPS, 기본 FedEx) → TMS 전송
- **Customer Email**: Confirm + Completed + 취소·환불 아님 건만 발송 가능
- **Detail**: Customer Membership Info (멤버 검색, 등록 처방전 선택, Non-Member) / Order Info (Mapped Product, C.O.F) / Prescription (업로드 + OCR 자동 입력 시뮬레이션, 환자·처방자, SPH·CYL·AXIS·PD(Single/Dual)·OC) / Purchaser Info (주문자) / Patient & Shipping Info (수령자 = 처방전 환자, 이름 자동 입력, Ship to Address 주소 검색·수기 입력·검증, Ship to Store) / Policy Agreements (동의 + 서명) / Comment
- **Index**: 섹션별 완료 체크 → Save (Unready → Requested) → Confirm / Reject. Unready 외 상태는 읽기 전용, 승인 상태는 목록에도 반영

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
