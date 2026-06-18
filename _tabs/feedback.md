---
title: Feedback
icon: fas fa-star
order: 5
script: true
---

<style>
  /* --- 1. Tabulator Core & Chirpy Theme Integration --- */
  .tabulator {
    background-color: var(--main-bg) !important;
    color: var(--text-color) !important;
    border: 1px solid var(--tb-border-color, #e9ecef) !important;
    font-family: inherit !important;
    height: auto !important; /* Allows table to expand naturally without double scrollbars */
  }

  /* Header row design */
  .tabulator .tabulator-header {
    background-color: var(--panel-bg, #f8f9fa) !important;
    color: var(--text-color) !important;
    border-bottom: 2px solid var(--tb-border-color, #dee2e6) !important;
  }

  /* Clean styling for header filter input boxes */
  .tabulator .tabulator-header .tabulator-header-filter input {
    background-color: var(--main-bg) !important;
    color: var(--text-color) !important;
    border: 1px solid var(--tb-border-color, #ccc) !important;
    border-radius: 4px;
    padding: 2px 4px !important;
  }

  /* Eliminates inside scrolling to prevent page jumps */
  .tabulator .tabulator-tableholder {
    height: auto !important; 
    overflow: visible !important;
  }

  /* Row configuration */
  .tabulator .tabulator-tableholder .tabulator-table .tabulator-row {
    background-color: var(--main-bg) !important;
    color: var(--text-color) !important;
    border-bottom: 1px solid var(--tb-border-color, #eee) !important;
  }

  /* 🎯 Zebra Striping: Odd & Even row background colors */
  .tabulator .tabulator-tableholder .tabulator-table .tabulator-row.tabulator-row-odd {
    background-color: var(--main-bg) !important;
  }
  .tabulator .tabulator-tableholder .tabulator-table .tabulator-row.tabulator-row-even {
    background-color: var(--panel-bg, #fdfdfd) !important;
  }

  /* 🎯 Your Custom Spacing Fixes (Perfectly Integrated) */
  .tabulator-col-resize-handle {
    height: 0 !important;
  }
  .tabulator .tabulator-row .tabulator-cell {
    padding: 6px 8px !important;
    height: auto !important;
  }

  /* Footer and pagination controls styling */
  .tabulator .tabulator-footer {
    background-color: var(--panel-bg, #f8f9fa) !important;
    color: var(--text-color) !important;
    border-top: 1px solid var(--tb-border-color, #dee2e6) !important;
    padding: 6px 8px !important;
  }

  .tabulator .tabulator-footer .tabulator-page {
    background-color: var(--main-bg) !important;
    color: var(--text-color) !important;
    border: 1px solid var(--tb-border-color, #ccc) !important;
    padding: 3px 8px !important;
    border-radius: 4px;
  }
  
  .tabulator .tabulator-footer .tabulator-page.active {
    background-color: var(--link-color, #007bff) !important;
    color: #fff !important;
  }

  /* Interactive Star rating colors */
  .tabulator-cell .tabulator-star-inactive { color: #ccc !important; }
  .tabulator-cell .tabulator-star-active { color: #ffc107 !important; }

  /* --- 2. Responsive Mobile Card Layout (Auto Collapse) --- */
  @media (max-width: 992px) {
    .tabulator .tabulator-header {
      display: none !important; 
    }
    
    .tabulator .tabulator-row {
      display: block !important;
      height: auto !important;
      padding: 12px !important;
      border: 1px solid var(--tb-border-color, #ccc) !important;
      margin-bottom: 12px !important;
      border-radius: 6px !important;
    }

    /* Keep the zebra background effect on mobile cards too */
    .tabulator .tabulator-row.tabulator-row-even {
      background-color: var(--panel-bg, #fdfdfd) !important;
    }

    .tabulator .tabulator-row .tabulator-cell {
      display: block !important;
      width: 100% !important;
      border: none !important;
      padding: 4px 0 !important;
      white-space: normal !important;
      text-align: left !important;
    }

    /* Automatically prefix labels for non-name fields on mobile views */
    .tabulator .tabulator-row .tabulator-cell[tabulator-field]:not([tabulator-field="નામ"]) {
      padding-left: 5px !important;
    }
    
    .tabulator .tabulator-row .tabulator-cell[tabulator-field]:not([tabulator-field="નામ"])::before {
      content: attr(tabulator-field) ": ";
      font-weight: bold;
      color: var(--link-color, #007bff);
      margin-right: 5px;
    }

    /* Primary highlight for identity column ("નામ") on mobile cards */
    .tabulator .tabulator-row .tabulator-cell[tabulator-field="નામ"] {
      font-size: 1.25rem !important;
      font-weight: bold !important;
      color: var(--text-color) !important;
      border-bottom: 1px dashed var(--tb-border-color, #ccc) !important;
      padding-bottom: 6px !important;
      margin-bottom: 6px !important;
    }
  }
</style>

<div id="feedbackTable" style="margin: 20px 0;"></div>

<link href="https://unpkg.com/tabulator-tables@6.3.0/dist/css/tabulator.min.css" rel="stylesheet">
<script src="https://unpkg.com/tabulator-tables@6.3.0/dist/js/tabulator.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/papaparse@5.5.3/papaparse.min.js"></script>

<script>
document.addEventListener("DOMContentLoaded", function() {
  const CSV_URL = "https://docs.google.com/spreadsheets/d/e/2PACX-1vQvpD3g8-1llhdrJnJfHcYTia2UxD6FiMlNemfDRGlQVWslFi6GVFcjV7tMttbGNdOxPwn6Xg_AWUYG/pub?gid=610263309&single=true&output=csv";

  Papa.parse(CSV_URL, {
    download: true,
    header: true,
    worker: true, /* Parses in the background thread for seamless UI operation */
    fastMode: false, /* Correctly supports line-breaks and multi-line paragraphs */
    skipEmptyLines: true,
    complete: function(res) {
      if (res.data && res.data.length > 0) {
        
        const columns = Object.keys(res.data[0]).map(function(key) {
          let colConfig = {
            title: key,
            field: key,
            headerFilter: true /* Built-in search filtering functionality enabled on desktop */
          };

          /* Rating columns transformation into graphical stars */
          if (key.toLowerCase() === "સ્નેહ મિલન" || key.toLowerCase() === "આયોજન" || key.toLowerCase() === "ભોજન વ્યવસ્થા" || key.toLowerCase() === "બેઠક વ્યવસ્થા" || key.toLowerCase() === "કાર્યકર્તાઓનો સહકાર" ) {
            colConfig.formatter = "star"; 
            colConfig.headerFilter = false; 
            colConfig.hozAlign = "center"; 
            colConfig.width = 120; 
          }

          /* Auto wrap long text descriptions seamlessly */
          if (key.toLowerCase() === "શું સૌથી વધુ ગમીયુ?" || key.toLowerCase() === "ભૂલ કે ફરિયાદ" || key.toLowerCase() === "સુધારો લાવવાની જરૂર છે" || key.toLowerCase() === "દિલથી સંદેશ/સૂચન") {
            colConfig.formatter = "textarea"; 
          }

          /* Identity column profile sizing */
          if (key.toLowerCase() === "નામ") {
            colConfig.hozAlign = "left"; 
            colConfig.width = 150; 
          }

          return colConfig;
        });

        /* Clean and standard Tabulator runtime initialization */
        new Tabulator("#feedbackTable", {
          data: res.data,
          columns: columns,
          layout: "fitColumns", /* Perfectly stretches table headers to 100% width grid */
          pagination: true,
          paginationSize: 12, /* Renders exactly 12 row items natively per viewport view */
          placeholder: "ડેટા લોડ થઈ રહ્યો છે..."
        });
      }
    }
  });
});
</script>
