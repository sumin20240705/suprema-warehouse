echo '<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>사내대체 관리 현황</title>
  <style>
    body {
      font-size: 10pt;
      background: #f5f6fa;
      margin: 0;
      font-family: "맑은 고딕", "Malgun Gothic", Arial, sans-serif;
    }
    .container {
      display: flex;
      height: 100vh;
    }
    .sidebar {
      min-width: 120px;
      background: #263238;
      color: #fff;
      padding: 0;
      display: flex;
      flex-direction: column;
      align-items: stretch;
    }
    .sidebar button {
      background: none;
      border: none;
      color: #fff;
      padding: 18px 10px;
      text-align: left;
      font-size: 10pt;
      cursor: pointer;
      border-bottom: 1px solid #37474f;
      transition: background 0.2s;
    }
    .sidebar button.active, .sidebar button:hover {
      background: #1976d2;
    }
    .main {
      flex: 1;
      padding: 24px 12px 0 24px;
      overflow: auto;
    }
    h2 {
      margin: 0 0 10px 0;
      font-size: 18pt;
      font-weight: bold;
      letter-spacing: -1px;
    }
    .header-controls {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 16px;
    }
    .filter-controls {
      display: flex;
      align-items: center;
      gap: 8px;
      margin-bottom: 16px;
    }
    .filter-controls input, .filter-controls select {
      padding: 4px 8px;
      border: 1px solid #ddd;
      border-radius: 3px;
      font-size: 10pt;
    }
    .btn {
      padding: 8px 16px;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      font-size: 10pt;
      margin-left: 8px;
    }
    .btn-primary {
      background: #1976d2;
      color: white;
    }
    .btn-secondary {
      background: #6c757d;
      color: white;
    }
    .btn-success {
      background: #28a745;
      color: white;
    }
    .btn-danger {
      background: #dc3545;
      color: white;
    }
    .btn-warning {
      background: #ff9800;
      color: white;
    }
    .btn:hover {
      opacity: 0.8;
    }
    .table-wrap {
      background: #f5f6fa;
      border-radius: 6px;
      padding: 0;
      overflow-x: auto;
      box-shadow: 0 2px 8px #0001;
      margin-top: 8px;
    }
    table {
      border-collapse: collapse;
      width: max-content;
      min-width: 100%;
      background: #fff;
      font-size: 10pt;
    }
    thead th {
      color: #fff;
      position: sticky;
      top: 0;
      z-index: 2;
      padding: 6px 8px;
      border: 1px solid #ddd;
      white-space: nowrap;
      text-align: center;
    }
    /* 헤더 배경색 - 이미지에 맞춰 설정 */
    .header-dark-gray { background: #6c757d; }
    .header-light-blue { background: #17a2b8; }
    .header-light-orange { background: #fd7e14; }
    .header-light-yellow { background: #ffc107; color: #000; }
    .header-red-text { color: #dc3545 !important; }
    
    tbody td {
      border: 1px solid #e3e3e3;
      padding: 4px 8px;
      white-space: nowrap;
      text-align: center;
    }
    
    /* 완료된 행 스타일 */
    .row-completed {
      background-color: #e9ecef !important;
    }
    
    /* 출고 가능 상태 스타일 */
    .status-possible {
      color: #28a745 !important;
      font-weight: bold;
    }
    .status-impossible {
      color: #dc3545 !important;
      font-weight: bold;
    }
    
    /* 출고 요청 일 날짜 입력 시 노란색 배경 */
    .shipment-request-highlight {
      background-color: #fff3cd !important;
    }
    
    /* 링크 스타일 */
    .link-cell a {
      color: #1976d2;
      text-decoration: underline;
    }
    .link-cell a:hover {
      color: #0d47a1;
    }
    
    /* 링크 칼럼 너비 제한 */
    .link-column {
      max-width: 120px;
      overflow: hidden;
      text-overflow: ellipsis;
    }
    
    .inline-edit {
      width: 90%;
      font-size: 10pt;
      border: 1px solid #90caf9;
      border-radius: 3px;
      padding: 2px 4px;
    }
    .modal-bg {
      position: fixed; left: 0; top: 0; width: 100vw; height: 100vh;
      background: #0005; z-index: 1000; display: flex; align-items: center; justify-content: center;
    }
    .modal {
      background: #fff; border-radius: 8px; padding: 24px 20px 16px 20px; min-width: 320px; max-width: 90vw;
      box-shadow: 0 4px 24px #0003;
    }
    .modal h3 { margin-top: 0; }
    .modal label { display: block; margin: 8px 0 2px 0; }
    .modal input, .modal select, .modal textarea {
      font-size: 10pt; padding: 2px 4px; border-radius: 3px; border: 1px solid #90caf9;
      width: 100%;
      margin-bottom: 8px;
    }
    .modal textarea {
      min-height: 80px; resize: vertical;
    }
    .modal .btns { margin-top: 12px; text-align: right; }
    .modal .btns button { font-size: 10pt; margin-left: 8px; }
    
    /* 메일 작성 모달 */
    .mail-modal {
      background: #fff; border-radius: 8px; padding: 24px 20px 16px 20px; min-width: 500px; max-width: 80vw;
      box-shadow: 0 4px 24px #0003;
    }
    .mail-modal h3 { margin-top: 0; }
    .mail-modal label { display: block; margin: 8px 0 2px 0; font-weight: bold; }
    .mail-modal input, .mail-modal textarea {
      font-size: 10pt; padding: 8px; border-radius: 3px; border: 1px solid #ddd;
      width: 100%;
      margin-bottom: 12px;
    }
    .mail-modal textarea {
      min-height: 200px; resize: vertical;
    }
    .mail-modal .btns { margin-top: 16px; text-align: right; }
    .mail-modal .btns button { font-size: 10pt; margin-left: 8px; }
    
    @media (max-width: 700px) {
      .sidebar { min-width: 60px; }
      .main { padding: 8px 2px 0 2px; }
    }
  </style>
</head>
<body>
  <div class="container">
    <nav class="sidebar">
      <button id="menu-dashboard" class="active">사내대체 관리 현황</button>
      <button id="menu-settings">사용자 설정</button>
      <button id="menu-export" class="btn btn-success" style="margin: 8px;">데이터 내보내기</button>
    </nav>
    <main class="main">
      <section id="section-dashboard">
        <div class="header-controls">
          <h2>사내대체 관리 현황</h2>
          <div>
            <button id="btn-select-all" class="btn btn-secondary">전체 선택</button>
            <button id="btn-add-new" class="btn btn-primary">신규추가</button>
            <button id="btn-send-mail" class="btn btn-warning">메일 발송</button>
            <button id="btn-delete" class="btn btn-danger">삭제</button>
          </div>
        </div>
        
        <div class="filter-controls">
          <label>요청일: <input type="date" id="filter-request-date"></label>
          <label>기안자: <input type="text" id="filter-drafter" placeholder="기안자명"></label>
          <label>법인: 
            <select id="filter-corporation">
              <option value="">전체</option>
              <option value="SP">SP</option>
              <option value="HQ">HQ</option>
              <option value="MOCA">MOCA</option>
            </select>
          </label>
          <label>실출고: 
            <select id="filter-actual-shipment">
              <option value="">전체</option>
              <option value="완료">완료</option>
              <option value="">미완료</option>
            </select>
          </label>
          <label>출고 가능: 
            <select id="filter-shipment-possible">
              <option value="">전체</option>
              <option value="가능">가능</option>
              <option value="불가">불가</option>
            </select>
          </label>
          <button id="btn-clear-filter" class="btn btn-secondary">필터 초기화</button>
        </div>
        
        <div class="table-wrap">
          <table id="main-table">
            <thead>
              <tr>
                <th class="header-dark-gray">선택</th>
                <th class="header-dark-gray">법인</th>
                <th class="header-dark-gray">연도</th>
                <th class="header-dark-gray">주차</th>
                <th class="header-dark-gray">요청일</th>
                <th class="header-light-blue">납기 L/T</th>
                <th class="header-light-orange header-red-text">출고 요청 일</th>
                <th class="header-light-orange">실출고</th>
                <th class="header-light-orange">완료 날짜</th>
                <th class="header-light-blue">전산작업</th>
                <th class="header-dark-gray">출고 가능</th>
                <th class="header-dark-gray">정규출고 합포</th>
                <th class="header-dark-gray">요청번호</th>
                <th class="header-dark-gray">기안제목</th>
                <th class="header-dark-gray">기안자</th>
                <th class="header-dark-gray">내부 기안</th>
                <th class="header-dark-gray">출고 형태</th>
                <th class="header-dark-gray">기안 최종 승인</th>
                <th class="header-dark-gray">특이사항(비고)</th>
                <th class="header-dark-gray link-column">링크</th>
              </tr>
            </thead>
            <tbody>
              <!-- JS로 행 추가 -->
            </tbody>
          </table>
        </div>
      </section>

      <section id="section-settings" style="display:none;">
        <div class="header-controls">
          <h2>사용자 설정</h2>
        </div>
        
        <div class="modal">
          <h3>기안자 메일 주소 설정</h3>
          <div id="email-settings">
            <div style="margin-bottom: 16px;">
              <label>기안자명: <input type="text" id="drafter-name" placeholder="기안자명"></label>
              <label>메일 주소: <input type="email" id="drafter-email" placeholder="email@example.com"></label>
              <button id="btn-add-email" class="btn btn-primary">추가</button>
            </div>
            <div id="email-list"></div>
          </div>
          
          <h3>메일 초안 설정</h3>
          <div>
            <label>메일 제목: <input type="text" id="mail-subject" placeholder="메일 제목"></label>
            <label>메일 내용: <textarea id="mail-content" placeholder="메일 내용을 입력하세요"></textarea></label>
            <button id="btn-save-mail-template" class="btn btn-success">저장</button>
          </div>
        </div>
      </section>

      <section id="section-export" style="display:none;">
        <div class="header-controls">
          <h2>데이터 내보내기</h2>
        </div>
        
        <div class="modal">
          <h3>데이터 내보내기</h3>
          <p>현재 입력된 모든 데이터를 포함한 HTML 파일을 생성합니다.</p>
          <button id="btn-generate-standalone" class="btn btn-primary">독립 실행 파일 생성</button>
          <div id="export-result" style="margin-top: 16px;"></div>
        </div>
      </section>
    </main>
  </div>

  <!-- 신규추가 모달 -->
  <div id="modal-bg" class="modal-bg" style="display:none;">
    <div class="modal">
      <h3>신규 데이터 추가</h3>
      <form id="add-form">
        <label>요청일 <input name="요청일" type="date" required></label>
        <label>법인 
          <select name="법인">
            <option value="">-</option>
            <option value="SP">SP</option>
            <option value="HQ">HQ</option>
            <option value="MOCA">MOCA</option>
          </select>
        </label>
        <label>출고 요청 일 <input name="출고요청일" type="date"></label>
        <label>정규출고 합포 
          <select name="정규출고합포">
            <option value="">-</option>
            <option value="O">O</option>
            <option value="X">X</option>
          </select>
        </label>
        <label>요청번호 <input name="요청번호"></label>
        <label>기안제목 <input name="기안제목"></label>
        <label>기안자 <input name="기안자"></label>
        <label>출고 형태 
          <select name="출고형태">
            <option value="">-</option>
            <option value="국내택배">국내택배</option>
            <option value="해외출고">해외출고</option>
            <option value="직접수령">직접수령</option>
            <option value="전산처리">전산처리</option>
          </select>
        </label>
        <label>특이사항(비고) <textarea name="특이사항"></textarea></label>
        <label>링크 <input name="링크"></label>
        <div class="btns">
          <button type="button" id="btn-cancel-modal">취소</button>
          <button type="submit">추가</button>
        </div>
      </form>
    </div>
  </div>

  <!-- 메일 작성 모달 -->
  <div id="mail-modal-bg" class="modal-bg" style="display:none;">
    <div class="mail-modal">
      <h3>메일 작성</h3>
      <form id="mail-form">
        <label>수신자: <input type="text" id="mail-to" readonly></label>
        <label>제목: <input type="text" id="mail-subject-input"></label>
        <label>내용: <textarea id="mail-body"></textarea></label>
        <div class="btns">
          <button type="button" id="btn-cancel-mail">취소</button>
          <button type="button" id="btn-send-mail-final">발송</button>
        </div>
      </form>
    </div>
  </div>

  <script>
    // 데이터 저장
    let dataList = [];
    let filteredDataList = [];
    let emailSettings = {};
    let mailTemplate = { subject: "", content: "" };

    // 자동 저장 기능
    function saveData() {
      localStorage.setItem("substituteManagement_dataList", JSON.stringify(dataList));
    }

    function loadData() {
      const savedDataList = localStorage.getItem("substituteManagement_dataList");
      if (savedDataList) dataList = JSON.parse(savedDataList);
    }

    function saveEmailSettings() {
      localStorage.setItem("substituteManagement_emailSettings", JSON.stringify(emailSettings));
    }

    function loadEmailSettings() {
      const saved = localStorage.getItem("substituteManagement_emailSettings");
      if (saved) emailSettings = JSON.parse(saved);
    }

    function saveMailTemplate() {
      localStorage.setItem("substituteManagement_mailTemplate", JSON.stringify(mailTemplate));
    }

    function loadMailTemplate() {
      const saved = localStorage.getItem("substituteManagement_mailTemplate");
      if (saved) mailTemplate = JSON.parse(saved);
    }

    // 연도 추출 함수
    function getYearFromDate(dateStr) {
      if (!dateStr) return "";
      return new Date(dateStr).getFullYear();
    }

    // 주차 계산 함수 (요청일 기준 주차-1)
    function getWeekFromDate(dateStr) {
      if (!dateStr) return "";
      const date = new Date(dateStr);
      const start = new Date(date.getFullYear(), 0, 1);
      const days = Math.floor((date - start) / (24 * 60 * 60 * 1000));
      const weekNumber = Math.ceil((days + start.getDay() + 1) / 7) - 1;
      return weekNumber > 0 ? weekNumber : 1;
    }

    // 납기 L/T 계산 함수 - 수정된 로직
    function calculateLT(requestDate, completionDate, shipmentRequestDate) {
      if (!requestDate) return "";
      
      // 출고 요청 일이 있으면 출고 요청 일 - 완료 날짜 계산
      if (shipmentRequestDate && completionDate) {
        const shipReqDate = new Date(shipmentRequestDate);
        const compDate = new Date(completionDate);
        const diffTime = compDate - shipReqDate;
        const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));
        return diffDays > 0 ? diffDays : "";
      }
      
      // 출고 요청 일이 없으면 완료 날짜 - 요청일 계산
      if (completionDate) {
        const reqDate = new Date(requestDate);
        const compDate = new Date(completionDate);
        const diffTime = compDate - reqDate;
        const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));
        return diffDays > 0 ? diffDays : "";
      }
      
      return "";
    }

    // 출고 가능 상태 계산 함수 - 수정된 로직
    function calculateShipmentStatus(internalDraft, finalApproval) {
      if (internalDraft === "O" && finalApproval === "O") {
        return "가능";
      } else {
        return "불가";
      }
    }

    // 필터링 함수
    function applyFilters() {
      const requestDate = document.getElementById("filter-request-date").value;
      const drafter = document.getElementById("filter-drafter").value.toLowerCase();
      const corporation = document.getElementById("filter-corporation").value;
      const actualShipment = document.getElementById("filter-actual-shipment").value;
      const shipmentPossible = document.getElementById("filter-shipment-possible").value;

      filteredDataList = dataList.filter(row => {
        if (requestDate && row["요청일"] !== requestDate) return false;
        if (drafter && !row["기안자"].toLowerCase().includes(drafter)) return false;
        if (corporation && row["법인"] !== corporation) return false;
        if (actualShipment !== "" && row["실출고"] !== actualShipment) return false;
        
        const status = calculateShipmentStatus(row["내부기안"], row["기안최종승인"]);
        if (shipmentPossible !== "" && status !== shipmentPossible) return false;
        
        return true;
      });
      
      renderTable();
    }

    // 필터 초기화
    function clearFilters() {
      document.getElementById("filter-request-date").value = "";
      document.getElementById("filter-drafter").value = "";
      document.getElementById("filter-corporation").value = "";
      document.getElementById("filter-actual-shipment").value = "";
      document.getElementById("filter-shipment-possible").value = "";
      filteredDataList = [...dataList];
      renderTable();
    }

    // 테이블 렌더링
    function renderTable() {
      const tbody = document.querySelector("#main-table tbody");
      tbody.innerHTML = "";
      
      const dataToRender = filteredDataList.length > 0 ? filteredDataList : dataList;
      
      dataToRender.forEach((row, idx) => {
        const originalIdx = dataList.indexOf(row);
        const tr = document.createElement("tr");
        
        // 체크박스
        const tdChk = document.createElement("td");
        const chk = document.createElement("input");
        chk.type = "checkbox";
        chk.className = "data-chk";
        chk.dataset.idx = originalIdx;
        tdChk.appendChild(chk);
        tr.appendChild(tdChk);
        
        // 법인 (드롭다운)
        const tdCorporation = document.createElement("td");
        const selectCorporation = document.createElement("select");
        selectCorporation.innerHTML = "<option value=\"\">-</option><option value=\"SP\">SP</option><option value=\"HQ\">HQ</option><option value=\"MOCA\">MOCA</option>";
        selectCorporation.value = row["법인"] || "";
        selectCorporation.onchange = function() {
          row["법인"] = this.value;
          renderTable();
          saveData();
        };
        tdCorporation.appendChild(selectCorporation);
        tr.appendChild(tdCorporation);
        
        // 연도 (자동 계산)
        const tdYear = document.createElement("td");
        tdYear.textContent = getYearFromDate(row["요청일"]);
        tr.appendChild(tdYear);
        
        // 주차 (자동 계산)
        const tdWeek = document.createElement("td");
        tdWeek.textContent = getWeekFromDate(row["요청일"]);
        tr.appendChild(tdWeek);
        
        // 요청일
        const tdRequestDate = document.createElement("td");
        tdRequestDate.textContent = row["요청일"] || "";
        tdRequestDate.ondblclick = function() {
          const input = document.createElement("input");
          input.type = "date";
          input.value = row["요청일"] || "";
          input.className = "inline-edit";
          input.onblur = function() {
            row["요청일"] = this.value;
            renderTable();
            saveData();
          };
          tdRequestDate.textContent = "";
          tdRequestDate.appendChild(input);
          input.focus();
        };
        tr.appendChild(tdRequestDate);
        
        // 납기 L/T (자동 계산)
        const tdLT = document.createElement("td");
        tdLT.textContent = calculateLT(row["요청일"], row["완료날짜"], row["출고요청일"]);
        tr.appendChild(tdLT);
        
        // 출고 요청 일
        const tdShipmentRequest = document.createElement("td");
        tdShipmentRequest.textContent = row["출고요청일"] || "";
        
        // 출고 요청 일에 날짜가 입력되면 노란색 배경 적용
        if (row["출고요청일"]) {
          tdShipmentRequest.classList.add("shipment-request-highlight");
        }
        
        tdShipmentRequest.ondblclick = function() {
          const input = document.createElement("input");
          input.type = "date";
          input.value = row["출고요청일"] || "";
          input.className = "inline-edit";
          input.onblur = function() {
            row["출고요청일"] = this.value;
            renderTable();
            saveData();
          };
          tdShipmentRequest.textContent = "";
          tdShipmentRequest.appendChild(input);
          input.focus();
        };
        tr.appendChild(tdShipmentRequest);
        
        // 실출고 (드롭다운)
        const tdActualShipment = document.createElement("td");
        const selectActualShipment = document.createElement("select");
        selectActualShipment.innerHTML = "<option value=\"\">-</option><option value=\"완료\">완료</option>";
        selectActualShipment.value = row["실출고"] || "";
        selectActualShipment.onchange = function() {
          row["실출고"] = this.value;
          renderTable();
          saveData();
        };
        tdActualShipment.appendChild(selectActualShipment);
        tr.appendChild(tdActualShipment);
        
        // 완료 날짜
        const tdCompletionDate = document.createElement("td");
        tdCompletionDate.textContent = row["완료날짜"] || "";
        tdCompletionDate.ondblclick = function() {
          const input = document.createElement("input");
          input.type = "date";
          input.value = row["완료날짜"] || "";
          input.className = "inline-edit";
          input.onblur = function() {
            row["완료날짜"] = this.value;
            renderTable();
            saveData();
          };
          tdCompletionDate.textContent = "";
          tdCompletionDate.appendChild(input);
          input.focus();
        };
        tr.appendChild(tdCompletionDate);
        
        // 전산작업 (드롭다운)
        const tdComputerWork = document.createElement("td");
        const selectComputerWork = document.createElement("select");
        selectComputerWork.innerHTML = "<option value=\"\">-</option><option value=\"완료\">완료</option>";
        selectComputerWork.value = row["전산작업"] || "";
        selectComputerWork.onchange = function() {
          row["전산작업"] = this.value;
          renderTable();
          saveData();
        };
        tdComputerWork.appendChild(selectComputerWork);
        tr.appendChild(tdComputerWork);
        
        // 출고 가능 (자동 계산)
        const tdShipmentPossible = document.createElement("td");
        const status = calculateShipmentStatus(row["내부기안"], row["기안최종승인"]);
        tdShipmentPossible.textContent = status;
        if (status === "가능") {
          tdShipmentPossible.className = "status-possible";
        } else if (status === "불가") {
          tdShipmentPossible.className = "status-impossible";
        }
        tr.appendChild(tdShipmentPossible);
        
        // 나머지 컬럼들
        const otherColumns = [
          "정규출고합포", "요청번호", "기안제목", "기안자", "내부기안", 
          "출고형태", "기안최종승인", "특이사항", "링크"
        ];
        
        otherColumns.forEach(col => {
          const td = document.createElement("td");
          
          if (col === "기안최종승인") {
            // 기안 최종 승인만 드롭다운으로 처리
            const select = document.createElement("select");
            select.innerHTML = "<option value=\"\">-</option><option value=\"O\">O</option>";
            select.value = row[col] || "";
            select.onchange = function() {
              row[col] = this.value;
              renderTable();
              saveData();
            };
            td.appendChild(select);
          } else if (col === "정규출고합포") {
            // 정규출고 합포 드롭다운
            const select = document.createElement("select");
            select.innerHTML = "<option value=\"\">-</option><option value=\"O\">O</option><option value=\"X\">X</option>";
            select.value = row[col] || "";
            select.onchange = function() {
              row[col] = this.value;
              renderTable();
              saveData();
            };
            td.appendChild(select);
          } else if (col === "출고형태") {
            // 출고 형태 드롭다운
            const select = document.createElement("select");
            select.innerHTML = "<option value=\"\">-</option><option value=\"국내택배\">국내택배</option><option value=\"해외출고\">해외출고</option><option value=\"직접수령\">직접수령</option><option value=\"전산처리\">전산처리</option>";
            select.value = row[col] || "";
            select.onchange = function() {
              row[col] = this.value;
              renderTable();
              saveData();
            };
            td.appendChild(select);
          } else if (col === "링크") {
            // 링크 처리
            td.className = "link-cell link-column";
            if (row[col] && row[col].trim()) {
              const link = document.createElement("a");
              link.href = row[col];
              link.target = "_blank";
              link.textContent = row[col];
              td.appendChild(link);
            } else {
              td.textContent = "";
            }
            
            td.ondblclick = function() {
              const input = document.createElement("input");
              input.value = row[col] || "";
              input.className = "inline-edit";
              input.onblur = function() {
                row[col] = this.value;
                renderTable();
                saveData();
              };
              input.onkeydown = function(e) {
                if (e.key === "Enter") this.blur();
              };
              td.textContent = "";
              td.appendChild(input);
              input.focus();
            };
          } else {
            // 일반 텍스트 입력 (내부기안 포함)
            td.textContent = row[col] || "";
            td.ondblclick = function() {
              const input = document.createElement("input");
              input.value = row[col] || "";
              input.className = "inline-edit";
              input.onblur = function() {
                row[col] = this.value;
                renderTable();
                saveData();
              };
              input.onkeydown = function(e) {
                if (e.key === "Enter") this.blur();
              };
              td.textContent = "";
              td.appendChild(input);
              input.focus();
            };
          }
          
          tr.appendChild(td);
        });
        
        // 실출고가 완료인 경우 행 전체를 회색으로
        if (row["실출고"] === "완료") {
          tr.classList.add("row-completed");
        }
        
        tbody.appendChild(tr);
      });
    }

    // 이메일 설정 렌더링
    function renderEmailSettings() {
      const emailList = document.getElementById("email-list");
      emailList.innerHTML = "";
      
      for (const [drafter, email] of Object.entries(emailSettings)) {
        const div = document.createElement("div");
        div.style.marginBottom = "8px";
        div.innerHTML = `
          <span>${drafter}: ${email}</span>
          <button onclick="removeEmail('${drafter}')" class="btn btn-danger" style="margin-left: 8px; padding: 4px 8px;">삭제</button>
        `;
        emailList.appendChild(div);
      }
    }

    // 이메일 추가
    function addEmail() {
      const drafterName = document.getElementById("drafter-name").value.trim();
      const drafterEmail = document.getElementById("drafter-email").value.trim();
      
      if (!drafterName || !drafterEmail) {
        alert("기안자명과 메일 주소를 모두 입력해주세요.");
        return;
      }
      
      emailSettings[drafterName] = drafterEmail;
      document.getElementById("drafter-name").value = "";
      document.getElementById("drafter-email").value = "";
      renderEmailSettings();
      saveEmailSettings();
    }

    // 이메일 삭제
    function removeEmail(drafter) {
      delete emailSettings[drafter];
      renderEmailSettings();
      saveEmailSettings();
    }

    // 메일 발송
    function sendMail() {
      const checked = Array.from(document.querySelectorAll(".data-chk"))
        .filter(chk => chk.checked)
        .map(chk => Number(chk.dataset.idx));
      
      if (!checked.length) {
        alert("메일을 발송할 항목을 선택하세요.");
        return;
      }
      
      const selectedRows = checked.map(idx => dataList[idx]);
      const uniqueDrafters = [...new Set(selectedRows.map(row => row["기안자"]))];
      
      // 기안자별로 메일 주소 확인
      const recipients = [];
      const missingEmails = [];
      
      uniqueDrafters.forEach(drafter => {
        if (emailSettings[drafter]) {
          recipients.push(emailSettings[drafter]);
        } else {
          missingEmails.push(drafter);
        }
      });
      
      if (missingEmails.length > 0) {
        alert(`다음 기안자의 메일 주소가 설정되지 않았습니다:\n${missingEmails.join(", ")}\n\n사용자 설정에서 메일 주소를 등록해주세요.`);
        return;
      }
      
      if (recipients.length === 0) {
        alert("메일 주소가 설정된 기안자가 없습니다.");
        return;
      }
      
      // 메일 작성 모달 표시
      document.getElementById("mail-to").value = recipients.join("; ");
      document.getElementById("mail-subject-input").value = mailTemplate.subject || "사내대체 관리 현황 안내";
      document.getElementById("mail-body").value = mailTemplate.content || "사내대체 관리 현황을 안내드립니다.";
      document.getElementById("mail-modal-bg").style.display = "";
    }

    // 메일 발송 실행
    function sendMailFinal() {
      const to = document.getElementById("mail-to").value;
      const subject = document.getElementById("mail-subject-input").value;
      const body = document.getElementById("mail-body").value;
      
      if (!to || !subject || !body) {
        alert("모든 필드를 입력해주세요.");
        return;
      }
      
      // 아웃룩 메일 작성
      const mailtoLink = `mailto:${to}?subject=${encodeURIComponent(subject)}&body=${encodeURIComponent(body)}`;
      window.open(mailtoLink);
      
      // 모달 닫기
      document.getElementById("mail-modal-bg").style.display = "none";
    }

    // 독립 실행 파일 생성
    function generateStandaloneFile() {
      const currentHTML = document.documentElement.outerHTML;
      
      // 현재 데이터를 HTML에 포함시키는 새로운 HTML 생성
      const standaloneHTML = currentHTML.replace(
        '// 페이지 로드 시 데이터 로드\n    loadData();\n    loadEmailSettings();\n    loadMailTemplate();\n    renderTable();',
        `// 페이지 로드 시 데이터 로드\n    // 내장된 데이터로 초기화\n    dataList = ${JSON.stringify(dataList)};\n    emailSettings = ${JSON.stringify(emailSettings)};\n    mailTemplate = ${JSON.stringify(mailTemplate)};\n    renderTable();`
      );
      
      // 다운로드 링크 생성
      const blob = new Blob([standaloneHTML], { type: 'text/html' });
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = '사내대체관리현황_독립실행파일.html';
      document.body.appendChild(a);
      a.click();
      document.body.removeChild(a);
      URL.revokeObjectURL(url);
      
      document.getElementById("export-result").innerHTML = 
        '<div style="color: green; font-weight: bold;">독립 실행 파일이 생성되었습니다!<br>이 파일을 다른 사람에게 공유하면 모든 데이터가 포함된 상태로 전달됩니다.</div>';
    }

    // 전체 선택 버튼
    document.getElementById("btn-select-all").onclick = function() {
      const checkboxes = document.querySelectorAll(".data-chk");
      const allChecked = Array.from(checkboxes).every(chk => chk.checked);
      
      checkboxes.forEach(chk => {
        chk.checked = !allChecked;
      });
      
      this.textContent = allChecked ? "전체 선택" : "전체 해제";
    };

    // 신규추가 버튼
    document.getElementById("btn-add-new").onclick = function() {
      document.getElementById("modal-bg").style.display = "";
      document.getElementById("add-form").reset();
      
      // 오늘 날짜를 기본값으로 설정
      const today = new Date().toISOString().split("T")[0];
      document.querySelector("input[name=\"요청일\"]").value = today;
    };

    // 모달 취소
    document.getElementById("btn-cancel-modal").onclick = function() {
      document.getElementById("modal-bg").style.display = "none";
    };

    // 메일 모달 취소
    document.getElementById("btn-cancel-mail").onclick = function() {
      document.getElementById("mail-modal-bg").style.display = "none";
    };

    // 신규추가 폼 제출
    document.getElementById("add-form").onsubmit = function(e) {
      e.preventDefault();
      const fd = new FormData(this);
      const newData = {
        "요청일": fd.get("요청일"),
        "법인": fd.get("법인"),
        "출고요청일": fd.get("출고요청일"),
        "실출고": "",
        "완료날짜": "",
        "전산작업": "",
        "정규출고합포": fd.get("정규출고합포"),
        "요청번호": fd.get("요청번호"),
        "기안제목": fd.get("기안제목"),
        "기안자": fd.get("기안자"),
        "내부기안": "",
        "출고형태": fd.get("출고형태"),
        "기안최종승인": "",
        "특이사항": fd.get("특이사항"),
        "링크": fd.get("링크")
      };
      
      dataList.push(newData);
      document.getElementById("modal-bg").style.display = "none";
      applyFilters();
      saveData();
    };

    // 삭제 버튼
    document.getElementById("btn-delete").onclick = function() {
      const checked = Array.from(document.querySelectorAll(".data-chk"))
        .filter(chk => chk.checked)
        .map(chk => Number(chk.dataset.idx));
      
      if (!checked.length) {
        alert("삭제할 항목을 선택하세요.");
        return;
      }
      
      if (confirm("선택한 항목을 삭제하시겠습니까?")) {
        dataList = dataList.filter((_, idx) => !checked.includes(idx));
        applyFilters();
        saveData();
      }
    };

    // 메일 발송 버튼
    document.getElementById("btn-send-mail").onclick = sendMail;

    // 메일 발송 실행 버튼
    document.getElementById("btn-send-mail-final").onclick = sendMailFinal;

    // 독립 실행 파일 생성 버튼
    document.getElementById("btn-generate-standalone").onclick = generateStandaloneFile;

    // 사이드바 메뉴 전환
    document.getElementById("menu-dashboard").onclick = function() {
      document.getElementById("section-dashboard").style.display = "";
      document.getElementById("section-settings").style.display = "none";
      document.getElementById("section-export").style.display = "none";
      this.classList.add("active");
      document.getElementById("menu-settings").classList.remove("active");
      document.getElementById("menu-export").classList.remove("active");
    };

    document.getElementById("menu-settings").onclick = function() {
      document.getElementById("section-dashboard").style.display = "none";
      document.getElementById("section-settings").style.display = "";
      document.getElementById("section-export").style.display = "none";
      this.classList.add("active");
      document.getElementById("menu-dashboard").classList.remove("active");
      document.getElementById("menu-export").classList.remove("active");
      
      // 설정 페이지 로드 시 현재 값들 표시
      document.getElementById("mail-subject").value = mailTemplate.subject;
      document.getElementById("mail-content").value = mailTemplate.content;
      renderEmailSettings();
    };

    document.getElementById("menu-export").onclick = function() {
      document.getElementById("section-dashboard").style.display = "none";
      document.getElementById("section-settings").style.display = "none";
      document.getElementById("section-export").style.display = "";
      this.classList.add("active");
      document.getElementById("menu-dashboard").classList.remove("active");
      document.getElementById("menu-settings").classList.remove("active");
    };

    // 이메일 추가 버튼
    document.getElementById("btn-add-email").onclick = addEmail;

    // 메일 템플릿 저장 버튼
    document.getElementById("btn-save-mail-template").onclick = function() {
      mailTemplate.subject = document.getElementById("mail-subject").value;
      mailTemplate.content = document.getElementById("mail-content").value;
      saveMailTemplate();
      alert("메일 템플릿이 저장되었습니다.");
    };

    // 필터 이벤트 리스너
    document.getElementById("filter-request-date").onchange = applyFilters;
    document.getElementById("filter-drafter").oninput = applyFilters;
    document.getElementById("filter-corporation").onchange = applyFilters;
    document.getElementById("filter-actual-shipment").onchange = applyFilters;
    document.getElementById("filter-shipment-possible").onchange = applyFilters;
    document.getElementById("btn-clear-filter").onclick = clearFilters;

    // 페이지 로드 시 데이터 로드
    loadData();
    loadEmailSettings();
    loadMailTemplate();
    renderTable();
  </script>
</body>
</html>' > substitute_management.html
