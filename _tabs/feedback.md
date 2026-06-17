---
title: Feedback
icon: fas fa-star
order: 5
script: true
---

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
          pagination: true,
          paginationSize: 15,
          placeholder: "ડેટા લોડ થઈ રહ્યો છે અથવા કોઈ રેકોર્ડ નથી...",
          responsiveLayout: "collapse" 
        });
      }
    }
  });
});
</script>