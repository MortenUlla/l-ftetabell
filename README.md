<!DOCTYPE html>
<html lang="no">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Løftekapasitet Kalkulator</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f9f9f9;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        .calculator {
            background-color: #fff;
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            padding: 20px;
            max-width: 400px;
            width: 100%;
        }
        label {
            display: block;
            margin: 10px 0 5px;
        }
        select, input {
            width: 100%;
            padding: 8px;
            margin-bottom: 15px;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
        button {
            width: 100%;
            padding: 10px;
            background-color: #007BFF;
            color: #fff;
            border: none;
            border-radius: 5px;
            font-size: 16px;
            cursor: pointer;
        }
        button:hover {
            background-color: #0056b3;
        }
        p#result {
            margin-top: 15px;
            font-size: 18px;
            text-align: center;
            color: #333;
        }
    </style>
</head>
<body>
    <div class="calculator">
        <h2>Løftekapasitet Kalkulator</h2>
        <label for="dimension">Dimensjon:</label>
        <select id="dimension">
            <option value="M8">VRS - M 8</option>
            <option value="M10">VRS - M 10</option>
            <option value="M12">VRS - M 12</option>
            <option value="M16">VRS - M 16</option>
            <option value="M20">VRS - M 20</option>
            <option value="M24">VRS - M 24</option>
            <option value="M30">VRS - M 30</option>
            <option value="M36">VRS - M 36</option>
            <option value="M42">VRS - M 42</option>
            <option value="M48">VRS - M 48</option>
        </select>
        
        <label for="liftingPoints">Antall løftepunkter:</label>
        <select id="liftingPoints">
            <option value="1">1</option>
            <option value="2">2</option>
            <option value="3&4">3 & 4</option>
        </select>
        
        <label for="angle">Arbeidsvinkel:</label>
        <select id="angle">
            <option value="90">90°</option>
            <option value="0">0°</option>
            <option value="0-45">0-45°</option>
            <option value="45-60">45-60°</option>
        </select>
        
        <button onclick="calculateCapacity()">Beregn</button>
        <p id="result"></p>
    </div>
    
    <script>
        const loadTable = {
            "M8": {"1": {"90": 400, "0": 1000, "0-45": 560, "45-60": 400}, "2": {"90": 800, "0": 2000, "0-45": 1000, "45-60": 750}, "3&4": {"0-45": 840, "45-60": 600}},
            "M10": {"1": {"90": 400, "0": 1000, "0-45": 560, "45-60": 400}, "2": {"90": 800, "0": 2000, "0-45": 1000, "45-60": 750}, "3&4": {"0-45": 840, "45-60": 600}},
            "M12": {"1": {"90": 750, "0": 2000, "0-45": 1000, "45-60": 750}, "2": {"90": 1500, "0": 4000, "0-45": 2000, "45-60": 1500}, "3&4": {"0-45": 1600, "45-60": 1120}},
            "M16": {"1": {"90": 1500, "0": 4000, "0-45": 2240, "45-60": 1500}, "2": {"90": 3000, "0": 8000, "0-45": 4500, "45-60": 3000}, "3&4": {"0-45": 4800, "45-60": 3360}},
            "M20": {"1": {"90": 2000, "0": 5000, "0-45": 2800, "45-60": 2000}, "2": {"90": 4000, "0": 10000, "0-45": 5600, "45-60": 4000}, "3&4": {"0-45": 6000, "45-60": 4500}},
            "M24": {"1": {"90": 2500, "0": 6300, "0-45": 3500, "45-60": 2500}, "2": {"90": 5000, "0": 12600, "0-45": 7000, "45-60": 5000}, "3&4": {"0-45": 8400, "45-60": 6300}},
            "M30": {"1": {"90": 4000, "0": 10000, "0-45": 5600, "45-60": 4000}, "2": {"90": 8000, "0": 20000, "0-45": 11200, "45-60": 8000}, "3&4": {"0-45": 12000, "45-60": 9000}},
            "M36": {"1": {"90": 6000, "0": 15000, "0-45": 8400, "45-60": 6000}, "2": {"90": 12000, "0": 30000, "0-45": 16800, "45-60": 12000}, "3&4": {"0-45": 18000, "45-60": 13500}},
            "M42": {"1": {"90": 8000, "0": 20000, "0-45": 11200, "45-60": 8000}, "2": {"90": 16000, "0": 40000, "0-45": 22400, "45-60": 16000}, "3&4": {"0-45": 24000, "45-60": 18000}},
            "M48": {"1": {"90": 10000, "0": 25000, "0-45": 14000, "45-60": 10000}, "2": {"90": 20000, "0": 50000, "0-45": 28000, "45-60": 20000}, "3&4": {"0-45": 30000, "45-60": 22500}},
        };
        
        function calculateCapacity() {
            const dimension = document.getElementById('dimension').value;
            const liftingPoints = document.getElementById('liftingPoints').value;
            const angle = document.getElementById('angle').value;
            
            if (loadTable[dimension] && loadTable[dimension][liftingPoints] && loadTable[dimension][liftingPoints][angle]) {
                document.getElementById('result').innerText = `Maks løftekapasitet: ${loadTable[dimension][liftingPoints][angle]} kg`;
            } else {
                document.getElementById('result').innerText = "Ingen data tilgjengelig for denne kombinasjonen";
            }
        }
    </script>
</body>
</html>
