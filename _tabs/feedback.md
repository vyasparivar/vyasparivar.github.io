---
title: Feedback
icon: fas fa-star
order: 5
script: true
---

<style>
  /* આખું ટેબલ અને એના હેડરનો કલર ચિર્પી થીમ જેવો કરવા */
  .tabulator {
    background-color: var(--main-bg) !important;
    color: var(--text-color) !important;
    border: 1px solid var(--tb-border-color, #e9ecef) !important;
    font-family: inherit !important;
  }

  /* હેડર રો (Header Row) નો કલર */
  .tabulator .tabulator-header {
    background-color: var(--panel-bg, #f8f9fa) !important;
    color: var(--text-color) !important;
    border-bottom: 2px solid var(--tb-border-color, #dee2e6) !important;
  }

  /* હેડરની અંદરના ફિલ્ટર બોક્સ */
  .tabulator .tabulator-header .tabulator-header-filter input {
    background-color: var(--main-bg) !important;
    color: var(--text-color) !important;
    border: 1px solid var(--tb-border-color, #ccc) !important;
    border-radius: 4px;
    padding: 4px;
  }

  /* ટેબલની અંદરની રો (Rows) નો કલર */
  .tabulator .tabulator-tableholder .tabulator-table .tabulator-row {
    background-color: var(--main-bg) !important;
    color: var(--text-color) !important;
    border-bottom: 1px solid var(--tb-border-color, #eee) !important;
  }

  /* એક છોડીને એક રો નો કલર (Aka Zebra Striping) */
  .tabulator .tabulator-tableholder .tabulator-table .tabulator-row.tabulator-row-even {
    background-color: var(--panel-bg, #fdfdfd) !important;
  }

  /* પેજિનેશન બાર (નીચેનું નેવિગેશન) */
  .tabulator .tabulator-footer {
    background-color: var(--panel-bg, #f8f9fa) !important;
    color: var(--text-color) !important;
    border-top: 1px solid var(--tb-border-color, #dee2e6) !important;
  }

  /* પેજિનેશનના બટન્સ */
  .tabulator .tabulator-footer .tabulator-page {
    background-color: var(--main-bg) !important;
    color: var(--text-color) !important;
    border: 1px solid var(--tb-border-color, #ccc) !important;
  }
  
  .tabulator .tabulator-footer .tabulator-page.active {
    background-color: var(--link-color, #007bff) !important;
    color: #fff !important;
  }
</style>

<div id="feedbackTable" style="margin: 20px 0;"></div>

<link href="https://unpkg.com/tabulator-tables@6.3.0/dist/css/tabulator.min.css" rel="stylesheet">
<script src="https://unpkg.com/tabulator-tables@6.3.0/dist/js/tabulator.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/papaparse@5.5.3/papaparse.min.js"></script>

<style>
  .tabulator-cell .tabulator-star-inactive {
    color: #ccc !important; 
  }
  .tabulator-cell .tabulator-star-active {
    color: #ffc107 !important; 
  }
</style>

<script>
document.addEventListener("DOMContentLoaded", function() {
  const CSV_URL = "https://docs.google.com/spreadsheets/d/e/2PACX-1vQvpD3g8-1llhdrJnJfHcYTia2UxD6FiMlNemfDRGlQVWslFi6GVFcjV7tMttbGNdOxPwn6Xg_AWUYG/pub?gid=610263309&single=true&output=csv";

  Papa.parse(CSV_URL, {
    download: true,
    header: true,
    worker: true,
    fastMode: true,
    skipEmptyLines: true,
    complete: function(res) {
      if (res.data && res.data.length > 0) {
        
        const columns = Object.keys(res.data[0]).map(function(key) {
          
          let colConfig = {
            title: key,
            field: key,
            headerFilter: true
          };

          if (key.toLowerCase() === "સ્નેહ મિલન" || key.toLowerCase() === "આયોજન" || key.toLowerCase() === "ભોજન વ્યવસ્થા" || key.toLowerCase() === "બેઠક વ્યવસ્થા" || key.toLowerCase() === "કાર્યકર્તાઓનો સહકાર" ) {
            colConfig.formatter = "star"; 
            colConfig.headerFilter = false; 
            colConfig.hozAlign = "center"; 
            colConfig.width = 120; 
          }

          /*if (key.toLowerCase() === "comment")*/else {
            colConfig.formatter = "textarea"; 
            colConfig.widthGrow = 2; 
          }

          return colConfig;
        });

        new Tabulator("#feedbackTable", {
          data: res.data,
          columns: columns,
          layout: "fitColumns",
          height: "600px",
          progressiveRender: true,
          pagination: true,
          paginationSize: 5,
          placeholder: "ડેટા લોડ થઈ રહ્યો છે અથવા કોઈ રેકોર્ડ નથી...",
          responsiveLayout: "collapse" 
        });
      }
    }
  });
});
</script>