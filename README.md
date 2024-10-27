# Prototype.html1
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Car Parking Application</title>
    <style>
        * {
            box-sizing: border-box;
        }

        body {
            font-family: Arial, sans-serif;
            background-image: url('https://png.pngtree.com/thumb_back/fw800/background/20230902/pngtree-cars-parked-in-an-underground-parking-garage-with-yellow-image_13136555.jpg');
            background-size: cover;
            margin: 0;
            padding: 20px;
            color: white;
        }

        .container {
            max-width: 600px;
            margin: 0 auto;
            text-align: center;
            background: rgba(255, 255, 255, 0.8);
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
        }

        h1 {
            margin-bottom: 20px;
            color: #333;
        }

        form {
            margin-bottom: 20px;
        }

        input {
            margin: 10px 0;
            padding: 10px;
            width: calc(100% - 22px);
            border: 1px solid #ddd;
            border-radius: 5px;
        }

        button {
            padding: 10px 20px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        button:hover {
            background-color: #45a049;
        }

        .parking-lot {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .spot {
            width: 100%;
            padding: 20px;
            background-color: #ccc;
            border-radius: 5px;
            font-size: 1.5em;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: background-color 0.3s;
        }

        .available {
            background-color: #4CAF50;
            color: white;
        }

        .occupied {
            background-color: #F44336;
            color: white;
        }

        #receipt {
            background-color: #e0f7fa; /* Light blue background for the receipt */
            border: 1px solid #ddd;
            padding: 20px;
            border-radius: 5px;
            color: #333; /* Darker text for better readability */
        }

        .hidden {
            display: none;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Car Parking Application</h1>
        <form id="parking-form">
            <input type="text" id="vehicle-number" placeholder="Vehicle Number" required>
            <input type="text" id="license-details" placeholder="License Details" required>
            <input type="number" id="time-period" placeholder="Time Period (hours)" min="1" required>
            <button type="submit">Park Vehicle</button>
        </form>

        <div class="parking-lot" id="parking-lot">
            <div class="spot available">1</div>
            <div class="spot available">2</div>
            <div class="spot available">3</div>
            <div class="spot available">4</div>
            <div class="spot occupied">5</div>
            <div class="spot available">6</div>
            <div class="spot occupied">7</div>
            <div class="spot available">8</div>
            <div class="spot available">9</div>
            <div class="spot available">10</div>
        </div>

        <div id="receipt" class="hidden">
            <h2>Receipt</h2>
            <p id="receipt-details"></p>
            <button onclick="resetApp()">New Entry</button>
        </div>
    </div>

    <script>
        document.getElementById('parking-form').addEventListener('submit', function(event) {
            event.preventDefault();

            const vehicleNumber = document.getElementById('vehicle-number').value;
            const licenseDetails = document.getElementById('license-details').value;
            const timePeriod = parseFloat(document.getElementById('time-period').value);

            if (timePeriod < 1) {
                alert('Please enter a valid positive time period.');
                return;
            }

            const chargePerHour = 100; // Set your rate here in INR
            const totalCharge = timePeriod * chargePerHour;

            const spots = document.querySelectorAll('.spot.available');
            if (spots.length > 0) {
                const allocatedSpot = spots[0];
                allocatedSpot.classList.remove('available');
                allocatedSpot.classList.add('occupied');
                allocatedSpot.textContent = vehicleNumber;

                const receiptDetails = `License Details: ${licenseDetails}<br>
                                        Time Period: ${timePeriod} hours<br>
                                        Total Charge: ₹${totalCharge.toFixed(2)}<br>
                                        vehicle Number: ${allocatedSpot.textContent}`;
                
                document.getElementById('receipt-details').innerHTML = receiptDetails;
                document.getElementById('receipt').classList.remove('hidden');

                // Reset the form
                document.getElementById('parking-form').reset();
            } else {
                alert('No available parking spots!');
            }
        });

        function resetApp() {
            const occupiedSpots = document.querySelectorAll('.spot.occupied');
            occupiedSpots.forEach(spot => {
                spot.classList.remove('occupied');
                spot.classList.add('available');
                spot.textContent = '';
            });
            document.getElementById('receipt').classList.add('hidden');
        }
    </script>
</body>
</html>
