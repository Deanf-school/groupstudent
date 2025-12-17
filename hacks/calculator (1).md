---
title: JS Calculator
comments: true
hide: true
layout: opencs
description: A modern, responsive calculator built with JavaScript. Features all basic operations and a clean UI.
permalink: /calculator
---

<style>

* {
  box-sizing: border-box;
}

/* Container for all buttons */
.calculator-container {
  display: grid;
  grid-template-columns: repeat(4, 70px); 
  grid-auto-rows: 70px; 
  gap: 10px; 
  padding: 10px;
  border: 2px solid #333;
  border-radius: 8px;
  background-color: #f0f0f0;
  width: max-content; 
  margin: auto; 

/* All buttons */
.calculator-number,
.calculator-operation,
.calculator-clear,
.calculator-equals {
  border: 1px solid #333;
  border-radius: 5px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 20px;
  cursor: pointer;
}
/* Output display */
#output {
  grid-column: span 4;       /* Make it span all 4 columns */
  height: 80px;              /* Make it taller */
  font-size: 36px;           /* Bigger numbers */
  text-align: right;         /* Align numbers to the right */
  padding: 10px;             /* Space inside the box */
  border: 2px solid #333;    /* Border around output */
  border-radius: 8px;        /* Rounded corners */
  background-color: #fff;    /* White background */
  box-sizing: border-box;    /* Include border in size */
}
</style>






<!-- Calculator container -->
<div id="animation">
  <div class="calculator-container">
      <!-- Display -->
      <div class="calculator-output" id="output">0</div>
      
      <!-- Row 1 -->
      <div class="calculator-clear">A/C</div>
      <div class="calculator-operation">/</div>
      <div class="calculator-operation">*</div>
      <div class="calculator-operation">-</div>
      
      <!-- Row 2 -->
      <div class="calculator-number">1</div>
      <div class="calculator-number">2</div>
      <div class="calculator-number">3</div>
      <div class="calculator-operation">+</div>
      
      <!-- Row 3 -->
      <div class="calculator-number">4</div>
      <div class="calculator-number">5</div>
      <div class="calculator-number">6</div>
      
      <!-- Row 4 -->
      <div class="calculator-number">0</div>
      <div class="calculator-number">7</div>
      <div class="calculator-number">8</div>
      
      <!-- Row 5 -->
      <div class="calculator-number">9</div>
      <div class="calculator-equals">=</div>
      <div class="calculator-operation">√</div>
   
  </div>
</div>

<!-- JavaScript implementation -->
<script>
// Calculator state
let firstNumber = null;
let operator = null;
let nextReady = true;

const output = document.getElementById("output");
const numbers = document.querySelectorAll(".calculator-number");
const operations = document.querySelectorAll(".calculator-operation");
const clear = document.querySelectorAll(".calculator-clear");
const equals = document.querySelectorAll(".calculator-equals");



// Number buttons
numbers.forEach(button => {
  button.addEventListener("click", () => {
    number(button.textContent);
  });
});

function number(value) {
  if (nextReady) {
    output.textContent = value;
    nextReady = false;
  } else {
    output.textContent += value;
  }
}

// Operation buttons
operations.forEach(button => {
  button.addEventListener("click", () => {
    operation(button.textContent);
  });
});

// Function to handle all keyboard input
function handleKeyboardInput(event) {
    const key = event.key;
    

    if (key === 'Backspace') {
        event.preventDefault();
    }
    
    // Numbers and Decimal points 
    if (/[0-9.]/.test(key)) {
      
        number(key);
    }
    
    //  Operations 
    else if (/[+\-*/]/.test(key)) {
      
        operation(key);
    } 
    
    //  equals
    else if (key === 'Enter' || key === '=') {
       
        
       
        if (firstNumber !== null && operator !== null) {
            firstNumber = calculate(firstNumber, parseFloat(output.textContent));
            output.textContent = firstNumber;
            firstNumber = null;
            operator = null;
            nextReady = true;
        }

        
        event.preventDefault(); 
    } 
    
    // esc is the key for A/C 
    else if (key === 'Escape') {
        
        firstNumber = null;
        operator = null;
        output.textContent = "0";
        nextReady = true;
    }
}


document.addEventListener('keydown', handleKeyboardInput);

function operation(choice) {
    if (firstNumber === null) {
        firstNumber = parseFloat(output.textContent);
        operator = choice; // store operator ("+", "-", "*", "/", "√")
        nextReady = true;
    } else {
        firstNumber = calculate(firstNumber, parseFloat(output.textContent));
        operator = choice;
        output.textContent = firstNumber;
        nextReady = true;
    }
}



function squareRoot() {
  let num = parseFloat(output.textContent);

  if (isNaN(num) || num < 0) {
        output.textContent = "Error";
        firstNumber = null;
        operator = null;
        nextReady = true;
        return;
  }

  let result = Math.sqrt(num);

  output.textContent = result.toString();
  firstNumber = result;
  operator = null;
  nextReady = true;
}

// Calculation logic
function calculate(first, second) {
  let result = 0;
  switch (operator) {
    case "+":
      result = first + second;
      break;
    case "-":
      result = first - second;
      break;
    case "*":
      
      result = first * second;
      break;
    case "/":
    
      if (second == 0) {
        result = "error";
      } else {
        result = first / second;
      }
      break;
    case "√":
      if (first < 0) {
        result = "error";
      } else {
        result = Math.sqrt(first);
      }
      break;
     default:
      result = first;
  }
  return result;
}

// Equals button
equals.forEach(button => {
  button.addEventListener("click", () => {
    if (firstNumber !== null && operator !== null) {
      firstNumber = calculate(firstNumber, parseFloat(output.textContent));
      output.textContent = firstNumber;
      firstNumber = null;
      operator = null;
      nextReady = true;
    }
  });
});

// Clear button
clear.forEach(button => {
  button.addEventListener("click", () => {
    firstNumber = null;
    operator = null;
    output.textContent = "0";
    nextReady = true;
  });
});
</script>

<!-- 
Vanta animations just for fun, load JS onto the page
-->
<script src="{{site.baseurl}}/assets/js/three.r119.min.js"></script>
<script src="{{site.baseurl}}/assets/js/vanta.halo.min.js"></script>
<script src="{{site.baseurl}}/assets/js/vanta.birds.min.js"></script>
<script src="{{site.baseurl}}/assets/js/vanta.net.min.js"></script>
<script src="{{site.baseurl}}/assets/js/vanta.rings.min.js"></script>

<script>
// setup vanta scripts as functions
var vantaInstances = {
  halo: VANTA.HALO,
  birds: VANTA.BIRDS,
  net: VANTA.NET,
  rings: VANTA.RINGS
};

// obtain a random vanta function
var vantaInstance = vantaInstances[Object.keys(vantaInstances)[Math.floor(Math.random() * Object.keys(vantaInstances).length)]];

// run the animation
vantaInstance({
  el: "#animation",
  mouseControls: true,
  touchControls: true,
  gyroControls: false
});
</script>
