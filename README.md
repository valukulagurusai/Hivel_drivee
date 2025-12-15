# Polynomial Secret Solver

A frontend-only web application that reconstructs the constant term of a polynomial using Lagrange interpolation. Built with pure HTML, CSS, and JavaScript—no frameworks, no dependencies, no backend.

## Features

- **100% Browser-based**: All computation happens in the browser using only HTML, CSS, and vanilla JavaScript
- **BigInt Support**: Handles arbitrarily large numbers using JavaScript's BigInt
- **Multi-base Input**: Accepts values in any base from 2 to 36
- **Safe Base Conversion**: Custom base-to-BigInt conversion that doesn't rely on unsafe methods
- **Lagrange Interpolation**: Mathematically correct polynomial reconstruction at x=0
- **Error Handling**: Graceful validation with clear error messages
- **Clean UI**: Modern, responsive design suitable for placement assignments

## Getting Started

### Prerequisites
- A modern web browser (Chrome, Firefox, Safari, Edge)

### Usage

1. **Open the application**: Simply open `index.html` in your web browser
2. **Prepare JSON file**: Create or use a test JSON file with the required format
3. **Upload file**: Click the file input and select your JSON file
4. **Solve**: Click the "Solve" button
5. **View result**: The constant term will be displayed as "Constant term (c) = <value>"

## JSON File Format

The input JSON must follow this exact structure:

```json
{
  "keys": {
    "n": <number of roots>,
    "k": <minimum roots required>
  },
  "<x1>": {
    "base": "<base>",
    "value": "<value>"
  },
  "<x2>": {
    "base": "<base>",
    "value": "<value>"
  }
}
```

### Format Details

- **keys.n**: Total number of roots available (integer)
- **keys.k**: Number of roots required to reconstruct the polynomial (integer, must be ≥ 2)
- **Each root key**: The x-coordinate of the root (as a string number)
- **base**: The numeric base of the value (2-36)
- **value**: The y-coordinate encoded in the specified base (string)

### Example

```json
{
  "keys": {
    "n": 4,
    "k": 3
  },
  "1": {
    "base": "10",
    "value": "4"
  },
  "2": {
    "base": "2",
    "value": "111"
  },
  "3": {
    "base": "10",
    "value": "12"
  },
  "6": {
    "base": "4",
    "value": "213"
  }
}
```

This example has 4 available roots but requires only 3 to reconstruct the polynomial. The application will use the first 3 roots (sorted by x-value).

## Testing

Two test files are provided:

### test_input.json
- Simple test case with 4 roots
- Bases: decimal (10), binary (2), quaternary (4)
- Expected behavior: Uses first 3 roots (x=1, 2, 3) to compute constant term

### test_input_large.json
- Complex test with 9 roots and large numbers
- Bases: decimal (10), hex (16), base-12, base-11, base-14, octal (8), base-9
- Tests: Large number handling, multiple base conversions, correct BigInt arithmetic

### How to Test

1. Open `index.html` in your browser
2. Click "Upload" and select `test_input.json`
3. Click "Solve" and verify the output appears
4. Repeat with `test_input_large.json` to test large number handling
5. Output should change based on the input data

## Technical Implementation

### Core Algorithms

#### Safe Base-to-BigInt Conversion
```javascript
function baseToDecimal(value, base) {
  // Safely converts any base (2-36) to BigInt
  // Validates each digit and base range
  // Returns exact BigInt result
}
```

#### Lagrange Interpolation
```javascript
function lagrangeInterpolation(roots) {
  // For each root point (x_i, y_i)
  // Computes L_i(0) = product of (-x_j / (x_i - x_j))
  // Sums y_i * L_i(0) to get f(0)
}
```

#### Exact Division Verification
- Uses GCD to simplify fractions
- Ensures denominator equals 1 (exact division)
- Throws error if polynomial won't have integer coefficients

### Key Features

1. **Root Sorting**: Roots are sorted by x-value before computation
2. **Root Selection**: Exactly k roots are selected (first k in sorted order)
3. **BigInt Arithmetic**: All calculations use BigInt to avoid floating-point errors
4. **Error Handling**: Validates JSON structure, base ranges, digit validity, and division exactness

## Browser Compatibility

Works in all modern browsers:
- Chrome/Edge 67+
- Firefox 68+
- Safari 14+
- Opera 54+

Requires support for:
- BigInt primitive type
- ES6+ JavaScript features
- FileReader API

## Mathematical Background

This application implements Shamir's Secret Sharing scheme recovery algorithm:

Given k points (x_i, f(x_i)) on a polynomial f(x) of degree k-1, the constant term f(0) can be uniquely determined using Lagrange interpolation:

```
f(0) = Σ(i=0 to k-1) f(x_i) * L_i(0)

where L_i(0) = Π(j≠i) (-x_j / (x_i - x_j))
```

All arithmetic is performed over integers using BigInt for mathematical correctness.

## File Structure

```
project/
├── index.html              # Main application (HTML + CSS + JavaScript)
├── test_input.json         # Simple test case
├── test_input_large.json   # Complex test case with large numbers
└── README.md              # This file
```

## No Dependencies

This application uses absolutely no external libraries or frameworks:
- No React, Vue, Angular
- No jQuery, Lodash, or utility libraries
- No npm or build tools
- Pure HTML, CSS, and JavaScript
- Works offline

## Performance

- Handles large numbers efficiently with BigInt
- Linear time complexity for base conversion: O(digits)
- O(k²) time complexity for Lagrange interpolation with k roots
- Suitable for k ≤ 100 roots on standard hardware

## License

Free to use for educational and placement assignment purposes.
