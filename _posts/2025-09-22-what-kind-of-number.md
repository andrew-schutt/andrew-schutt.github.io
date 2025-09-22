---
layout: post
title: "Four Ancient Ways to Describe Numbers"
date: 2025-09-22 10:06:02
categories: numbers programming
---
The ancient Greeks didn’t just study numbers for calculation they also explored their personalities. They asked: what happens when you add up all the proper divisors of a number (that is, all the positive divisors excluding the number itself)? From this question came four beautiful classifications. Insipired by this [newsletter](https://buttondown.com/cassidoo/archive/the-love-that-you-withhold-is-the-pain-that-you/) from [Cassidoo](https://cassidoo.co/)

## Abundant Numbers

A number is called abundant if the sum of its proper divisors is greater than the number itself.

Example: 12 has divisors 1, 2, 3, 4, 6 → sum = 16, which is more than 12.

Interpretation: it has “more than enough” divisors.

## Deficient Numbers

A number is deficient if the sum of its proper divisors is less than the number itself.

Example: 8 has divisors 1, 2, 4 → sum = 7, which is less than 8.

Interpretation: it “falls short.”

## Perfect Numbers

A number is perfect when the sum of its proper divisors is exactly equal to the number itself.

Example: 28 has divisors 1, 2, 4, 7, 14 → sum = 28.

Interpretation: balanced and complete. (The Greeks admired these numbers greatly.)

## Amicable Numbers

Here, the focus shifts to pairs. Two numbers are amicable if each number’s proper divisors add up to the other.

Example: 220 and 284.

Divisors of 220: sum = 284.

Divisors of 284: sum = 220.

Interpretation: a friendship between numbers.

## Try It Yourself: Ruby Program

If you’d like to experiment with these number types, here’s a simple Ruby command-line program. You can run it with one number (to check if it’s abundant, deficient, or perfect) or with two numbers (to check if they’re amicable).

```ruby
#!/usr/bin/env ruby
# whatKindOfNumber.rb

require 'optparse'

options = {
  verbose: false
}

OptionParser.new do |opts|
  opts.banner = "Usage: my_program.rb [options] number [number]"

  opts.on("-v", "--verbose", "Run verbosely") do
    options[:verbose] = true
  end

  opts.on("-h", "--help", "Prints help") do
    puts opts
    exit
  end
end.parse!

def proper_divisors(n)
  return [] if n <= 1

  divisors = [1]
  limit = Math.sqrt(n).floor

  (2..limit).each do |i|
    if n % i == 0
      divisors << i
      complement = n / i
      divisors << complement if complement != i && complement != n
    end
  end

  divisors.sort
end

def compare_divisor_sums(numbers, divisors)
  if divisors[0].sum == numbers[1] && divisors[1].sum == numbers[0]
    return 'amicable'
  end
  if divisors[0].sum == numbers[0]
    return 'perfect'
  end
  if divisors[0].sum > numbers[0]
    return 'abundant'
  end
  if divisors[0].sum < numbers[0]
    return 'deficient'
  end

  return 'nothing'
end

if ARGV.empty?
  puts "Error: no number(s) provided.\n\nUsage: my_program.rb [options] number [number]"
  exit 1
end

numbers = ARGV.map(&:to_i)

if numbers.size == 1
  n = numbers.first
  result = compare_divisor_sums([n, -1], [proper_divisors(n), -1])
  puts result
elsif numbers.size == 2
  n1, n2 = numbers
  result = compare_divisor_sums([n1, n2], [proper_divisors(n1), proper_divisors(n2)])
  puts result
else
  puts "Error: too many arguments. Please provide one or two numbers."
  exit 1
end
```

Closing Thought

What’s fascinating is that this classification is over two thousand years old, yet still sparks curiosity today. Abundant, deficient, perfect, and amicable numbers remind us that math isn’t only about utility—it’s also about beauty and relationships.

Source:
https://www.encyclopedia.com/education/news-wires-white-papers-and-books/numbers-abundant-deficient-perfect-and-amicable

