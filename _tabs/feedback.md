---
title: Feedback
icon: fas fa-star
order: 5
script: true
---

<style>
  /* --- 1. Tabulator General and Chirpy Theme Matching --- */
  .tabulator {
    background-color: var(--main-bg) !important;
    color: var(--text-color) !important;
    border: 1px solid var(--tb-border-color, #e9ecef) !important;
    font-family: inherit !important;
    height: auto !important; /* Allows table to expand naturally */
  }

  /* Header row style */
  .tabulator .tabulator-header {
    background-color: var(--panel-bg, #f8f9fa) !important;
    color: var(--text-color) !important;
    border-bottom: 2px solid var(--tb-border-color, #dee2e6) !important;
  }

  /* Filter input boxes inside header */
  .tabulator .tabulator-header .tabulator-header-filter input {
    background-color: var(--main-bg) !important;
    color: var(--text-color) !important;
    border: 1px solid var(--tb-border-color, #ccc) !important;
    border-radius: 4px;
    padding: 2px 4px !important; /* Reduced padding for tighter look */
  }

  /* Data rows container */
  .tabulator .tabulator-tableholder {
    height: auto !important; 
    overflow: visible !important;
  }

  /* Individual data row style - REDUCED SPACING HERE */
  .tabulator .tabulator-tableholder .tabulator-table .tabulator-row {
    background-color: var(--main-bg) !important;
    color: var(--text-color) !important;
    border-bottom: 1px solid var(--tb-border-color, #eee) !important;
    min-height: 40px !important; /* Reduced from 60px */
    height: auto !important; /* Allows cell to size independently */
  }

  /* Reduce spacing inside cells */
  .tabulator .tabulator-row .tabulator-cell {
    padding: 6px 4px !important; /* Tighter padding to eliminate extra vertical spacing */
    height: auto !important;
  }

  /* Zebra striping for even rows */
  .tabulator .tabulator-tableholder .tabulator-table .tabulator-row.tabulator-row-even {
    background-color: var(--panel-bg, #fdfdfd) !important;
  }

  /* Footer and pagination controls */
  .tabulator .tabulator-footer {
    background-color: var(--panel-bg, #f8f9fa) !important;
    color: var(--text-color) !important;
    border-top: 1px solid var(--tb-border-color, #dee2e6) !important;
    padding: 4px 8px !important;
  }

  .tabulator .tabulator-footer .tabulator-page {
    background-color: var(--main-bg) !important;
    color: var(--text-color) !important;
    border: 1px solid var(--tb-border-color, #ccc) !important;
    padding: 3px 6px !important;
  }
  
  .tabulator .tabulator-footer .tabulator-page.active {
    background-color: var(--link-color, #007bff) !important;
    color: #fff !important;
  }

  /* Star colors */
  .tabulator-cell .tabulator-star-inactive {
    color: #ccc !important; 
  }
  .tabulator-cell .tabulator-star-active {
    color: #ffc107 !important; 
  }

  /* --- 2. Responsive Mobile Layout: Collapse Everything Except "નામ" --- */
  @media (max-width: 992px) {
    .tabulator .tabulator-header {
      display: none !important; 
    }
    
    .tabulator .tabulator-row {
      display: block !important;
      height: auto !important;
      padding: 12px !important;
      border: 1px solid var(--tb-border-color, #ccc) !important;
      margin-bottom: 10px !important;
      border-radius: 6px !important;
    }

    .tabulator .tabulator-row .tabulator-cell {
      display: block !important;
      width: 100% !important;
      border: none !important;
      padding: 4px 0 !important;
      white-space: normal !important;
      text-align: left !important;
    }

    .tabulator .tabulator-row .tabulator-cell[tabulator-field]:not([tabulator-field="નામ"]) {
      padding-left: 5px !important;
    }
    
    .tabulator .tabulator-row .tabulator-cell[tabulator-field]:not([tabulator-field="નામ"])::before {
      content: attr(tabulator-field) ": ";
      font-weight: bold;
      color: var(--link-color, #007bff);
      margin-right: 5px;
    }

    .tabulator .tabulator-row .tabulator-cell[tabulator-field="નામ"] {
      font-size: 1.25rem !important;
      font-weight: bold !important;
      color: var(--text-color) !important;
      border-bottom: 1px dashed var(--tb-border-color, #ccc) !important;
      padding-bottom: 8px !important;
      margin-bottom: 8px !important;
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
    worker: true, /* Run parser in background thread */
    fastMode: false, /* Keep false to handle multiline paragraphs correctly */
    skipEmptyLines: true,
    complete: function(res) {
      if (res.data && res.data.length > 0) {
        
        /* Map dynamically generated headers from Google Sheet */
        const columns = Object.keys(res.data[0]).map(function(key) {
          
          let colConfig = {
            title: key,
            field: key,
            headerFilter: true /* Keeps column filters enabled on desktop view */
          };

          /* Format 1 to 5 numeric values to interactive graphical star icons */
          if (key.toLowerCase() === "સ્નેહ મિલન" || key.toLowerCase() === "આયોજન" || key.toLowerCase() === "ભોજન વ્યવસ્થા" || key.toLowerCase() === "બેઠક વ્યવસ્થા" || key.toLowerCase() === "કાર્યકર્તાઓનો સહકાર" ) {
            colConfig.formatter = "star"; 
            colConfig.headerFilter = false; /* Disable filter input boxes specifically for star ratings */
            colConfig.hozAlign = "center"; 
            colConfig.width = 120; 
          }

          /* Wrap long feedback comments and multi-line paragraphs cleanly */
          if (key.toLowerCase() === "શું સૌથી વધુ ગમીયુ?" || key.toLowerCase() === "ભૂલ કે ફરિયાદ" || key.toLowerCase() === "સુધારો લાવવાની જરૂર છે" || key.toLowerCase() === "દિલથી સંદેશ/સૂચન") {
            colConfig.formatter = "textarea"; 
          }

          /* Configurations for the primary identity column */
          if (key.toLowerCase() === "નામ") {
            colConfig.hozAlign = "left"; 
            colConfig.width = 150; 
          }

          return colConfig;
        });

        /* Main Tabulator initialization */
        new Tabulator("#feedbackTable", {
          data: res.data,
          columns: columns,
          layout: "fitColumns", /* Clean horizontal distribution on desktop screen */
          /* 🚀 Removed 'dataLayout: "textarea"' to fix the global row-stretching / heavy spacing bug */
          pagination: true,
          paginationSize: 12, /* Renders exactly 12 records per page view */
          placeholder: "ડેટા લોડ થઈ રહ્યો છે અથવા કોઈ રેકોર્ડ નથી..."
        });
      }
    }
  });
});
</script>
