<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>QuadraCalc by Chinnarasu</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
        }
        .calculator-body {
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1), 0 5px 10px rgba(0, 0, 0, 0.04);
        }
        .nav-button.active {
            background-color: #4F46E5;
            color: white;
            box-shadow: 0 4px 14px 0 rgb(0 118 255 / 39%);
        }
        .btn-sci {
            background-color: #374151;
            color: white;
        }
        .btn-op {
             background-color: #f59e0b;
             color: white;
        }
        .slider-thumb::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            width: 20px;
            height: 20px;
            background: #4F46E5;
            cursor: pointer;
            border-radius: 50%;
        }
        .slider-thumb::-moz-range-thumb {
            width: 20px;
            height: 20px;
            background: #4F46E5;
            cursor: pointer;
            border-radius: 50%;
        }
    </style>
</head>
<body class="bg-gray-100 dark:bg-gray-900 text-gray-900 dark:text-gray-100 transition-colors duration-500">
    <div class="container mx-auto p-4 md:p-8">
        <!-- Header -->
        <header class="text-center mb-8">
            <h1 class="text-4xl md:text-5xl font-bold text-indigo-600 dark:text-indigo-400">QuadraCalc</h1>
            <p class="text-gray-600 dark:text-gray-400 mt-2">Expertly Coded by Chinnarasu</p>
        </header>

        <!-- Navigation -->
        <nav class="flex justify-center flex-wrap gap-2 mb-8">
            <button id="emi-nav" class="nav-button active px-4 py-2 rounded-lg font-semibold transition-all duration-300">EMI Calculator</button>
            <button id="bmi-nav" class="nav-button px-4 py-2 rounded-lg font-semibold transition-all duration-300">BMI Calculator</button>
            <button id="std-nav" class="nav-button px-4 py-2 rounded-lg font-semibold transition-all duration-300">Standard Calculator</button>
            <button id="sci-nav" class="nav-button px-4 py-2 rounded-lg font-semibold transition-all duration-300">Scientific Calculator</button>
        </nav>

        <main id="main-content">
            <!-- EMI Calculator -->
            <div id="emi-calculator" class="calculator-view p-6 bg-white dark:bg-gray-800 rounded-2xl calculator-body max-w-4xl mx-auto">
                <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                    <!-- Input Section -->
                    <div>
                        <h2 class="text-2xl font-bold mb-6 text-indigo-600 dark:text-indigo-400">Loan Details</h2>
                        <!-- Loan Amount -->
                        <div class="mb-6">
                            <label for="loanAmount" class="block mb-2 font-medium">Loan Amount</label>
                            <div class="flex items-center">
                                <span class="mr-2 font-bold text-indigo-500">₹</span>
                                <input type="text" id="loanAmountValue" value="1000000" class="w-full bg-transparent font-bold text-lg border-b-2 border-gray-300 focus:outline-none focus:border-indigo-500">
                            </div>
                            <input type="range" id="loanAmount" min="50000" max="10000000" step="10000" value="1000000" class="w-full h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer mt-2 slider-thumb">
                        </div>
                        <!-- Interest Rate -->
                        <div class="mb-6">
                            <label for="interestRate" class="block mb-2 font-medium">Interest Rate (%)</label>
                            <div class="flex items-center">
                                <input type="text" id="interestRateValue" value="8.5" class="w-full bg-transparent font-bold text-lg border-b-2 border-gray-300 focus:outline-none focus:border-indigo-500">
                                <span class="ml-2 font-bold text-indigo-500">%</span>
                            </div>
                            <input type="range" id="interestRate" min="1" max="20" step="0.1" value="8.5" class="w-full h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer mt-2 slider-thumb">
                        </div>
                        <!-- Loan Tenure -->
                        <div class="mb-6">
                            <label for="loanTenure" class="block mb-2 font-medium">Loan Tenure (Years)</label>
                            <div class="flex items-center">
                                <input type="text" id="loanTenureValue" value="20" class="w-full bg-transparent font-bold text-lg border-b-2 border-gray-300 focus:outline-none focus:border-indigo-500">
                                <span class="ml-2 font-bold text-indigo-500">Yrs</span>
                            </div>
                            <input type="range" id="loanTenure" min="1" max="30" step="1" value="20" class="w-full h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer mt-2 slider-thumb">
                        </div>
                    </div>
                    <!-- Result Section -->
                    <div class="flex flex-col items-center justify-center bg-gray-50 dark:bg-gray-700 p-6 rounded-xl">
                        <h2 class="text-xl font-bold mb-4">Your EMI Details</h2>
                        <div class="text-center mb-4">
                            <p class="text-gray-500 dark:text-gray-400">Monthly EMI</p>
                            <p id="emiResult" class="text-4xl font-bold text-indigo-600 dark:text-indigo-400">₹ 8,678</p>
                        </div>
                        <canvas id="emiChart" width="200" height="200"></canvas>
                        <div class="w-full mt-4 text-sm">
                           <div class="flex justify-between py-1">
                                <span class="flex items-center"><span class="w-3 h-3 rounded-full bg-indigo-500 mr-2"></span>Principal Amount</span>
                                <span id="totalPrincipal" class="font-semibold">₹ 10,00,000</span>
                            </div>
                            <div class="flex justify-between py-1">
                                <span class="flex items-center"><span class="w-3 h-3 rounded-full bg-amber-500 mr-2"></span>Total Interest</span>
                                <span id="totalInterest" class="font-semibold">₹ 10,82,781</span>
                            </div>
                            <hr class="my-2 border-gray-300 dark:border-gray-600">
                            <div class="flex justify-between py-1 font-bold">
                                <span>Total Payment</span>
                                <span id="totalPayment">₹ 20,82,781</span>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- BMI Calculator -->
            <div id="bmi-calculator" class="calculator-view hidden p-6 bg-white dark:bg-gray-800 rounded-2xl calculator-body max-w-2xl mx-auto">
                 <h2 class="text-2xl font-bold mb-6 text-center text-indigo-600 dark:text-indigo-400">BMI Calculator</h2>
                 <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <div>
                        <div class="mb-4">
                            <label for="height" class="block mb-2 font-medium">Height (cm)</label>
                            <input type="number" id="height" placeholder="e.g., 175" class="w-full p-3 rounded-lg bg-gray-100 dark:bg-gray-700 border border-gray-300 dark:border-gray-600 focus:outline-none focus:ring-2 focus:ring-indigo-500">
                        </div>
                        <div class="mb-4">
                            <label for="weight" class="block mb-2 font-medium">Weight (kg)</label>
                            <input type="number" id="weight" placeholder="e.g., 70" class="w-full p-3 rounded-lg bg-gray-100 dark:bg-gray-700 border border-gray-300 dark:border-gray-600 focus:outline-none focus:ring-2 focus:ring-indigo-500">
                        </div>
                        <button id="calculateBmi" class="w-full bg-indigo-600 text-white font-bold py-3 px-4 rounded-lg hover:bg-indigo-700 transition duration-300">Calculate BMI</button>
                    </div>
                     <div id="bmi-result-container" class="text-center p-4 bg-gray-50 dark:bg-gray-700 rounded-lg flex flex-col justify-center items-center">
                        <p class="text-gray-500 dark:text-gray-400">Your BMI</p>
                        <p id="bmiValue" class="text-5xl font-bold my-2">-</p>
                        <p id="bmiCategory" class="text-lg font-semibold">-</p>
                    </div>
                 </div>
                 <div id="bmi-description" class="mt-6 p-4 bg-indigo-50 dark:bg-gray-700 rounded-lg hidden">
                     <h3 class="font-bold text-lg mb-2 text-indigo-800 dark:text-indigo-300">Analysis & Suggestions</h3>
                     <p id="bmiSuggestion" class="text-gray-700 dark:text-gray-300"></p>
                 </div>
            </div>

            <!-- Standard Calculator -->
            <div id="std-calculator" class="calculator-view hidden p-4 bg-white dark:bg-gray-800 rounded-2xl calculator-body max-w-sm mx-auto">
                <div class="bg-gray-200 dark:bg-gray-900 rounded-lg p-4 mb-4 text-right">
                    <div id="std-display" class="text-4xl font-bold h-12 truncate">0</div>
                    <div id="std-sub-display" class="text-lg text-gray-500 h-6 truncate"></div>
                </div>
                <div class="grid grid-cols-4 gap-2">
                    <button class="std-btn bg-gray-300 dark:bg-gray-600 p-4 rounded-lg text-xl font-semibold" data-key="clear">AC</button>
                    <button class="std-btn bg-gray-300 dark:bg-gray-600 p-4 rounded-lg text-xl font-semibold" data-key="backspace">⌫</button>
                    <button class="std-btn btn-op p-4 rounded-lg text-xl font-semibold" data-key="%">%</button>
                    <button class="std-btn btn-op p-4 rounded-lg text-xl font-semibold" data-key="/">÷</button>
                    <button class="std-btn bg-gray-200 dark:bg-gray-700 p-4 rounded-lg text-xl font-semibold" data-key="7">7</button>
                    <button class="std-btn bg-gray-200 dark:bg-gray-700 p-4 rounded-lg text-xl font-semibold" data-key="8">8</button>
                    <button class="std-btn bg-gray-200 dark:bg-gray-700 p-4 rounded-lg text-xl font-semibold" data-key="9">9</button>
                    <button class="std-btn btn-op p-4 rounded-lg text-xl font-semibold" data-key="*">×</button>
                    <button class="std-btn bg-gray-200 dark:bg-gray-700 p-4 rounded-lg text-xl font-semibold" data-key="4">4</button>
                    <button class="std-btn bg-gray-200 dark:bg-gray-700 p-4 rounded-lg text-xl font-semibold" data-key="5">5</button>
                    <button class="std-btn bg-gray-200 dark:bg-gray-700 p-4 rounded-lg text-xl font-semibold" data-key="6">6</button>
                    <button class="std-btn btn-op p-4 rounded-lg text-xl font-semibold" data-key="-">-</button>
                    <button class="std-btn bg-gray-200 dark:bg-gray-700 p-4 rounded-lg text-xl font-semibold" data-key="1">1</button>
                    <button class="std-btn bg-gray-200 dark:bg-gray-700 p-4 rounded-lg text-xl font-semibold" data-key="2">2</button>
                    <button class="std-btn bg-gray-200 dark:bg-gray-700 p-4 rounded-lg text-xl font-semibold" data-key="3">3</button>
                    <button class="std-btn btn-op p-4 rounded-lg text-xl font-semibold" data-key="+">+</button>
                    <button class="std-btn bg-gray-200 dark:bg-gray-700 p-4 rounded-lg text-xl font-semibold col-span-2" data-key="0">0</button>
                    <button class="std-btn bg-gray-200 dark:bg-gray-700 p-4 rounded-lg text-xl font-semibold" data-key=".">.</button>
                    <button class="std-btn bg-indigo-600 text-white p-4 rounded-lg text-xl font-semibold" data-key="=">=</button>
                </div>
            </div>

            <!-- Scientific Calculator -->
            <div id="sci-calculator" class="calculator-view hidden p-4 bg-white dark:bg-gray-800 rounded-2xl calculator-body max-w-md mx-auto">
                <div class="bg-gray-200 dark:bg-gray-900 rounded-lg p-4 mb-4 text-right">
                    <div id="sci-display" class="text-4xl font-bold h-12 truncate">0</div>
                    <div id="sci-sub-display" class="text-lg text-gray-500 h-6 truncate"></div>
                </div>
                <div class="grid grid-cols-5 gap-2">
                     <button class="sci-btn btn-sci p-3 rounded-lg text-md font-semibold" data-key="sin">sin</button>
                     <button class="sci-btn btn-sci p-3 rounded-lg text-md font-semibold" data-key="cos">cos</button>
                     <button class="sci-btn btn-sci p-3 rounded-lg text-md font-semibold" data-key="tan">tan</button>
                     <button class="sci-btn btn-sci p-3 rounded-lg text-md font-semibold" data-key="log">log</button>
                     <button class="sci-btn btn-sci p-3 rounded-lg text-md font-semibold" data-key="ln">ln</button>
                     <button class="sci-btn btn-sci p-3 rounded-lg text-md font-semibold" data-key="^">x<sup>y</sup></button>
                     <button class="sci-btn btn-sci p-3 rounded-lg text-md font-semibold" data-key="sqrt">√</button>
                     <button class="sci-btn btn-sci p-3 rounded-lg text-md font-semibold" data-key="(">(</button>
                     <button class="sci-btn btn-sci p-3 rounded-lg text-md font-semibold" data-key=")">)</button>
                     <button class="sci-btn btn-sci p-3 rounded-lg text-md font-semibold" data-key="pi">π</button>
                     <button class="sci-btn bg-gray-300 dark:bg-gray-600 p-3 rounded-lg text-xl font-semibold" data-key="clear">AC</button>
                     <button class="sci-btn bg-gray-300 dark:bg-gray-600 p-3 rounded-lg text-xl font-semibold" data-key="backspace">⌫</button>
                     <button class="sci-btn btn-op p-3 rounded-lg text-xl font-semibold" data-key="%">%</button>
                     <button class="sci-btn btn-op p-3 rounded-lg text-xl font-semibold" data-key="/">÷</button>
                     <button class="sci-btn btn-op p-3 rounded-lg text-xl font-semibold" data-key="*">×</button>
                     <button class="sci-btn bg-gray-200 dark:bg-gray-700 p-3 rounded-lg text-xl font-semibold" data-key="7">7</button>
                     <button class="sci-btn bg-gray-200 dark:bg-gray-700 p-3 rounded-lg text-xl font-semibold" data-key="8">8</button>
                     <button class="sci-btn bg-gray-200 dark:bg-gray-700 p-3 rounded-lg text-xl font-semibold" data-key="9">9</button>
                     <button class="sci-btn bg-gray-200 dark:bg-gray-700 p-3 rounded-lg text-xl font-semibold" data-key="e">e</button>
                     <button class="sci-btn btn-op p-3 rounded-lg text-xl font-semibold" data-key="-">-</button>
                     <button class="sci-btn bg-gray-200 dark:bg-gray-700 p-3 rounded-lg text-xl font-semibold" data-key="4">4</button>
                     <button class="sci-btn bg-gray-200 dark:bg-gray-700 p-3 rounded-lg text-xl font-semibold" data-key="5">5</button>
                     <button class="sci-btn bg-gray-200 dark:bg-gray-700 p-3 rounded-lg text-xl font-semibold" data-key="6">6</button>
                     <button class="sci-btn bg-gray-200 dark:bg-gray-700 p-3 rounded-lg text-xl font-semibold" data-key="10^">10<sup>x</sup></button>
                     <button class="sci-btn btn-op p-3 rounded-lg text-xl font-semibold" data-key="+">+</button>
                     <button class="sci-btn bg-gray-200 dark:bg-gray-700 p-3 rounded-lg text-xl font-semibold" data-key="1">1</button>
                     <button class="sci-btn bg-gray-200 dark:bg-gray-700 p-3 rounded-lg text-xl font-semibold" data-key="2">2</button>
                     <button class="sci-btn bg-gray-200 dark:bg-gray-700 p-3 rounded-lg text-xl font-semibold" data-key="3">3</button>
                     <button class="sci-btn bg-gray-200 dark:bg-gray-700 p-3 rounded-lg text-xl font-semibold" data-key="fact">n!</button>
                     <button class="sci-btn bg-indigo-600 text-white p-3 rounded-lg text-xl font-semibold" data-key="=">=</button>
                     <button class="sci-btn bg-gray-200 dark:bg-gray-700 p-3 rounded-lg text-xl font-semibold col-span-2" data-key="0">0</button>
                     <button class="sci-btn bg-gray-200 dark:bg-gray-700 p-3 rounded-lg text-xl font-semibold" data-key=".">.</button>
                     <button class="sci-btn bg-gray-200 dark:bg-gray-700 p-3 rounded-lg text-xl font-semibold" data-key="abs">|x|</button>
                </div>
            </div>
        </main>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function () {
            // --- General Navigation ---
            const navButtons = document.querySelectorAll('.nav-button');
            const calculatorViews = document.querySelectorAll('.calculator-view');

            navButtons.forEach(button => {
                button.addEventListener('click', () => {
                    // Update button styles
                    navButtons.forEach(btn => btn.classList.remove('active'));
                    button.classList.add('active');

                    // Show correct calculator view
                    const targetId = button.id.replace('-nav', '-calculator');
                    calculatorViews.forEach(view => {
                        if (view.id === targetId) {
                            view.classList.remove('hidden');
                        } else {
                            view.classList.add('hidden');
                        }
                    });
                });
            });

            // --- EMI Calculator ---
            const loanAmountSlider = document.getElementById('loanAmount');
            const interestRateSlider = document.getElementById('interestRate');
            const loanTenureSlider = document.getElementById('loanTenure');
            const loanAmountValue = document.getElementById('loanAmountValue');
            const interestRateValue = document.getElementById('interestRateValue');
            const loanTenureValue = document.getElementById('loanTenureValue');
            
            const emiResultEl = document.getElementById('emiResult');
            const totalPrincipalEl = document.getElementById('totalPrincipal');
            const totalInterestEl = document.getElementById('totalInterest');
            const totalPaymentEl = document.getElementById('totalPayment');

            let emiChart;

            function formatNumber(num) {
                 return new Intl.NumberFormat('en-IN').format(Math.round(num));
            }

            function updateEmiInputs(source, target) {
                 source.addEventListener('input', () => {
                    target.value = source.value;
                    calculateEmi();
                 });
                 target.addEventListener('input', () => {
                     source.value = target.value;
                     calculateEmi();
                 })
            }

            updateEmiInputs(loanAmountSlider, loanAmountValue);
            updateEmiInputs(interestRateSlider, interestRateValue);
            updateEmiInputs(loanTenureSlider, loanTenureValue);

            function calculateEmi() {
                const p = parseFloat(loanAmountValue.value.replace(/,/g, ''));
                const r = parseFloat(interestRateValue.value) / 12 / 100;
                const n = parseFloat(loanTenureValue.value) * 12;

                if (p && r && n) {
                    const emi = (p * r * Math.pow(1 + r, n)) / (Math.pow(1 + r, n) - 1);
                    const totalPayment = emi * n;
                    const totalInterest = totalPayment - p;

                    emiResultEl.textContent = `₹ ${formatNumber(emi)}`;
                    totalPrincipalEl.textContent = `₹ ${formatNumber(p)}`;
                    totalInterestEl.textContent = `₹ ${formatNumber(totalInterest)}`;
                    totalPaymentEl.textContent = `₹ ${formatNumber(totalPayment)}`;
                    
                    updateEmiChart(p, totalInterest);
                }
            }

             function updateEmiChart(principal, interest) {
                const ctx = document.getElementById('emiChart').getContext('2d');
                if (emiChart) {
                    emiChart.destroy();
                }
                emiChart = new Chart(ctx, {
                    type: 'doughnut',
                    data: {
                        labels: ['Principal Amount', 'Total Interest'],
                        datasets: [{
                            data: [principal, interest],
                            backgroundColor: ['#4F46E5', '#F59E0B'],
                            borderWidth: 0,
                        }]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        plugins: {
                            legend: {
                                display: false
                            }
                        },
                        cutout: '70%'
                    }
                });
            }

            calculateEmi();
            
            // --- BMI Calculator ---
            const heightInput = document.getElementById('height');
            const weightInput = document.getElementById('weight');
            const calculateBmiBtn = document.getElementById('calculateBmi');
            const bmiValueEl = document.getElementById('bmiValue');
            const bmiCategoryEl = document.getElementById('bmiCategory');
            const bmiDescriptionContainer = document.getElementById('bmi-description');
            const bmiSuggestionEl = document.getElementById('bmiSuggestion');
            const bmiResultContainer = document.getElementById('bmi-result-container');

            calculateBmiBtn.addEventListener('click', () => {
                const height = parseFloat(heightInput.value);
                const weight = parseFloat(weightInput.value);

                if (height > 0 && weight > 0) {
                    const heightInMeters = height / 100;
                    const bmi = weight / (heightInMeters * heightInMeters);
                    const bmiRounded = bmi.toFixed(1);

                    bmiValueEl.textContent = bmiRounded;

                    let category = '';
                    let suggestion = '';
                    let colorClass = '';

                    if (bmi < 18.5) {
                        category = 'Underweight';
                        colorClass = 'text-blue-500';
                        suggestion = "You are in the underweight category. It's important to focus on a balanced, nutrient-dense diet to reach a healthy weight. Consider consulting a doctor or nutritionist. Scientific approach: Focus on increasing caloric intake with healthy fats, proteins, and complex carbohydrates. Strength training can also help build lean muscle mass.";
                    } else if (bmi >= 18.5 && bmi <= 24.9) {
                        category = 'Normal weight';
                        colorClass = 'text-green-500';
                        suggestion = "Congratulations! Your BMI is in the healthy range. Maintain this by continuing a balanced diet and regular physical activity. Scientific approach: A mix of cardiovascular exercise (150 mins/week) and strength training (2 days/week) is recommended for long-term health maintenance.";
                    } else if (bmi >= 25 && bmi <= 29.9) {
                        category = 'Overweight';
                        colorClass = 'text-yellow-500';
                        suggestion = "You are in the overweight category. This increases the risk of certain health problems. A focus on diet and exercise is recommended. Scientific approach: Create a sustainable caloric deficit (around 500 calories/day) for gradual weight loss. Incorporate both aerobic exercise and resistance training to preserve muscle mass.";
                    } else {
                        category = 'Obese';
                        colorClass = 'text-red-500';
                        suggestion = "You are in the obese category, which can pose significant health risks. It's highly recommended to consult a healthcare professional. Scientific approach: A structured plan combining dietary modification (e.g., Mediterranean or DASH diet), increased physical activity, and behavioral therapy is most effective. Medical interventions might be considered under a doctor's guidance.";
                    }

                    bmiCategoryEl.textContent = category;
                    bmiValueEl.className = `text-5xl font-bold my-2 ${colorClass}`;
                    bmiCategoryEl.className = `text-lg font-semibold ${colorClass}`;
                    bmiSuggestionEl.textContent = suggestion;
                    bmiDescriptionContainer.classList.remove('hidden');

                } else {
                    bmiValueEl.textContent = '-';
                    bmiCategoryEl.textContent = 'Enter values';
                    bmiValueEl.className = `text-5xl font-bold my-2`;
                    bmiCategoryEl.className = `text-lg font-semibold`;
                    bmiDescriptionContainer.classList.add('hidden');
                }
            });

            // --- Standard & Scientific Calculator Logic ---
            function initializeCalculator(type) {
                const display = document.getElementById(`${type}-display`);
                const subDisplay = document.getElementById(`${type}-sub-display`);
                const buttons = document.querySelectorAll(`#${type}-calculator .${type}-btn`);
                
                let currentInput = '0';
                let expression = '';
                let resultDisplayed = false;

                function updateDisplay() {
                    display.textContent = currentInput;
                    subDisplay.textContent = expression.replace(/\*/g, '×').replace(/\//g, '÷');
                }

                function factorial(n) {
                    if (n < 0) return NaN;
                    if (n === 0) return 1;
                    let result = 1;
                    for (let i = 2; i <= n; i++) {
                        result *= i;
                    }
                    return result;
                }
                
                function handleInput(key) {
                    if (/\d/.test(key)) { // Number
                        if (resultDisplayed || currentInput === '0') {
                            currentInput = key;
                            resultDisplayed = false;
                        } else {
                            currentInput += key;
                        }
                    } else if (key === '.') { // Decimal
                        if (resultDisplayed) {
                            currentInput = '0.';
                            resultDisplayed = false;
                        } else if (!currentInput.includes('.')) {
                            currentInput += '.';
                        }
                    } else if (['+', '-', '*', '/'].includes(key)) { // Operator
                         if (resultDisplayed) {
                            expression = currentInput + ` ${key} `;
                            resultDisplayed = false;
                         } else {
                            expression += `${currentInput} ${key} `;
                         }
                         currentInput = '0';
                    } else if (key === '=') { // Equals
                        if (expression) {
                            expression += currentInput;
                            try {
                                let evalExpression = expression.replace(/×/g, '*').replace(/÷/g, '/').replace(/\^/g, '**');
                                const result = eval(evalExpression);
                                currentInput = String(result);
                                expression = '';
                                resultDisplayed = true;
                            } catch (e) {
                                currentInput = 'Error';
                                expression = '';
                                resultDisplayed = true;
                            }
                        }
                    } else if (key === 'clear') { // AC
                        currentInput = '0';
                        expression = '';
                        resultDisplayed = false;
                    } else if (key === 'backspace') { // Backspace
                        if (currentInput.length > 1) {
                            currentInput = currentInput.slice(0, -1);
                        } else {
                            currentInput = '0';
                        }
                    } else if (key === '%') {
                        currentInput = String(parseFloat(currentInput) / 100);
                        resultDisplayed = true;
                    } 
                    // Scientific functions
                    else if (type === 'sci') {
                         handleScientificInput(key);
                    }
                    
                    updateDisplay();
                }

                function handleScientificInput(key) {
                     const num = parseFloat(currentInput);
                     let result;
                     switch(key) {
                        case 'sin': result = Math.sin(num * Math.PI / 180); break;
                        case 'cos': result = Math.cos(num * Math.PI / 180); break;
                        case 'tan': result = Math.tan(num * Math.PI / 180); break;
                        case 'log': result = Math.log10(num); break;
                        case 'ln': result = Math.log(num); break;
                        case 'sqrt': result = Math.sqrt(num); break;
                        case '^': expression += currentInput + ' ^ '; currentInput='0'; break;
                        case 'pi': currentInput = String(Math.PI); break;
                        case 'e': currentInput = String(Math.E); break;
                        case '10^': result = Math.pow(10, num); break;
                        case 'fact': result = factorial(num); break;
                        case 'abs': result = Math.abs(num); break;
                     }
                     if (result !== undefined) {
                        currentInput = String(result);
                        resultDisplayed = true;
                     }
                }

                buttons.forEach(button => {
                    button.addEventListener('click', () => handleInput(button.dataset.key));
                });

                updateDisplay();
            }

            initializeCalculator('std');
            initializeCalculator('sci');

        });
    </script>
</body>
</html>
