# Quadra Calc

import React, { useState, useEffect, useMemo } from 'react';

// CDN Imports - These would be in your index.html
// <script src="https://cdn.tailwindcss.com"></script>
// <script src="https://unpkg.com/recharts/umd/Recharts.min.js"></script>
// <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjs/12.4.1/math.js"></script>
// <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet">
// <script>
//   tailwind.config = {
//     theme: {
//       extend: {
//         fontFamily: {
//           poppins: ['Poppins', 'sans-serif'],
//         },
//       },
//     },
//   }
// </script>

// --- Helper Components ---

const GlassCard = ({ children, className = '' }) => (
    <div className={`bg-white/10 backdrop-blur-xl border border-white/20 rounded-2xl shadow-lg p-6 md:p-8 text-white ${className}`}>
        {children}
    </div>
);

const CustomSlider = ({ value, min, max, step, onChange, label, unit }) => (
    <div className="space-y-2">
        <div className="flex justify-between items-center">
            <label className="font-medium text-gray-300">{label}</label>
            <div className="flex items-center bg-white/10 rounded-lg px-3 py-1">
                <span className="text-lg font-semibold">{unit === '₹' && unit} {value.toLocaleString('en-IN')} {unit !== '₹' && unit}</span>
            </div>
        </div>
        <input
            type="range"
            min={min}
            max={max}
            step={step}
            value={value}
            onChange={onChange}
            className="w-full h-2 bg-white/20 rounded-lg appearance-none cursor-pointer range-lg accent-purple-400"
        />
        <div className="flex justify-between text-xs text-gray-400">
            <span>{unit === '₹' && unit} {min.toLocaleString('en-IN')}</span>
            <span>{unit === '₹' && unit} {max.toLocaleString('en-IN')}</span>
        </div>
    </div>
);


// --- Calculator Components ---

// 1. EMI Calculator
const EMICalculator = () => {
    const [loanAmount, setLoanAmount] = useState(1000000);
    const [interestRate, setInterestRate] = useState(8.5);
    const [tenure, setTenure] = useState(10);

    const { emi, totalPayment, totalInterest, principalPercent, interestPercent } = useMemo(() => {
        const principal = parseFloat(loanAmount);
        const rate = parseFloat(interestRate) / 100 / 12;
        const n = parseFloat(tenure) * 12;

        if (principal > 0 && rate > 0 && n > 0) {
            const calculatedEmi = (principal * rate * Math.pow(1 + rate, n)) / (Math.pow(1 + rate, n) - 1);
            const totalPayment = calculatedEmi * n;
            const totalInterest = totalPayment - principal;

            return {
                emi: Math.round(calculatedEmi),
                totalPayment: Math.round(totalPayment),
                totalInterest: Math.round(totalInterest),
                principalPercent: (principal / totalPayment) * 100,
                interestPercent: (totalInterest / totalPayment) * 100,
            };
        }
        return { emi: 0, totalPayment: 0, totalInterest: 0, principalPercent: 100, interestPercent: 0 };
    }, [loanAmount, interestRate, tenure]);

    const pieData = [
        { name: 'Principal Amount', value: loanAmount, fill: '#8884d8' },
        { name: 'Total Interest', value: totalInterest, fill: '#82ca9d' },
    ];
    
    // Check if Recharts is available on window object
    const { PieChart, Pie, Cell, Tooltip, ResponsiveContainer } = window.Recharts || {};


    return (
        <GlassCard className="w-full max-w-4xl mx-auto">
            <div className="grid md:grid-cols-2 gap-8 md:gap-12">
                {/* Left Side: Inputs */}
                <div className="space-y-6">
                    <h2 className="text-2xl font-bold text-center md:text-left">EMI Calculator</h2>
                    <CustomSlider
                        label="Loan Amount"
                        value={loanAmount}
                        min={1000}
                        max={10000000}
                        step={1000}
                        onChange={(e) => setLoanAmount(Number(e.target.value))}
                        unit="₹"
                    />
                    <CustomSlider
                        label="Interest Rate"
                        value={interestRate}
                        min={0.1}
                        max={25}
                        step={0.1}
                        onChange={(e) => setInterestRate(Number(e.target.value))}
                        unit="%"
                    />
                    <CustomSlider
                        label="Loan Tenure"
                        value={tenure}
                        min={1}
                        max={30}
                        step={1}
                        onChange={(e) => setTenure(Number(e.target.value))}
                        unit="Years"
                    />
                </div>

                {/* Right Side: Results & Chart */}
                <div className="flex flex-col items-center justify-center space-y-6">
                    <div className="text-center">
                        <p className="text-gray-300">Monthly EMI</p>
                        <p className="text-4xl font-bold text-purple-300">₹ {emi.toLocaleString('en-IN')}</p>
                    </div>
                    
                    <div className="w-full h-48 md:h-64">
                         {PieChart ? (
                            <ResponsiveContainer width="100%" height="100%">
                                <PieChart>
                                    <Tooltip
                                        contentStyle={{ backgroundColor: 'rgba(30, 30, 30, 0.8)', border: 'none', borderRadius: '10px' }}
                                        formatter={(value) => `₹ ${value.toLocaleString('en-IN')}`}
                                    />
                                    <Pie data={pieData} dataKey="value" nameKey="name" cx="50%" cy="50%" innerRadius={50} outerRadius={80} paddingAngle={5}>
                                        <Cell key={`cell-0`} fill="#a78bfa" />
                                        <Cell key={`cell-1`} fill="#60a5fa" />
                                    </Pie>
                                </PieChart>
                            </ResponsiveContainer>
                         ) : <p>Loading Chart...</p>}
                    </div>

                    <div className="w-full text-center space-y-3">
                        <div className="flex justify-between items-center bg-white/5 p-3 rounded-lg">
                           <div className="flex items-center gap-2">
                                <span className="w-3 h-3 rounded-full bg-purple-400"></span>
                                <p>Principal Amount</p>
                           </div>
                           <p className="font-semibold">₹ {loanAmount.toLocaleString('en-IN')}</p>
                        </div>
                         <div className="flex justify-between items-center bg-white/5 p-3 rounded-lg">
                           <div className="flex items-center gap-2">
                                <span className="w-3 h-3 rounded-full bg-blue-400"></span>
                                <p>Total Interest</p>
                           </div>
                           <p className="font-semibold">₹ {totalInterest.toLocaleString('en-IN')}</p>
                        </div>
                        <div className="flex justify-between items-center bg-white/5 p-3 rounded-lg font-bold">
                           <p>Total Payment</p>
                           <p>₹ {totalPayment.toLocaleString('en-IN')}</p>
                        </div>
                    </div>
                </div>
            </div>
        </GlassCard>
    );
};

// 2. BMI Calculator
const BMICalculator = () => {
    const [weight, setWeight] = useState(70);
    const [height, setHeight] = useState(175);
    const [bmiResult, setBmiResult] = useState(null);

    const calculateBMI = () => {
        if (weight > 0 && height > 0) {
            const heightInMeters = height / 100;
            const bmi = weight / (heightInMeters * heightInMeters);
            let category = '';
            let emoji = '';
            let suggestions = [];

            if (bmi < 18.5) {
                category = 'Underweight';
                emoji = '😔';
                suggestions = ["Focus on a nutrient-dense, high-protein diet.", "Incorporate resistance training to build muscle mass.", "Consider consulting a nutritionist for a personalized plan."];
            } else if (bmi >= 18.5 && bmi <= 24.9) {
                category = 'Healthy Weight';
                emoji = '✅';
                suggestions = ["Maintain your balanced diet and regular physical activity.", "Continue monitoring your health to stay in this range.", "Great job! Keep up the healthy habits."];
            } else if (bmi >= 25 && bmi <= 29.9) {
                category = 'Overweight';
                emoji = '⚠️';
                suggestions = ["Incorporate moderate calorie control in your diet.", "Aim for at least 30-60 minutes of walking or cardio daily.", "Increase water intake and reduce sugary drinks."];
            } else {
                category = 'Obese';
                emoji = '❌';
                suggestions = ["Seek guidance from a healthcare professional or a registered dietitian.", "Focus on a structured plan for calorie deficit and portion control.", "Start with low-impact exercises like swimming or cycling."];
            }

            setBmiResult({
                bmi: bmi.toFixed(1),
                category,
                emoji,
                suggestions
            });
        } else {
            setBmiResult(null);
        }
    };

    useEffect(() => {
        calculateBMI();
    }, [weight, height]);

    return (
        <GlassCard className="w-full max-w-lg mx-auto">
            <h2 className="text-2xl font-bold text-center mb-6">BMI Calculator</h2>
            <div className="space-y-6">
                <div className="space-y-2">
                    <label className="font-medium text-gray-300">Weight (kg)</label>
                    <input type="number" value={weight} onChange={(e) => setWeight(Number(e.target.value))} className="w-full bg-white/10 rounded-lg p-3 text-lg font-semibold focus:outline-none focus:ring-2 focus:ring-green-400" />
                </div>
                <div className="space-y-2">
                    <label className="font-medium text-gray-300">Height (cm)</label>
                    <input type="number" value={height} onChange={(e) => setHeight(Number(e.target.value))} className="w-full bg-white/10 rounded-lg p-3 text-lg font-semibold focus:outline-none focus:ring-2 focus:ring-green-400" />
                </div>
            </div>

            {bmiResult && (
                <div className="mt-8 text-center bg-white/5 p-6 rounded-xl">
                    <p className="text-gray-300">Your BMI</p>
                    <p className="text-5xl font-bold my-2">{bmiResult.bmi}</p>
                    <p className="text-xl font-semibold">{bmiResult.category} {bmiResult.emoji}</p>

                    <div className="mt-6 text-left space-y-2">
                         <h3 className="font-semibold text-lg text-green-300">Suggestions:</h3>
                         <ul className="list-disc list-inside text-gray-300 space-y-1">
                             {bmiResult.suggestions.map((s, i) => <li key={i}>{s}</li>)}
                         </ul>
                    </div>
                </div>
            )}
        </GlassCard>
    );
};

// 3. Basic Calculator
const BasicCalculator = () => {
    const [display, setDisplay] = useState('0');

    const handleInput = (value) => {
        if (display === '0' && value !== '.') {
            setDisplay(value);
        } else if (value === '.' && display.includes('.')) {
            return;
        } else {
            setDisplay(display + value);
        }
    };

    const handleOperator = (operator) => {
        const lastChar = display.slice(-1);
        if (['+', '-', '*', '/'].includes(lastChar)) {
            setDisplay(display.slice(0, -1) + operator);
        } else {
            setDisplay(display + operator);
        }
    };

    const handleClear = () => setDisplay('0');
    
    const handleBackspace = () => {
        if (display.length > 1) {
            setDisplay(display.slice(0, -1));
        } else {
            setDisplay('0');
        }
    };
    
    const calculate = () => {
        try {
            // A simple, safer eval replacement for basic math
            const result = new Function('return ' + display.replace(/%/g, '/100*'))();
            setDisplay(String(result));
        } catch (error) {
            setDisplay('Error');
        }
    };

    const NeumorphicButton = ({ children, onClick, className = '' }) => (
        <button
            onClick={onClick}
            className={`rounded-2xl shadow-[5px_5px_10px_#1a1a1a,-5px_-5px_10px_#2c2c2c] active:shadow-[inset_5px_5px_10px_#1a1a1a,inset_-5px_-5px_10px_#2c2c2c] text-2xl font-semibold transition-all duration-150 ${className}`}
        >
            {children}
        </button>
    );

    return (
        <div className="w-full max-w-sm mx-auto p-4 bg-[#232323] rounded-3xl shadow-2xl">
            <div className="bg-black/20 text-white text-right p-4 rounded-2xl mb-4 shadow-inner">
                <p className="text-5xl font-light break-words">{display}</p>
            </div>
            <div className="grid grid-cols-4 gap-4 text-white">
                <NeumorphicButton onClick={handleClear} className="bg-red-500/80">AC</NeumorphicButton>
                <NeumorphicButton onClick={handleBackspace} className="bg-gray-500/80">C</NeumorphicButton>
                <NeumorphicButton onClick={() => setDisplay(display + '%')} className="bg-gray-500/80">%</NeumorphicButton>
                <NeumorphicButton onClick={() => handleOperator('/')} className="bg-orange-500/80">/</NeumorphicButton>

                <NeumorphicButton onClick={() => handleInput('7')}>7</NeumorphicButton>
                <NeumorphicButton onClick={() => handleInput('8')}>8</NeumorphicButton>
                <NeumorphicButton onClick={() => handleInput('9')}>9</NeumorphicButton>
                <NeumorphicButton onClick={() => handleOperator('*')} className="bg-orange-500/80">*</NeumorphicButton>

                <NeumorphicButton onClick={() => handleInput('4')}>4</NeumorphicButton>
                <NeumorphicButton onClick={() => handleInput('5')}>5</NeumorphicButton>
                <NeumorphicButton onClick={() => handleInput('6')}>6</NeumorphicButton>
                <NeumorphicButton onClick={() => handleOperator('-')} className="bg-orange-500/80">-</NeumorphicButton>

                <NeumorphicButton onClick={() => handleInput('1')}>1</NeumorphicButton>
                <NeumorphicButton onClick={() => handleInput('2')}>2</NeumorphicButton>
                <NeumorphicButton onClick={() => handleInput('3')}>3</NeumorphicButton>
                <NeumorphicButton onClick={() => handleOperator('+')} className="bg-orange-500/80">+</NeumorphicButton>

                <NeumorphicButton onClick={() => handleInput('0')} className="col-span-2">0</NeumorphicButton>
                <NeumorphicButton onClick={() => handleInput('.')}>.</NeumorphicButton>
                <NeumorphicButton onClick={calculate} className="bg-orange-500/80">=</NeumorphicButton>
            </div>
        </div>
    );
};

// 4. Scientific Calculator
const ScientificCalculator = () => {
    const [expression, setExpression] = useState('');
    const [result, setResult] = useState('0');
    
    // Check if mathjs is available
    const math = window.math;

    const handleInput = (value) => {
        setExpression(prev => prev + value);
    };

    const clearAll = () => {
        setExpression('');
        setResult('0');
    };
    
    const backspace = () => {
        setExpression(prev => prev.slice(0, -1));
    };

    const calculateResult = () => {
        if (!math) {
            setResult('Error: Math library not loaded');
            return;
        }
        if (expression === '') return;
        try {
            const evalResult = math.evaluate(expression);
            setResult(String(evalResult));
        } catch (error) {
            setResult('Error');
        }
    };
    
    useEffect(() => {
        const handleKeyPress = (event) => {
            const key = event.key;
            if (/[0-9]/.test(key)) {
                handleInput(key);
            } else if (['+', '-', '*', '/', '.', '(', ')'].includes(key)) {
                handleInput(key);
            } else if (key === 'Enter' || key === '=') {
                event.preventDefault();
                calculateResult();
            } else if (key === 'Backspace') {
                backspace();
            } else if (key.toLowerCase() === 'c') {
                clearAll();
            }
        };

        window.addEventListener('keydown', handleKeyPress);
        return () => window.removeEventListener('keydown', handleKeyPress);
    }, [expression]);

    const Button = ({ children, onClick, className = '' }) => (
        <button onClick={onClick} className={`bg-gray-700 hover:bg-gray-600 rounded-lg text-lg font-medium transition-colors duration-200 focus:outline-none focus:ring-2 focus:ring-blue-400 ${className}`}>
            {children}
        </button>
    );

    const buttons = [
        { label: 'sin', action: () => handleInput('sin(') }, { label: 'cos', action: () => handleInput('cos(') }, { label: 'tan', action: () => handleInput('tan(') },
        { label: 'log', action: () => handleInput('log10(') }, { label: 'ln', action: () => handleInput('log(') },
        { label: '(', action: () => handleInput('(') }, { label: ')', action: () => handleInput(')') },
        { label: '√', action: () => handleInput('sqrt(') }, { label: '^', action: () => handleInput('^') },
        { label: 'π', action: () => handleInput('pi') }, { label: 'e', action: () => handleInput('e') },
        { label: 'AC', action: clearAll, className: 'bg-red-600/80 hover:bg-red-500/80' },
        { label: 'C', action: backspace, className: 'bg-orange-600/80 hover:bg-orange-500/80' },
        { label: '/', action: () => handleInput('/') }, { label: '*', action: () => handleInput('*') },
        { label: '7', action: () => handleInput('7') }, { label: '8', action: () => handleInput('8') }, { label: '9', action: () => handleInput('9') }, { label: '-', action: () => handleInput('-') },
        { label: '4', action: () => handleInput('4') }, { label: '5', action: () => handleInput('5') }, { label: '6', action: () => handleInput('6') }, { label: '+', action: () => handleInput('+') },
        { label: '1', action: () => handleInput('1') }, { label: '2', action: () => handleInput('2') }, { label: '3', action: () => handleInput('3') },
        { label: '0', action: () => handleInput('0') }, { label: '.', action: () => handleInput('.') },
        { label: '=', action: calculateResult, className: 'row-span-2 bg-blue-600/80 hover:bg-blue-500/80' }
    ];

    return (
        <div className="w-full max-w-md mx-auto p-4 bg-gray-900 rounded-2xl shadow-2xl border border-gray-700">
            {/* Display */}
            <div className="bg-gray-800 text-white text-right p-4 rounded-lg mb-4 shadow-inner">
                <div className="text-gray-400 text-xl h-7 truncate">{expression || ' '}</div>
                <div className="text-4xl font-semibold break-words">{result}</div>
            </div>

            {/* Buttons */}
            <div className="grid grid-cols-5 grid-rows-6 gap-2 text-white h-80">
                 {/* Special Functions */}
                <Button onClick={() => handleInput('sin(')}>sin</Button>
                <Button onClick={() => handleInput('cos(')}>cos</Button>
                <Button onClick={() => handleInput('tan(')}>tan</Button>
                <Button onClick={() => handleInput('log10(')}>log</Button>
                <Button onClick={() => handleInput('log(')}>ln</Button>

                <Button onClick={() => handleInput('(')}>(</Button>
                <Button onClick={() => handleInput(')')}>)</Button>
                <Button onClick={() => handleInput('sqrt(')}>√</Button>
                <Button onClick={() => handleInput('^')}>^</Button>
                <Button onClick={() => handleInput('pi')}>π</Button>
                
                {/* Main Pad */}
                <div className="col-span-3 row-span-4 grid grid-cols-3 gap-2">
                    <Button onClick={() => handleInput('7')}>7</Button>
                    <Button onClick={() => handleInput('8')}>8</Button>
                    <Button onClick={() => handleInput('9')}>9</Button>
                    <Button onClick={() => handleInput('4')}>4</Button>
                    <Button onClick={() => handleInput('5')}>5</Button>
                    <Button onClick={() => handleInput('6')}>6</Button>
                    <Button onClick={() => handleInput('1')}>1</Button>
                    <Button onClick={() => handleInput('2')}>2</Button>
                    <Button onClick={() => handleInput('3')}>3</Button>
                    <Button onClick={() => handleInput('0')} className="col-span-2">0</Button>
                    <Button onClick={() => handleInput('.')}>.</Button>
                </div>
                
                {/* Operators */}
                <div className="col-start-4 row-start-3 row-span-4 grid grid-cols-2 gap-2">
                    <Button onClick={clearAll} className="bg-red-600/80 hover:bg-red-500/80">AC</Button>
                    <Button onClick={backspace} className="bg-orange-600/80 hover:bg-orange-500/80">C</Button>
                    <Button onClick={() => handleInput('/')}>/</Button>
                    <Button onClick={() => handleInput('*')}>*</Button>
                    <Button onClick={() => handleInput('-')}>-</Button>
                    <Button onClick={() => handleInput('+')}>+</Button>
                    <Button onClick={calculateResult} className="col-span-2 bg-blue-600/80 hover:bg-blue-500/80">=</Button>
                </div>
            </div>
        </div>
    );
};


// --- Main App Component ---

const App = () => {
    const [activeCalc, setActiveCalc] = useState('EMI');

    const calculators = {
        'EMI': <EMICalculator />,
        'BMI': <BMICalculator />,
        'Basic': <BasicCalculator />,
        'Scientific': <ScientificCalculator />,
    };

    const NavButton = ({ name }) => (
        <button
            onClick={() => setActiveCalc(name)}
            className={`px-4 py-2 text-sm md:text-base font-semibold rounded-lg transition-all duration-300
                ${activeCalc === name ? 'bg-white/20 text-white' : 'text-gray-300 hover:bg-white/10'}`}
        >
            {name}
        </button>
    );

    const Logo = () => (
        <div className="flex items-center gap-2">
            <svg width="32" height="32" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path d="M12 2C6.48 2 2 6.48 2 12C2 17.52 6.48 22 12 22C17.52 22 22 17.52 22 12C22 6.48 17.52 2 12 2ZM13.04 18H10.96V15.61L12 14.58L13.04 15.61V18ZM13.04 13.42L12 12.39L10.96 13.42V11.38H13.04V13.42ZM15.61 13.04L14.58 12L15.61 10.96H18V13.04H15.61ZM8.39 13.04H6V10.96H8.39L9.42 12L8.39 13.04ZM13.04 8.62V6H10.96V8.62L12 9.66L13.04 8.62Z" fill="url(#logo-gradient)"/>
                <defs>
                    <linearGradient id="logo-gradient" x1="2" y1="2" x2="22" y2="22" gradientUnits="userSpaceOnUse">
                        <stop stopColor="#a855f7"/>
                        <stop offset="1" stopColor="#60a5fa"/>
                    </linearGradient>
                </defs>
            </svg>
            <span className="font-bold text-xl text-white tracking-wider">QuadraCalc</span>
        </div>
    );


    return (
        <div className="min-h-screen w-full bg-gray-900 font-poppins text-white selection:bg-purple-500/50" style={{background: 'linear-gradient(135deg, #1e1b4b 0%, #312e81 50%, #1e1b4b 100%)'}}>
            <style>
                {`body { overflow-x: hidden; }`}
            </style>
            <header className="sticky top-0 z-50 bg-gray-900/30 backdrop-blur-lg border-b border-white/10">
                <nav className="container mx-auto px-4 sm:px-6 lg:px-8">
                    <div className="flex items-center justify-between h-16">
                        <Logo />
                        <div className="hidden sm:flex items-center space-x-2 bg-gray-800/50 p-1 rounded-xl">
                            {Object.keys(calculators).map(name => <NavButton key={name} name={name} />)}
                        </div>
                        {/* Mobile Menu could be added here */}
                    </div>
                </nav>
                {/* Mobile Tab Bar */}
                <div className="sm:hidden flex items-center justify-around bg-gray-800/50 p-1">
                     {Object.keys(calculators).map(name => <NavButton key={name} name={name} />)}
                </div>
            </header>

            <main className="py-10 px-4">
                 <div className="text-center mb-10">
                    <h1 className="text-4xl md:text-5xl font-bold tracking-tight">QuadraCalc <span className="text-gray-400">by Chinnarasu</span></h1>
                    <p className="mt-3 text-lg md:text-xl text-gray-300">One Website. Four Smart Calculators.</p>
                </div>

                <div className="transition-all duration-500 ease-in-out">
                    {calculators[activeCalc]}
                </div>
            </main>

            <footer className="py-6 text-center text-gray-400">
                <p>Created by Chinnarasu</p>
            </footer>
        </div>
    );
};

export default App;
