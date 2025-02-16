<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Code Reveal Challenge</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
            background-color: #0a1929;
        }

        .container {
            max-width: 600px;
            width: 100%;
            text-align: center;
        }

        .image-container {
            margin: 20px 0;
        }
        
        .image-container img {
            border-radius: 15px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.3);
        }

        @keyframes shake {
            0% { transform: translate(-50%, 0); filter: blur(0); }
            10% { transform: translate(-50%, 0) translateX(9px); filter: blur(1px); }
            20% { transform: translate(-50%, 0) translateX(-8px); filter: blur(1px); }
            30% { transform: translate(-50%, 0) translateX(7px); filter: blur(1px); }
            40% { transform: translate(-50%, 0) translateX(-6px); filter: blur(0.5px); }
            50% { transform: translate(-50%, 0) translateX(5px); filter: blur(0.5px); }
            60% { transform: translate(-50%, 0) translateX(-4px); filter: blur(0.5px); }
            70% { transform: translate(-50%, 0) translateX(3px); filter: blur(0px); }
            80% { transform: translate(-50%, 0) translateX(-2px); }
            90% { transform: translate(-50%, 0) translateX(1px); }
            100% { transform: translate(-50%, 0); }
        }

        .error-popup {
            position: fixed;
            top: 20px;
            left: 50%;
            transform: translate(-50%, 0);
            background-color: #ff3333;
            color: white;
            padding: 8px 16px;
            border-radius: 20px;
            font-size: 14px;
            animation: shake 2s cubic-bezier(0.45, 0.05, 0.55, 0.95) forwards;
            display: none;
        }
            0% { border-color: #ff0000; }
            17% { border-color: #ff8800; }
            33% { border-color: #ffff00; }
            50% { border-color: #00ff00; }
            67% { border-color: #0099ff; }
            83% { border-color: #6633ff; }
            100% { border-color: #ff0000; }
        }

        @keyframes rainbowBackground {
            0% { background-color: #ff0000; }
            17% { background-color: #ff8800; }
            33% { background-color: #ffff00; }
            50% { background-color: #00ff00; }
            67% { background-color: #0099ff; }
            83% { background-color: #6633ff; }
            100% { background-color: #ff0000; }
        }

        .code-input {
            margin: 20px 0;
        }

        input[type="text"] {
            padding: 10px;
            font-size: 16px;
            width: 200px;
            margin-right: 10px;
            background: transparent;
            border: 3px solid #ff0000;
            border-radius: 8px;
            color: white;
            animation: rainbow 5s linear infinite;
        }

        button {
            padding: 10px 20px;
            font-size: 16px;
            background-color: #ff0000;
            color: white;
            border: none;
            cursor: pointer;
            border-radius: 8px;
            animation: rainbowBackground 5s linear infinite;
            transition: transform 0.2s;
        }

        button:hover {
            transform: scale(1.05);
        }

        .hint-container {
            margin: 20px 0;
            padding: 15px;
            background-color: rgba(255, 255, 255, 0.1);
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }

        .hint-progress {
            font-family: monospace;
            font-size: 24px;
            letter-spacing: 8px;
            margin: 20px 0;
            display: flex;
            flex-direction: column;
            gap: 10px;
            align-items: center;
        }
        .hint-line {
            width: 100%;
            text-align: center;
        }

        .timer {
            font-size: 14px;
            color: #ffffff;
            margin-top: 10px;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="image-container">
            <img src="/api/placeholder/400/300" alt="Challenge Image">
        </div>
        
        <div class="hint-progress" id="hintProgress"></div>

        <div class="code-input">
            <input type="text" id="codeInput" placeholder="..." maxlength="6">
            <button onclick="checkCode()">submit</button>
        </div>

        <div class="error-popup" id="errorPopup">wrong</div>

        <div class="hint-container">
            <h3>hint</h3>
            <div class="timer" id="timer"></div>
        </div>
    </div>

    <script>
        const _0x5f2c = {
            k: '88dff88e38f',
            s: 'ryansex'
        };
        // Obscure the actual code
        const correctCode = (() => {
            const shift = 3;
            const base = _0x5f2c.s;
            return base; // The deobfuscation happens in the check
        })();

        const hints = [
            "The first letter is 'r'",
            "The second letter is 'y'",
            "The third letter is 'a'",
            "The fourth letter is 'n'",
            "The fifth letter is 's'",
            "The sixth letter is 'e'",
            "The final letter is 'x'"
        ];

        // Get initial state or set default
        let startDate = localStorage.getItem('startDate');
        if (!startDate) {
            startDate = new Date().toISOString();
            localStorage.setItem('startDate', startDate);
        }

        function updateRevealedLetters() {
            const daysPassed = Math.floor((new Date() - new Date(startDate)) / (1000 * 60 * 60 * 24));
            const hintProgressDiv = document.getElementById('hintProgress');
            hintProgressDiv.innerHTML = ''; // Clear existing hints
            
            // Show progress for each day that has passed
            for (let day = 0; day <= Math.min(daysPassed, correctCode.length - 1); day++) {
                const hintLine = document.createElement('div');
                hintLine.className = 'hint-line';
                let line = '';
                
                // Build the line with revealed letters and dashes
                for (let i = 0; i < correctCode.length; i++) {
                    if (i <= day) {
                        line += correctCode[i].toUpperCase();
                    } else {
                        line += '-';
                    }
                    if (i < correctCode.length - 1) {
                        line += ' '; // Add space between characters
                    }
                }
                
                hintLine.textContent = line;
                hintProgressDiv.appendChild(hintLine);
            }
            
            // Update hint text
            if (daysPassed < hints.length) {
                document.getElementById('hint').textContent = hints[daysPassed];
            }
        }

        function updateTimer() {
            const now = new Date();
            const tomorrow = new Date(now);
            tomorrow.setDate(tomorrow.getDate() + 1);
            tomorrow.setHours(0, 0, 0, 0);
            
            const timeLeft = tomorrow - now;
            const hours = Math.floor(timeLeft / (1000 * 60 * 60));
            const minutes = Math.floor((timeLeft % (1000 * 60 * 60)) / (1000 * 60));
            const seconds = Math.floor((timeLeft % (1000 * 60)) / 1000);
            
            document.getElementById('timer').textContent = 
                `${hours}h ${minutes}m ${seconds}s`;
        }

        function checkCode() {
            const input = document.getElementById('codeInput').value.toLowerCase();
            if (input === correctCode) {
                // Do nothing for now, will be implemented later
            } else {
                const errorPopup = document.getElementById('errorPopup');
                errorPopup.style.display = 'block';
                errorPopup.style.animation = 'none';
                errorPopup.offsetHeight; // Trigger reflow
                errorPopup.style.animation = 'shake 0.5s ease-in-out';
                
                setTimeout(() => {
                    errorPopup.style.display = 'none';
                }, 1900); // Set to slightly less than animation duration to prevent gap
            }
            document.getElementById('codeInput').value = ''; // Clear input
        }

        // Initialize and start timers
        updateRevealedLetters();
        setInterval(updateTimer, 1000);
        setInterval(updateRevealedLetters, 60000); // Check for new reveals every minute

        // Reset button for testing (you can remove this in production)
        function resetProgress() {
            localStorage.removeItem('startDate');
            location.reload();
        }
    </script>
</body>
</html>