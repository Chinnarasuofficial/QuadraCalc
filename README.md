# Quadra Calc by Chinnarasu

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>QuadraCalc by Chinnarasu</title>
  <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet"/>
</head>
<body class="bg-gradient-to-br from-blue-100 to-indigo-200 min-h-screen text-gray-800 p-4">
  <h1 class="text-4xl font-bold text-center mb-6">QuadraCalc by Chinnarasu</h1>
  <div class="max-w-4xl mx-auto grid gap-8">

    <!-- EMI Calculator -->
    <section class="bg-white shadow-md rounded-2xl p-6">
      <h2 class="text-2xl font-semibold mb-4">Loan EMI Calculator</h2>
      <label>Loan Amount (₹): <span id="loanAmountLabel">500000</span></label>
      <input type="range" min="100000" max="1000000" step="10000" value="500000" id="loanAmount" class="w-full">
      <input type="number" id="loanInput" class="border w-full p-2 rounded mt-2" value="500000">

      <label class="mt-4 block">Interest Rate (%): <span id="rateLabel">8</span></label>
      <input type="range" min="1" max="20" step="0.1" value="8" id="rate" class="w-full">

      <label class="mt-4 block">Loan Tenure (years): <span id="tenureLabel">5</span></label>
      <input type="range" min="1" max="30" step="1" value="5" id="tenure" class="w-full">

      <div class="mt-4 text-lg font-semibold">Monthly EMI: ₹<span id="emi">0</span></div>
      <div class="mt-2">Principal Amount: ₹<span id="principal">0</span></div>
      <div>Total Interest: ₹<span id="interest">0</span></div>
      <div>Total Amount: ₹<span id="total">0</span></div>
    </section>

    <!-- BMI Calculator -->
    <section class="bg-white shadow-md rounded-2xl p-6">
      <h2 class="text-2xl font-semibold mb-4">BMI Calculator</h2>
      <label>Height (cm):</label>
      <input type="number" id="height" class="border w-full p-2 rounded mb-2" value="170">
      <label>Weight (kg):</label>
      <input type="number" id="weight" class="border w-full p-2 rounded mb-2" value="65">
      <p class="text-lg font-semibold">BMI: <span id="bmi">0</span></p>
      <p id="bmiStatus" class="font-medium text-blue-700"></p>
    </section>

    <!-- Basic Calculator -->
    <section class="bg-white shadow-md rounded-2xl p-6">
      <h2 class="text-2xl font-semibold mb-4">Basic Calculator</h2>
      <input type="number" id="basicA" placeholder="First Number" class="w-full p-2 mb-2 border rounded">
      <input type="number" id="basicB" placeholder="Second Number" class="w-full p-2 mb-2 border rounded">
      <div class="grid grid-cols-5 gap-2 mb-2">
        <button onclick="basicOp('+')" class="bg-blue-500 text-white py-2 rounded">+</button>
        <button onclick="basicOp('-')" class="bg-blue-500 text-white py-2 rounded">-</button>
        <button onclick="basicOp('*')" class="bg-blue-500 text-white py-2 rounded">×</button>
        <button onclick="basicOp('/')" class="bg-blue-500 text-white py-2 rounded">÷</button>
        <button onclick="basicOp('%')" class="bg-blue-500 text-white py-2 rounded">%</button>
      </div>
      <p class="text-lg font-semibold">Result: <span id="basicResult">0</span></p>
    </section>

    <!-- Scientific Calculator -->
    <section class="bg-white shadow-md rounded-2xl p-6">
      <h2 class="text-2xl font-semibold mb-4">Scientific Calculator</h2>
      <input type="text" id="sciInput" placeholder="Enter expression like Math.sin(30)" class="w-full p-2 mb-2 border rounded">
      <button onclick="calculateSci()" class="bg-purple-500 text-white px-4 py-2 rounded">Calculate</button>
      <p class="text-lg font-semibold mt-2">Result: <span id="sciResult">0</span></p>
    </section>
  </div>

  <footer class="text-center mt-8 text-sm">Created by Chinnarasu</footer>

  <script>
    function formatCurrency(num) {
      return num.toLocaleString('en-IN');
    }

    // EMI Logic
    const loanInput = document.getElementById('loanInput');
    const loanAmount = document.getElementById('loanAmount');
    const rate = document.getElementById('rate');
    const tenure = document.getElementById('tenure');
    const emi = document.getElementById('emi');
    const principal = document.getElementById('principal');
    const interest = document.getElementById('interest');
    const total = document.getElementById('total');
    const loanLabel = document.getElementById('loanAmountLabel');
    const rateLabel = document.getElementById('rateLabel');
    const tenureLabel = document.getElementById('tenureLabel');

    function calculateEMI() {
      const P = parseInt(loanInput.value);
      const R = parseFloat(rate.value) / 12 / 100;
      const N = parseInt(tenure.value) * 12;
      const emiVal = Math.round((P * R * Math.pow(1 + R, N)) / (Math.pow(1 + R, N) - 1));
      const totalAmount = emiVal * N;
      const totalInterest = totalAmount - P;

      emi.textContent = formatCurrency(emiVal);
      principal.textContent = formatCurrency(P);
      interest.textContent = formatCurrency(totalInterest);
      total.textContent = formatCurrency(totalAmount);
      loanLabel.textContent = formatCurrency(P);
      rateLabel.textContent = rate.value;
      tenureLabel.textContent = tenure.value;
    }

    loanAmount.oninput = (e) => {
      loanInput.value = e.target.value;
      calculateEMI();
    };
    loanInput.oninput = () => {
      loanAmount.value = loanInput.value;
      calculateEMI();
    };
    rate.oninput = tenure.oninput = calculateEMI;
    calculateEMI();

    // BMI
    const height = document.getElementById('height');
    const weight = document.getElementById('weight');
    const bmi = document.getElementById('bmi');
    const bmiStatus = document.getElementById('bmiStatus');

    function calcBMI() {
      const h = height.value / 100;
      const w = weight.value;
      const b = (w / (h * h)).toFixed(1);
      bmi.textContent = b;
      if (b < 18.5) bmiStatus.textContent = 'Underweight – Eat more protein and train.';
      else if (b < 24.9) bmiStatus.textContent = 'Normal – Stay consistent!';
      else if (b < 29.9) bmiStatus.textContent = 'Overweight – Consider cardio & diet.';
      else bmiStatus.textContent = 'Obese – Consult a doctor and plan serious weight loss.';
    }

    height.oninput = weight.oninput = calcBMI;
    calcBMI();

    // Basic Calc
    function basicOp(op) {
      const a = parseFloat(document.getElementById('basicA').value);
      const b = parseFloat(document.getElementById('basicB').value);
      let res = 0;
      if (op === '+') res = a + b;
      else if (op === '-') res = a - b;
      else if (op === '*') res = a * b;
      else if (op === '/') res = a / b;
      else if (op === '%') res = (a / b) * 100;
      document.getElementById('basicResult').textContent = res;
    }

    // Sci Calc
    function calculateSci() {
      try {
        const expr = document.getElementById('sciInput').value;
        const result = Function('return ' + expr)();
        document.getElementById('sciResult').textContent = result;
      } catch {
        document.getElementById('sciResult').textContent = 'Invalid expression';
      }
    }
  </script>
</body>
</html>
