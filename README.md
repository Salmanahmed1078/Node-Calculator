# Node Calculator

![License](https://img.shields.io/badge/license-MIT-blue) ![Node](https://img.shields.io/badge/node-%3E%3D16-green) ![Tests](https://img.shields.io/badge/tests-passing-brightgreen) ![Coverage](https://img.shields.io/badge/coverage-100%25-brightgreen)

A command-line calculator built with Node.js supporting standard arithmetic, bitwise, and advanced math operations. Developed as a DevOps and SCD lab exercise focused on clean module design, input validation, and test coverage.

---

## Table of Contents

- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
- [Supported Operations](#supported-operations)
- [Programmatic API](#programmatic-api)
- [Running Tests](#running-tests)
- [Project Structure](#project-structure)
- [Error Handling](#error-handling)
- [Contributing](#contributing)
- [License](#license)

---

## Features

- Integer and decimal arithmetic
- Interactive REPL mode and single-command mode
- Expression parsing (no `eval` — custom parser)
- Full input validation with descriptive error messages
- 100% unit test coverage
- Zero runtime dependencies

---

## Prerequisites

- [Node.js](https://nodejs.org/) >= 16.0.0
- [npm](https://www.npmjs.com/) >= 8.0.0

---

## Installation

```bash
git clone https://github.com/Salmanahmed1078/Node-Calculator.git
cd Node-Calculator
npm install
```

---

## Usage

### Single operation mode

Pass the expression directly as command-line arguments:

```bash
node calculator.js 10 + 5        # 15
node calculator.js 20 / 4        # 5
node calculator.js 3 ** 3        # 27
node calculator.js 17 % 5        # 2
node calculator.js 100 sqrt      # 10
node calculator.js 45 sin        # 0.8509 (degrees)
```

### Interactive (REPL) mode

Run without arguments to enter interactive mode:

```bash
node calculator.js

> Calculator v1.0 — type 'help' for commands, 'exit' to quit

calc> 10 + 5
  = 15

calc> 3 ** 4
  = 81

calc> sqrt 144
  = 12

calc> history
  1. 10 + 5 = 15
  2. 3 ** 4 = 81
  3. sqrt 144 = 12

calc> exit
  Goodbye.
```

### Expression mode

Pass a full expression string in quotes:

```bash
node calculator.js "10 + 5 * 2"     # 20 (respects PEMDAS)
node calculator.js "(10 + 5) * 2"   # 30
```

---

## Supported Operations

### Arithmetic

| Operator | Description | Example | Result |
|---|---|---|---|
| `+` | Addition | `10 + 5` | `15` |
| `-` | Subtraction | `10 - 3` | `7` |
| `*` | Multiplication | `6 * 7` | `42` |
| `/` | Division | `20 / 4` | `5` |
| `%` | Modulo | `17 % 5` | `2` |
| `**` | Exponentiation | `2 ** 10` | `1024` |

### Math functions

| Command | Description | Example | Result |
|---|---|---|---|
| `sqrt` | Square root | `sqrt 144` | `12` |
| `abs` | Absolute value | `abs -42` | `42` |
| `floor` | Floor | `floor 3.9` | `3` |
| `ceil` | Ceiling | `ceil 3.1` | `4` |
| `round` | Round | `round 3.5` | `4` |
| `log` | Natural log | `log 2.718` | `1` |
| `log10` | Base-10 log | `log10 1000` | `3` |
| `sin` | Sine (degrees) | `sin 90` | `1` |
| `cos` | Cosine (degrees) | `cos 0` | `1` |
| `tan` | Tangent (degrees) | `tan 45` | `1` |

### Interactive commands

| Command | Description |
|---|---|
| `history` | Show last 20 calculations |
| `clear` | Clear history |
| `help` | Show available operations |
| `exit` / `quit` | Exit the calculator |

---

## Programmatic API

Import the calculator into your own Node.js code:

```js
const { calculate, evaluate } = require('./src/calculator');

// Simple binary operations
console.log(calculate(10, '+', 5));    // 15
console.log(calculate(7, '**', 3));    // 343
console.log(calculate(0, '/', 5));     // 0

// Expression evaluation
console.log(evaluate('10 + 5 * 2'));       // 20
console.log(evaluate('(10 + 5) * 2'));     // 30
console.log(evaluate('sqrt(144) + 2'));    // 14

// Math functions
const { mathFn } = require('./src/math');
console.log(mathFn('sqrt', 81));     // 9
console.log(mathFn('sin', 90));      // 1
```

### Return values

All functions return a number. If the operation is invalid, they throw a `CalculatorError` with a descriptive message.

```js
try {
  calculate(10, '/', 0);
} catch (err) {
  console.log(err.message);  // "Division by zero is not allowed"
  console.log(err.code);     // "DIVISION_BY_ZERO"
}
```

---

## Running Tests

```bash
# Run all tests
npm test

# Run with coverage
npm run test:coverage

# Watch mode
npm run test:watch

# Run specific test file
npx jest tests/arithmetic.test.js
```

### Test structure

```
tests/
├── arithmetic.test.js     # +, -, *, /, %, **
├── math-functions.test.js # sqrt, sin, cos, log, etc.
├── expression.test.js     # Expression parsing and PEMDAS
├── validation.test.js     # Input validation and error cases
└── cli.test.js            # CLI argument parsing
```

### Example test output

```
PASS tests/arithmetic.test.js
  Arithmetic operations
    ✓ adds two integers (2 ms)
    ✓ handles negative numbers
    ✓ multiplies decimals correctly
    ✓ throws on division by zero
    ✓ handles modulo with negatives

PASS tests/math-functions.test.js
  Math functions
    ✓ sqrt of perfect square
    ✓ sqrt of non-perfect square
    ✓ sin of 90 degrees returns 1
    ...

Test Suites: 5 passed, 5 total
Tests:       42 passed, 42 total
Coverage:    100%
```

---

## Project Structure

```
Node-Calculator/
├── src/
│   ├── calculator.js       # Core binary operations
│   ├── math.js             # Math function implementations
│   ├── parser.js           # Expression parser (no eval)
│   ├── validator.js        # Input validation logic
│   ├── history.js          # Calculation history manager
│   └── errors.js           # Custom error classes
├── tests/
│   ├── arithmetic.test.js
│   ├── math-functions.test.js
│   ├── expression.test.js
│   ├── validation.test.js
│   └── cli.test.js
├── calculator.js           # CLI entry point
├── jest.config.js
└── package.json
```

---

## Error Handling

The calculator validates all inputs before processing and throws typed errors:

| Error Code | Trigger | Message |
|---|---|---|
| `DIVISION_BY_ZERO` | Dividing by 0 | "Division by zero is not allowed" |
| `INVALID_OPERATOR` | Unknown operator | "Unknown operator: '^'" |
| `INVALID_NUMBER` | Non-numeric input | "Invalid number: 'abc'" |
| `DOMAIN_ERROR` | sqrt of negative | "Cannot take square root of a negative number" |
| `OVERFLOW` | Result > MAX_SAFE_INTEGER | "Result exceeds safe integer range" |

---

## Contributing

1. Fork the repository
2. Create a branch: `git checkout -b feat/your-feature`
3. Add tests for any new operations or functions
4. Ensure `npm test` passes with 100% coverage on changed files
5. Open a pull request

---

## License

MIT © Salman Ahmed
