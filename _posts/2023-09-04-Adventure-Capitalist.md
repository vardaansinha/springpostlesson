---
comments: True
layout: post
title: Adventure Capitalist
description: A classical ripoff of Adventure Capitalist
type: tangibles
courses: {'csse': {'week': 1}, 'csp': {'week': 1}, csa: {'week': 3}}
categories: ['C4.1']
---
<style>
 @import url('https://fonts.googleapis.com/css?family=Jost');
      .click-button {
        background-color: #2e6930;
        font-family: 'Jost';
        font-size: 36px;
        width: 200px;
        height: 80px;
        color: #fefefe;
        border: 3px solid #ffffff;
        border-radius: 8px;
        transition: background-color 0.3s ease, color 0.3s ease;
        position: relative
      }
      .business {
        background-color: #265828;
        font-family: 'Jost';
        font-size: 22px;
        width: 340px;
        color: #fefefe;
        border: 3px solid #ffffff;
        border-radius: 8px;
        transition: background-color 0.3s ease, color 0.3s ease;
      }
    .search {
        background-color: #265828;
        font-family: 'Jost';
        font-size: 11px;
        width: 100px;
        color: #fefefe;
        border: 3px solid #ffffff;
        border-radius: 8px;
        transition: background-color 0.3s ease, color 0.3s ease;
      }
    .purchase-button {
        background-color: #265828;
        font-family: 'Jost';
        font-size: 10px;
        width: 100px;
        color: #fefefe;
        border: 3px solid #ffffff;
        border-radius: 8px;
        transition: background-color 0.2s ease, color 0.2s ease;
      }
    .businesses {
          display: flex;
          flex-direction: column;
          align-items: center;
          justify-content: center;
          height: 20vh; /* This makes the container take the full viewport height */
          position: relative
      }
      .money-container{
            display: flex;
          flex-direction: column;
          align-items: center;
          justify-content: center;
          height: 50vh; /* This makes the container take the full viewport height */
          position: relative
      }
</style>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Adventure Capitalist Clicker Game</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <div class="money-container">
            <h1>Adventure Capitalist Clicker</h1>
            <h2>Make as much money as possible within 3 minutes!</h2>
            <h3>Time Left: <span id="timer">0</span> seconds</h3>
            <h3>Money: $<span id="money">0</span></h3>
            <h3 id="money-per-second">Money per second: $0.00</h3>
            <button id="click-button" class = "click-button">~ Click ~</button>
        </div>
        <div class="businesses">
            <button id="business1" class="business">Lemonade Stand ($25) ~ 0</button>
            <button id="business2" class="business">Tech Repair ($100) ~ 0</button>
            <button id="business3" class="business">Gas Station ($250) ~ 0</button>
            <button id="business4" class="business">Wholesale Store ($1000) ~ 0</button>
        </div>
    <div id="player-businesses">
        <h2>Your Businesses:</h2>
        <ul id="player-business-list"></ul>
    </div>
            <!-- Business search input -->
    <h3><label for="search-input">Search for businesses:</label></h3>
    <input type="text" id="search-input">
    <button id="search-button" class = "search">Search</button>
    <!-- Display search results -->
    <div id="results">
        <h2>Search Results:</h2>
        <table id="business-table">
            <thead>
                <tr>
                    <th>Company Name</th>
                    <th>Cost</th>
                    <th>Action</th>
                </tr>
            </thead>
        <tbody id="business-list"></tbody>
    </table>
    </div>
    <!-- Display the player's businesses -->
    </div>
    <script src="script.js"></script>
</body>
</html>
<script>
// Helper function to set a cookie with a given name and value
let congrats = "Make as much money as you can";
let money = 0;
let playerBusinesses = [];
let business1Count = 0;
let business2Count = 0;
let business3Count = 0;
let business4Count = 0;
let businessesRevenue = 0;
let startTime = null;
let endTime = null;
let isGamePaused = false;
let highestScore = parseInt(getCookie("highestScore")) || 0;
const moneyDisplay = document.getElementById("money");
const clickButton = document.getElementById("click-button");
const business1Button = document.getElementById("business1");
const business2Button = document.getElementById("business2");
const business3Button = document.getElementById("business3");
const business4Button = document.getElementById("business4");
const timerDisplay = document.getElementById("timer");
const scoreDisplay = document.getElementById("score");
const businessDisplay = document.getElementById("player-business-list");
const searchInput = document.getElementById("search-input");
    const searchButton = document.getElementById("search-button");
    const businessList = document.getElementById("business-list");
    const businesses = [
        { name: "ABC Corporation", networth: 5000 },
        { name: "XYZ Industries", networth: 3000 },
        { name: "Alpha Inc.", networth: 1500 },
        { name: "Beta Corp.", networth: 8000 },
        { name: "Gamma Corp.", networth: 7000 },
        { name: "Delta Enterprises", networth: 6000 },
        { name: "Omega Ltd.", networth: 2000 },
        { name: "Sigma Co.", networth: 4000 },
        { name: "Zeta Holdings", networth: 9000 },
        { name: "Epsilon Ventures", networth: 2500 },
        { name: "Microsoft", networth: 10000},
        { name: "Apple", networth: 150000},
        { name: "Google", networth: 900000},
        { name: "Bob", networth: 99999999},
    ];
searchButton.addEventListener("click", function () {
    if (!isGamePaused){
            const searchTerm = searchInput.value.toLowerCase();
    // Clear previous results
    businessList.innerHTML = "";
    // Filter and sort businesses based on the search term
    const filteredBusinesses = businesses.filter(business => business.name.toLowerCase().includes(searchTerm));
    const sortedBusinesses = filteredBusinesses.sort((a, b) => b.networth - a.networth);
    // Display the top 10 businesses
    const top10Businesses = sortedBusinesses.slice(0, 10);
    top10Businesses.forEach(business => {
        const row = document.createElement("tr");
        row.innerHTML = `
            <td>${business.name}</td>
            <td>$${business.networth}</td>
            <td><button class="purchase-button" data-business-name="${business.name}" data-cost="${business.networth}">Purchase</button></td>
        `;
        businessList.appendChild(row);
    });
    // Add event listeners to the "Purchase" buttons
    const purchaseButtons = document.querySelectorAll(".purchase-button");
    purchaseButtons.forEach(button => {
        button.addEventListener("click", function () {
            if(!isGamePaused){
                const businessName = this.getAttribute("data-business-name");
                const cost = parseInt(this.getAttribute("data-cost"));
                purchaseBusiness(businessName, cost);
            }
        });
    });
    }
});
function updatePlayerBusinessesList() {
    const playerBusinessList = document.getElementById("player-business-list");
    playerBusinessList.innerHTML = "";
    playerBusinesses.forEach((business, index) => {
        const listItem = document.createElement("li");
        listItem.textContent = `Business ${index + 1}: ${business.name} (Revenue: $${business.revenue.toFixed(2)} per second)`;
        playerBusinessList.appendChild(listItem);
    });
}
    // Function to purchase a business
// Call the updatePlayerBusinessesList function whenever a business is purchased
function purchaseBusiness(businessName, cost) {
    if (money >= cost && !isGamePaused) {
        money -= cost;
        const revenue = cost / 10; // Each business generates 1/10 of its cost per second
        playerBusinesses.push({ name: businessName, revenue });
        updateMoneyDisplay();
        updatePlayerBusinessesList(); // Update the list when a business is purchased
        const make = cost/20;
        businessesRevenue += make;
    } else {
        alert("Not enough money to buy this business.");
    }
}
function setCookie(cname, cvalue, exdays) {
  const d = new Date();
  d.setTime(d.getTime() + (exdays * 24 * 60 * 60 * 1000));
  let expires = "expires="+d.toUTCString();
  document.cookie = cname + "=" + cvalue + ";" + expires + ";path=/";
}
function getCookie(cname) {
  let name = cname + "=";
  let ca = document.cookie.split(';');
  for(let i = 0; i < ca.length; i++) {
    let c = ca[i];
    while (c.charAt(0) == ' ') {
      c = c.substring(1);
    }
    if (c.indexOf(name) == 0) {
      return c.substring(name.length, c.length);
    }
  }
  return "";
}
function checkCookie() {
  let user = getCookie("username");
  if (user != "") {
    alert("Welcome back to Adventure Capitalist " + user);
  } else {
    user = prompt("Please enter your name:", "");
    if (user != "" && user != null) {
      setCookie("username", user, 365);
    }
  }
}
checkCookie();
clickButton.addEventListener("click", () => {
    if (!isGamePaused) {
        money += 1;
        updateMoneyDisplay();
        startTimer();
    }
});
business1Button.addEventListener("click", () => {
    if (money >= 25 && !isGamePaused) {
        money -= 25;
        business1Count += 1;
        updateMoneyDisplay();
    }
});
business2Button.addEventListener("click", () => {
    if (money >= 100 && !isGamePaused) {
        money -= 100;
        business2Count += 1;
        updateMoneyDisplay();
    }
});
business3Button.addEventListener("click", () => {
    if (money >= 250 && !isGamePaused) {
        money -= 250;
        business3Count += 1;
        updateMoneyDisplay();
    }
});
business4Button.addEventListener("click", () => {
    if (money >= 1000 && !isGamePaused) {
        money -= 1000;
        business4Count += 1;
        updateMoneyDisplay();
    }
});
function updateMoneyDisplay() {
    moneyDisplay.textContent = money;
    business1Button.textContent = `🍋 Lemonade Stand ($25) ~ ${business1Count}`;
    business2Button.textContent = `👨‍💻 Tech Repair ($100) ~ ${business2Count}`;
    business3Button.textContent = `⛽ Gas Station ($250) ~ ${business3Count}`;
    business4Button.textContent = `🏬 Wholesale Store ($1000) ~ ${business4Count}`;
}
function startTimer() {
    if (!startTime) {
        startTime = Date.now();
        endTime = startTime + 180000; // 3 minutes
        setInterval(updateTimer, 1000);
    }
}
function updateTimer() {
    if (startTime && !isGamePaused) {
        const currentTime = Date.now();
        const remainingTimeInSeconds = Math.max(0, Math.floor((endTime - currentTime) / 1000));
        timerDisplay.textContent = remainingTimeInSeconds;
        if (remainingTimeInSeconds <= 0) {
            gameOver();
        }
    }
}
function gameOver() {
    isGamePaused = true;
    const elapsedTimeInSeconds = Math.floor((endTime - startTime) / 1000);
    timerDisplay.textContent = "Time is up man! You have zero ";
    setCookie("playerScore", money, 365);
    if (money < highestScore) {
        alert("You didn't make it... Your balance was $" + money + "... but your high score is $" + highestScore);
    }
    if (money > highestScore) {
        highestScore = money;
        setCookie("highestScore", highestScore, 365);
        alert("Congratulations! Your bank was shocked as they looked at your account and saw $" + highestScore );
    }
}
// Add an interval for passive income from businesses
setInterval(() => {
    if (!isGamePaused) {
        money += business1Count * .5 + business2Count * 2.5 + business3Count * 7.5 + business4Count * 37.5 + businessesRevenue;
        updateMoneyDisplay();
    }
}, 250);
resetButton.addEventListener("click", () => {
    money = 0;
    startTime = null;
    endTime = null;
    let business1Count = 0;
    let business2Count = 0;
    let business3Count = 0;
    let business4Count = 0;
    isGamePaused = false;
    updateMoneyDisplay();
    timerDisplay.textContent = "180";
});
const moneyPerSecondDisplay = document.getElementById("money-per-second");
setInterval(updateMoneyPerSecondDisplay, 250);
function updateMoneyPerSecondDisplay() {
    if (!isGamePaused) {
        const currentTime = Date.now();
        const elapsedTimeInSeconds = (currentTime - startTime) / 1000;
        const moneyPerSecond = (money / elapsedTimeInSeconds).toFixed(2);
        moneyPerSecondDisplay.textContent = `Money per second: $${moneyPerSecond}`;
    }
};
</script>
<html>
<!--
Draft and ideas
Idea - A cookie clicker game to keep the user entertained
-
First Draft - A game where the objective is to get as much money as you can. You are able to buy businesses to grow your economy. We included cookies and math functions to remember the users's name as well as count up the money.
Addon 1 - We added styling and a bigger business to make the game more fast paced and fluid. we also made the game more cometitive by adding a time limit, requiring players to react fast. We also added a cookie to save the player's score. Attempted to add reset button.
Addon 2 - We added a database as per requirements which allows you to search up a company and purchase it, bringing in even more cash. We also implemented a money-per-second counter to allow the user to keep track of how much they earn. Used the player's score to add a message determining whether you beat your previous score or lost.
-
-
ChatGPT College Board Review and Comments
HTML and CSS Styling:
1. Your use of HTML and CSS for structuring and styling the web page is well done. Properly structured HTML and CSS are important foundational skills in web development.
-
JavaScript Functions:
2. You've defined various JavaScript functions to handle different aspects of the game logic. This demonstrates your understanding of organizing code into functions, a fundamental concept in programming.
-
Event Listeners:
3. You've utilized event listeners to respond to user interactions like button clicks. Event handling is a key concept, and you've implemented it effectively.
-
Cookies:
4. You've used cookies to store and retrieve information (e.g., the username and highest score). This shows knowledge of handling browser cookies, which can be useful in web applications.
-
DOM Manipulation:
5. You manipulate the Document Object Model (DOM) to update and display information dynamically. This is a fundamental skill for web development and is necessary for creating interactive web pages.
-
Game Logic:
6. Your game includes features like clicking to earn money, purchasing businesses, and calculating income per second. These elements demonstrate the application of basic game logic.
-
Conditional Statements:
7. You use conditional statements (e.g., if statements) to check conditions and make decisions. This is a core programming concept and is used effectively in your code.
-
Loops:
8. You've used loops, such as setInterval, to create periodic updates in the game (e.g., income from businesses). Loops are important for managing game dynamics.
-
Data Structures:
9. Your use of arrays (playerBusinesses) to store and manage player-owned businesses showcases knowledge of data structures, an essential concept in programming.
-
Time Handling:
10. You manage time within the game by tracking the start and end times. Handling time is a common requirement in game development and other applications.
-
User Interface (UI):
11. Your UI design, including buttons and labels, is clear and user-friendly. Good UI design is crucial for user engagement.
-
Comments:
12. You've added comments at various points in your code to explain its functionality. This is a good practice for code readability and documentation.
-
Error Handling:
13. You handle cases where the user may not have enough money to purchase a business or where the game timer runs out. Error handling is an important aspect of software development.
-
Scalability:
14. Your code allows for scalability by adding more businesses to the game dynamically. This demonstrates an understanding of code design for extensibility.
-
Overall Structure:
15. Your code is well-organized, with clear separation of concerns between HTML, CSS, and JavaScript. This is good practice for maintainable code.
-
Optimizations:
16. You've implemented optimizations, such as calculating money per second and handling passive income from businesses. This adds depth to the gameplay.
-->
</html>