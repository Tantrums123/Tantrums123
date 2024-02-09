<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Steam Ban System</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }

        label {
            display: block;
            margin-bottom: 5px;
        }

        input {
            margin-bottom: 10px;
        }

        button {
            padding: 8px;
            cursor: pointer;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }

        th, td {
            border: 1px solid #ddd;
            padding: 8px;
            text-align: left;
        }

        th {
            background-color: #f2f2f2;
        }
    </style>
</head>
<body>

    <h1>Steam Ban System</h1>

    <label for="steamId">Steam ID:</label>
    <input type="text" id="steamId" placeholder="Enter Steam ID">

    <button onclick="searchSteamId()">Search Steam ID</button>

    <table id="banTable">
        <thead>
            <tr>
                <th>Steam ID</th>
                <th>Photo URL</th>
                <th>Ban Reason</th>
            </tr>
        </thead>
        <tbody id="banTableBody"></tbody>
    </table>

    <script>
        function searchSteamId() {
            const steamId = document.getElementById('steamId').value;

            // Assuming a server-side API for handling database interactions
            fetch(`/api/search?steamId=${steamId}`)
                .then(response => response.json())
                .then(data => {
                    const banTableBody = document.getElementById('banTableBody');
                    let found = false;

                    // Check if the Steam ID is already in the table
                    for (let i = 0; i < banTableBody.rows.length; i++) {
                        const rowSteamId = banTableBody.rows[i].cells[0].innerHTML;
                        if (rowSteamId === steamId) {
                            found = true;
                            break;
                        }
                    }

                    // If Steam ID is not found, add a new row; otherwise, update the existing row
                    if (!found) {
                        const newRow = banTableBody.insertRow();
                        const cell1 = newRow.insertCell(0);
                        const cell2 = newRow.insertCell(1);
                        const cell3 = newRow.insertCell(2);

                        if (data.error) {
                            cell1.innerHTML = steamId;
                            cell2.innerHTML = "N/A";
                            cell3.innerHTML = "Not found";
                            alert(`Steam ID ${steamId} was not found.`);
                        } else {
                            cell1.innerHTML = data.steamId;
                            cell2.innerHTML = data.photoUrl;
                            cell3.innerHTML = data.banReason;
                            alert(`Steam ID ${data.steamId} has been added to the ban list.`);
                        }
                    } else {
                        alert(`Steam ID ${steamId} is already in the ban list. Updating information.`);
                        // You can choose to update the existing row with the latest information here
                    }
                })
                .catch(error => {
                    console.error('Error:', error);
                });
        }
    </script>

</body>
</html>
