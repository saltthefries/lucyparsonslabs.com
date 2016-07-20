---
layout: page
title: "1505 Audit Results"
header: no
permalink: /fullaudit/
---

Over the last year and a half, the Lucy Parsons Labs has obtained approximately six hundred checks related to the Chicago Police Department's of civil asset forfeiture. This webpage is a searchable and sortable repository of all the checks we obtained, a link to the Freedom of Information Act request, the PDF result of the response and the check amounts. 

      <div id='table-container'></div>

    </div><!-- /.container -->

    <!-- Bootstrap core JavaScript
    ================================================== -->
    <!-- Placed at the end of the document so the pages load faster -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.2/jquery.min.js"></script>
    <script src="js/bootstrap.min.js"></script>
    <script src="js/jquery.csv.min.js"></script>
    <script src="js/jquery.dataTables.min.js"></script>
    <script src="js/dataTables.bootstrap.js"></script>
    <script src='js/csv_to_html_table.js'></script>

    <script>
      init_table({
        csv_path: 'data/July1505Data.csv', 
        element: 'table-container', 
        allow_download: false,
        csv_options: {separator: ',', delimiter: '"'},
        datatables_options: {"paging": false}
      });
    </script>
