# Egg-opener-6
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Egg Opener Game</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #f7f7f7;
            padding: 20px;
        }
        .button {
            padding: 15px 25px;
            font-size: 20px;
            color: #fff;
            background-color: #007BFF;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            margin: 5px;
        }
        .button:hover {
            background-color: #0056b3;
        }
        .output {
            margin-top: 20px;
            font-size: 24px;
            color: #333;
        }
        .pet-popup {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: white;
            border: 2px solid #ccc;
            border-radius: 10px;
            padding: 20px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
            opacity: 0;
            animation: fadeInScale 0.7s forwards;
        }
        @keyframes fadeInScale {
            from {
                opacity: 0;
                transform: translate(-50%, -50%) scale(0.8);
            }
            to {
                opacity: 1;
                transform: translate(-50%, -50%) scale(1);
            }
        }
        .inventory {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.8);
            display: none;
            flex-wrap: wrap;
            align-items: center;
            justify-content: center;
            overflow-y: auto;
        }
        .inventory.active {
            display: flex;
        }
        .pet-box {
            width: 120px;
            height: 150px;
            margin: 10px;
            background-color: #fff;
            border-radius: 10px;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            position: relative;
        }
        .pet-box .count {
            position: absolute;
            top: 5px;
            right: 5px;
            background-color: #007BFF;
            color: white;
            font-size: 12px;
            padding: 3px 6px;
            border-radius: 50%;
        }
        .pet-box .name {
            font-size: 14px;
            font-weight: bold;
            margin-top: 10px;
        }
        .close-button {
            position: absolute;
            top: 10px;
            right: 10px;
            background-color: #ff0000;
            color: white;
            border: none;
            border-radius: 50%;
            width: 30px;
            height: 30px;
            font-size: 18px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <h1>Egg Opener Simulator</h1>
    <div>
        <button class="button" onclick="openEgg()">Open Egg</button>
        <button class="button" onclick="toggleInventory()">Inventar</button>
    </div>
    <div class="output" id="output">Click the button to open an egg!</div>
    <div id="popupContainer"></div>

    <div class="inventory" id="inventory">
        <button class="close-button" onclick="toggleInventory()">×</button>
        <!-- Pet boxes will be dynamically created here -->
    </div>

    <script>
        // Pets and their probabilities
        const pets = [
            { name: "Common Cat", chance: 60.0, count: 0 },
            { name: "Common Dog", chance: 20.0, count: 0 },
            { name: "Uncommon Bird", chance: 10.0, count: 0 },
            { name: "Rare Fox", chance: 5.0, count: 0 },
            { name: "Epic Dragon", chance: 2.5, count: 0 },
            { name: "Legendary Unicorn", chance: 1.0, count: 0 },
            { name: "Mythic Phoenix", chance: 0.5, count: 0 },
            { name: "Divine Tiger", chance: 0.25, count: 0 },
            { name: "Godly Wolf", chance: 0.1, count: 0 },
            { name: "Celestial Whale", chance: 0.05, count: 0 },
            { name: "Stellar Griffin", chance: 0.02, count: 0 },
            { name: "Galactic Serpent", chance: 0.01, count: 0 },
            { name: "Astral Beast", chance: 0.005, count: 0 },
            { name: "Ethereal Spirit", chance: 0.001, count: 0 },
            { name: "Ultimate Egglord", chance: 0.00001, count: 0 },
        ];

        // Calculate total chance
        const totalChance = pets.reduce((sum, pet) => sum + pet.chance, 0);

        // Function to draw a random pet
        function openEgg() {
            const random = Math.random() * totalChance;
            let cumulativeChance = 0;

            for (const pet of pets) {
                cumulativeChance += pet.chance;
                if (random <= cumulativeChance) {
                    pet.count++;
                    showPopup(pet.name);
                    return;
                }
            }
        }

        // Function to display popup with animation
        function showPopup(petName) {
            const popupContainer = document.getElementById("popupContainer");

            // Clear previous popup
            popupContainer.innerHTML = "";

            // Create popup element
            const popup = document.createElement("div");
            popup.className = "pet-popup";
            popup.textContent = `You got: ${petName}`;

            // Add popup to container
            popupContainer.appendChild(popup);

            // Remove popup after 3 seconds
            setTimeout(() => {
                popup.remove();
            }, 3000);
        }

        // Toggle inventory overlay
        function toggleInventory() {
            const inventory = document.getElementById("inventory");
            inventory.classList.toggle("active");

            if (inventory.classList.contains("active")) {
                renderInventory();
            }
        }

        // Render inventory
        function renderInventory() {
            const inventory = document.getElementById("inventory");
            inventory.innerHTML = '<button class="close-button" onclick="toggleInventory()">×</button>'; // Reset inventory content

            pets.forEach(pet => {
                const petBox = document.createElement("div");
                petBox.className = "pet-box";

                // Add pet count
                const countBadge = document.createElement("div");
                countBadge.className = "count";
                countBadge.textContent = pet.count;
                petBox.appendChild(countBadge);

                // Add pet name
                const petName = document.createElement("div");
                petName.className = "name";
                petName.textContent = pet.name;
                petBox.appendChild(petName);

                inventory.appendChild(petBox);
            });
        }
    </script>
</body>
</html>
