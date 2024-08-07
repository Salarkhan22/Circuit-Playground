<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Smart Energy Monitoring Application</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        .container {
            background-color: #fff;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
            width: 90%;
            max-width: 600px;
        }
        h1 {
            text-align: center;
            color: #333;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        th, td {
            padding: 10px;
            border: 1px solid #ddd;
            text-align: left;
        }
        th {
            background-color: #f2f2f2;
        }
        .total-bill {
            font-size: 1.2em;
            font-weight: bold;
            text-align: center;
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Smart Energy Monitoring Application</h1>
        <table>
            <thead>
                <tr>
                    <th>Appliance</th>
                    <th>Consumption (kWh)</th>
                    <th>Charge (INR)</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td>AC</td>
                    <td id="ac">120</td>
                    <td id="ac_charge">373.20</td>
                </tr>
                <tr>
                    <td>Washing Machine</td>
                    <td id="washing_machine">50</td>
                    <td id="washing_machine_charge">155.50</td>
                </tr>
                <tr>
                    <td>TV</td>
                    <td id="tv">30</td>
                    <td id="tv_charge">93.30</td>
                </tr>
                <tr>
                    <td>Fan</td>
                    <td id="fan">20</td>
                    <td id="fan_charge">62.20</td>
                </tr>
                <tr>
                    <td>Microwave Oven</td>
                    <td id="microwave">10</td>
                    <td id="microwave_charge">31.10</td>
                </tr>
                <tr>
                    <td>Refrigerator</td>
                    <td id="refrigerator">60</td>
                    <td id="refrigerator_charge">186.60</td>
                </tr>
            </tbody>
        </table>
        <div class="total-bill">
            Total Monthly Bill: <span id="total_bill">901.90</span> INR
        </div>
    </div>
    <script>
        const unitPrice = 3.11; // price per kWh in INR

        function updateCharges(data) {
            const appliances = ['ac', 'washing_machine', 'tv', 'fan', 'microwave', 'refrigerator'];
            let totalBill = 0;

            appliances.forEach(appliance => {
                const consumption = data[appliance];
                const charge = (consumption * unitPrice).toFixed(2);
                document.getElementById(appliance).textContent = consumption;
                document.getElementById(appliance + '_charge').textContent = charge;
                totalBill += parseFloat(charge);
            });

            document.getElementById('total_bill').textContent = totalBill.toFixed(2);
        }

        async function updateData() {
            try {
                const response = await fetch('/update_data');
                const data = await response.json();
                updateCharges(data);
            } catch (error) {
                console.error('Error fetching data:', error);
            }
        }

        document.addEventListener('DOMContentLoaded', () => {
            // Example initial data
            const initialData = {
                ac: 120,
                washing_machine: 50,
                tv: 30,
                fan: 20,
                microwave: 10,
                refrigerator: 60
            };
            updateCharges(initialData);

            setInterval(updateData, 5000);  // Update every 5 seconds
        });
    </script>
</body>
</html>
