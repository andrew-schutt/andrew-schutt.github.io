---
layout: post
title: "A Simple Ruby CLI for the Hyperfactorial"
date: 2025-10-05 11:48:15 -0500
categories: ruby math cli
---

## What is the hyperfactorial?

At a high level, the **hyperfactorial** of a non‑negative integer *n* is the product of each integer from 1 to *n*, raised to its own power:

\[ H(n) = \prod\limits_{k=1}^n k^k,\quad\text{with } H(0)=1. \]

For more background, see the MathWorld entry: [Hyperfactorial](https://mathworld.wolfram.com/Hyperfactorial.html?utm_source=cassidoo&utm_medium=email&utm_campaign=i-recommend-the-freedom-that-comes-from-asking).

### Small examples

- H(0) = 1  
- H(1) = 1  
- H(2) = 4  
- H(3) = 108  
- H(4) = 27,648  
- H(5) = 86,400,000  
- H(6) = 4,031,078,400,000  
- H(7) = 3,319,766,398,771,200,000  
- H(8) exceeds signed 64‑bit integer range

## Ruby CLI: straightforward, no flags

This standalone script accepts **exactly one** non‑negative integer argument (0 allowed).  
It prints the exact hyperfactorial if it fits in a signed 64‑bit integer; otherwise it prints `out of bounds`.  
Invalid input prints `invalid input` to STDERR and exits with code 1.

```ruby
#!/usr/bin/env ruby
# hyperfactorial

SIGNED_64_MAX = (2**63 - 1)

def invalid!
  $stderr.puts "invalid input"
  exit 1
end

# Validate arity
invalid! unless ARGV.length == 1

arg = ARGV[0]
# Validate non-negative integer (no signs, no decimals)
invalid! unless arg.match?(/\A\d+\z/)

n = arg.to_i

# H(0) = 1 by convention
result = 1

(1..n).each do |k|
  term = k ** k

  # Early overflow check for 64-bit signed integer
  # If term alone exceeds the limit, or result * term would exceed it -> out of bounds
  if term > SIGNED_64_MAX || result > SIGNED_64_MAX / term
    puts "out of bounds"
    exit 0
  end

  result *= term
end

puts result
```

## Usage

Make it executable and run with a single integer:

```bash
chmod +x hyperfactorial
./hyperfactorial 5
# 86400000
```

**Notes**  
- Valid input: `0, 1, 2, …`  
- Error behavior: `invalid input` to STDERR, exit code 1.  
- Range guard: prints `out of bounds` once the exact value would overflow a signed 64‑bit integer (H(8) and above).
