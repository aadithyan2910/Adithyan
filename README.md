<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Upload, Add Data, and Click to Add Images in Table</title>
    <style>
        body {
            background-image: url('leaf-background.jpg'); /* Replace with your leaf image URL */
            background-size: cover;
            background-position: center;
            background-repeat: no-repeat;
            font-family: Arial, sans-serif;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
            background: rgba(255, 255, 255, 0.8);
        }
        th, td {
            border: 1px solid black;
            padding: 8px;
            text-align: left;
            cursor: pointer;
        }
        th {
            background-color: #f2f2f2;
        }
        img {
            width: 50px;
            height: 50px;
            object-fit: cover;
        }
    </style>
</head>
<body>
    <h2>Upload a CSV File</h2>
    <input type="file" id="fileInput" accept=".csv">
    
    <h2>Add Data</h2>
    <input type="text" id="newData" placeholder="Enter comma-separated values">
    <button onclick="addRow()">Add Row</button>
    
    <input type="file" id="imageInput" accept="image/*" style="display: none;">
    <table id="dataTable"></table>

    <script>
        document.getElementById('fileInput').addEventListener('change', function(event) {
            const file = event.target.files[0];
            if (!file) return;

            const reader = new FileReader();
            reader.onload = function(e) {
                const content = e.target.result;
                displayTable(content);
            };
            reader.readAsText(file);
        });

        function displayTable(csv) {
            const rows = csv.split('\n');
            const table = document.getElementById('dataTable');
            table.innerHTML = '';
            
            rows.forEach((row, index) => {
                const cols = row.split(',');
                const tr = document.createElement('tr');
                cols.forEach(col => {
                    const cell = document.createElement(index === 0 ? 'th' : 'td');
                    cell.textContent = col.trim();
                    cell.addEventListener('click', function() { editCell(cell); });
                    tr.appendChild(cell);
                });
                table.appendChild(tr);
            });
        }
        
        function addRow() {
            const inputData = document.getElementById('newData').value;
            if (!inputData) return;

            const table = document.getElementById('dataTable');
            const tr = document.createElement('tr');
            const cols = inputData.split(',');
            
            cols.forEach(col => {
                const td = document.createElement('td');
                td.textContent = col.trim();
                td.addEventListener('click', function() { editCell(td); });
                tr.appendChild(td);
            });
            
            table.appendChild(tr);
            document.getElementById('newData').value = ''; // Clear input field
        }

        function editCell(cell) {
            const currentValue = cell.textContent;
            const input = document.createElement('input');
            input.type = 'text';
            input.value = currentValue;
            input.onblur = function() {
                cell.textContent = input.value;
                cell.addEventListener('click', function() { editCell(cell); });
            };
            input.onkeydown = function(event) {
                if (event.key === 'Enter') {
                    input.blur();
                }
            };
            cell.innerHTML = '';
            cell.appendChild(input);
            input.focus();
        }
    </script>
</body>
</html>
