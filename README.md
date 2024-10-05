<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Excel Cell Finder Game</title>
    <style>
        table {
            border-collapse: collapse;
            margin: 20px 0;
        }
        td, th {
            border: 1px solid black;
            padding: 8px;
            text-align: center;
            width: 40px;
            height: 40px;
        }
        td {
            cursor: pointer;
        }
        #message {
            margin-top: 20px;
            font-size: 18px;
        }
        #score {
            font-weight: bold;
        }
        .correct {
            background-color: lightgreen;
        }
        .wrong {
            background-color: lightcoral;
        }
    </style>
</head>
<body>

<h2>Find the Correct Excel Cell</h2>
<p id="command">Click on the cell: <span id="targetCell"></span></p>
<table>
    <thead>
        <tr>
            <th></th>
            <th>A</th>
            <th>B</th>
            <th>C</th>
            <th>D</th>
            <th>E</th>
            <th>F</th>
            <th>G</th>
            <th>H</th>
            <th>I</th>
            <th>J</th>
        </tr>
    </thead>
    <tbody id="excelGrid">
        <!-- Rows and cells will be dynamically generated here -->
    </tbody>
</table>

<div id="message"></div>

<script>
    const columns = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J'];
    const totalRows = 10;
    let score = 0;
    let attempts = 0;
    let targetCell = '';

    // Function to generate random cell addresses
    function getRandomCell() {
        const randomCol = columns[Math.floor(Math.random() * columns.length)];
        const randomRow = Math.floor(Math.random() * totalRows) + 1;
        return randomCol + randomRow;
    }

    // Function to create the Excel table grid
    function createExcelGrid() {
        const tbody = document.getElementById('excelGrid');
        for (let row = 1; row <= totalRows; row++) {
            const tr = document.createElement('tr');
            const rowHeader = document.createElement('th');
            rowHeader.textContent = row;
            tr.appendChild(rowHeader);
            for (let col = 0; col < columns.length; col++) {
                const td = document.createElement('td');
                td.dataset.cellAddress = columns[col] + row;
                td.addEventListener('click', handleCellClick);
                tr.appendChild(td);
            }
            tbody.appendChild(tr);
        }
    }

    // Function to handle the cell clicks
    function handleCellClick(event) {
        const clickedCell = event.target.dataset.cellAddress;
        attempts++;
        if (clickedCell === targetCell) {
            event.target.classList.add('correct');
            score++;
            document.getElementById('message').innerHTML = `<span style="color: green;">Correct!</span>`;
        } else {
            event.target.classList.add('wrong');
            document.getElementById('message').innerHTML = `<span style="color: red;">Wrong! The correct cell was ${targetCell}.</span>`;
        }

        if (attempts < 10) {
            setNewTargetCell();
        } else {
            document.getElementById('command').textContent = `Game Over! Your score is: ${score}/10`;
            document.querySelectorAll('td').forEach(td => td.removeEventListener('click', handleCellClick)); // Disable clicking after the game
        }
    }

    // Function to set a new target cell
    function setNewTargetCell() {
        targetCell = getRandomCell();
        document.getElementById('targetCell').textContent = targetCell;
    }

    // Initialize the game
    createExcelGrid();
    setNewTargetCell();
</script>

</body>
</html>
