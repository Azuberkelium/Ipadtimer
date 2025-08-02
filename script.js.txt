document.addEventListener('DOMContentLoaded', () => {
    // --- State variables to keep track of counts and timers ---
    let greenCircleCount = 0;
    let warningCount = 0;
    let ipadHours = 0;
    let timerInterval = null; // To store the timer for yellow/red cards
    let timerEndTime = null; // To store the time when the countdown should end

    // --- Select all the HTML elements we need to interact with ---
    const greenCircleBtn = document.getElementById('greenCircleBtn');
    const greenCircleCountEl = document.getElementById('greenCircleCount');
    const greenSquareBtn = document.getElementById('greenSquareBtn');
    const ipadTimerDisplay = document.getElementById('ipadTimerDisplay');
    const resetBtn = document.getElementById('resetBtn');
    const newDayBtn = document.getElementById('newDayBtn');
    const warningBtn = document.getElementById('warningBtn');
    const warningCountEl = document.getElementById('warningCount');
    const yellowCardBtn = document.getElementById('yellowCardBtn');
    const redCardBtn = document.getElementById('redCardBtn');

    // --- Functions to update the displays ---
    function updateIpadTimerDisplay() {
        // Format the timer display to HH:MM:SS
        const hours = String(Math.floor(ipadHours)).padStart(2, '0');
        ipadTimerDisplay.textContent = `${hours}:00:00`;
    }

    function updateCardTimerDisplay() {
        // Clear the main iPad timer display and show the countdown
        if (timerEndTime) {
            const now = new Date().getTime();
            const distance = timerEndTime - now;

            const hours = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
            const minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
            const seconds = Math.floor((distance % (1000 * 60)) / 1000);

            ipadTimerDisplay.textContent = `${String(hours).padStart(2, '0')}:${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}`;

            if (distance < 0) {
                clearInterval(timerInterval);
                ipadTimerDisplay.textContent = "Timer Expired!";
            }
        }
    }

    function startCountdownTimer(hours) {
        clearInterval(timerInterval); // Clear any existing timer
        timerEndTime = new Date().getTime() + (hours * 60 * 60 * 1000);
        timerInterval = setInterval(updateCardTimerDisplay, 1000);
    }
    
    // --- Event Listeners for all the buttons ---
    
    // Left Side Buttons
    greenCircleBtn.addEventListener('click', () => {
        greenCircleCount++;
        greenCircleCountEl.textContent = greenCircleCount;

        if (greenCircleCount >= 5) {
            greenCircleCount = 0;
            greenCircleCountEl.textContent = greenCircleCount;
            ipadHours++;
            updateIpadTimerDisplay();
        }
    });

    greenSquareBtn.addEventListener('click', () => {
        // The prompt did not specify what this button does, so we'll leave it as a placeholder.
        // You can add your own logic here, e.g.,
        // ipadHours += 2;
        // updateIpadTimerDisplay();
        console.log("Green square button clicked!");
    });
    
    // Middle Buttons
    resetBtn.addEventListener('click', () => {
        // Reset all counts and timers
        ipadHours = 0;
        greenCircleCount = 0;
        warningCount = 0;
        clearInterval(timerInterval);
        timerInterval = null;
        timerEndTime = null;

        greenCircleCountEl.textContent = greenCircleCount;
        warningCountEl.textContent = warningCount;
        updateIpadTimerDisplay();
    });

    newDayBtn.addEventListener('click', () => {
        ipadHours += 3;
        updateIpadTimerDisplay();
    });
    
    // Right Side Buttons
    warningBtn.addEventListener('click', () => {
        warningCount++;
        warningCountEl.textContent = warningCount;

        if (warningCount >= 5) {
            warningCount = 0;
            warningCountEl.textContent = warningCount;
            
            // Reset the iPad timer
            ipadHours = 0;
            clearInterval(timerInterval);
            timerInterval = null;
            timerEndTime = null;
            updateIpadTimerDisplay();
        }
    });

    yellowCardBtn.addEventListener('click', () => {
        // Reset iPad timer to 0 and start 12-hour countdown
        ipadHours = 0;
        updateIpadTimerDisplay(); // Clears the hours display
        startCountdownTimer(12);
    });

    redCardBtn.addEventListener('click', () => {
        // Reset iPad timer to 0 and start 24-hour countdown
        ipadHours = 0;
        updateIpadTimerDisplay(); // Clears the hours display
        startCountdownTimer(24);
    });
});
