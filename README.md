<p align="center">
  <img src="https://github.com/ayogun/42-project-badges/raw/main/covers/cover-push_swap-bonus.png" alt="push_swap cover" width="100%">
</p>

<h1 align="center">
  <a href="https://github.com/fernandoruanb/push_swap">
    <img src="https://raw.githubusercontent.com/ayogun/42-project-badges/main/badges/push_swapm.png" alt="push_swap badge" width="200">
  </a>
  <br>
  push_swap
  <br>
</h1>

<h4 align="center">
  A sorting project built in <a href="https://www.c-language.org/" target="_blank">C</a>, focused on algorithmic strategy, cost analysis, stack manipulation, and optimization under strict operation limits.
</h4>

<p align="center">
  <img src="https://img.shields.io/badge/Final%20Score-125%2F125-00C853?style=for-the-badge" alt="Final Score 125/125">
  <img src="https://img.shields.io/badge/Language-C-00599C?style=for-the-badge&logo=c" alt="Language C">
  <img src="https://img.shields.io/badge/Algorithm-Mechanical%20Turk-blueviolet?style=for-the-badge" alt="Mechanical Turk Algorithm">
  <img src="https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge" alt="Status Completed">
</p>

<p align="center">
  <a href="#about-the-project">About</a> •
  <a href="#core-idea">Core Idea</a> •
  <a href="#input-validation">Input Validation</a> •
  <a href="#algorithm-choice-and-cost">Algorithm Choice</a> •
  <a href="#mechanical-turk-strategy">Mechanical Turk Strategy</a> •
  <a href="#sorting-examples">Sorting Examples</a> •
  <a href="#checker-bonus">Checker Bonus</a> •
  <a href="#how-to-use">How To Use</a> •
  <a href="#team">Team</a>
</p>

---

## About the Project

**push_swap** is an introduction to the world of **sorting algorithms**, but with a very particular constraint:

the goal is not simply to sort numbers correctly.

The goal is to sort them using **only two stacks** and a **limited set of operations**, while keeping the total number of moves as low as possible.

That changes everything.

This project teaches an important lesson very early:

an algorithm that is excellent for sorting a very small amount of numbers may become too expensive for large inputs, while an algorithm that behaves very well for large datasets may be excessive or inefficient for small cases.

So the challenge is not only correctness.

It is also about:

- strategy
- adaptation
- move efficiency
- cost calculation
- choosing the right approach for the constraints of the project

To achieve the best score, I chose the **Mechanical Turk algorithm**, because in **push_swap** the goal is not necessarily to build the most theoretically powerful sorting algorithm in the world, but to design or select one that solves the problem **within the move limits required for the project**.

---

## Core Idea

In **push_swap**, we work with only:

- **stack A**
- **stack B**

The initial numbers start in **stack A**, and the mission is to end with:

- **stack A sorted in ascending order**
- **stack B completely empty**

If one element remains in stack B, even if stack A looks sorted, the result is still invalid.

That is one of the beautiful details of the project:
sorting is not just visual organization, but a full and correct final state.

The program does not print the sorted numbers directly.

Instead, it prints a sequence of operations such as:

- `sa`
- `pb`
- `pa`
- `ra`
- `rra`
- `rr`

Each printed instruction represents one move in the sorting process.

That means **push_swap** is really a program that outputs a **sorting plan**.

---

## Input Validation

Before sorting even begins, the project demands careful validation.

For me, this was the easiest part conceptually, but it is still essential because the algorithm is only meaningful if the input is valid.

The program must reject cases such as:

- values greater than `INT_MAX`
- values smaller than `INT_MIN`
- letters
- empty strings
- invalid numeric formats
- duplicated numbers

To make that safer, I used a custom **`ft_atoi_check`**, with support for a numeric range larger than the normal C `int` during parsing.

That way, if the number being read exceeds the valid integer range while it is still being parsed, I immediately stop the conversion and print:

```text
Error
````

to `stderr`.

The project must also behave correctly in simple edge cases:

* if no arguments are passed, it does nothing
* if the input is already sorted, it also does nothing

That silent behavior is important because unnecessary operations would be wrong in spirit and wasteful in practice.

---

## Algorithm Choice and Cost

One of the most subtle lessons in **push_swap** is understanding that sorting is not only about order.

It is about **cost**.

Every operation has a price.

For example:

* `pa` pushes to stack A
* `pb` pushes to stack B
* `ra` rotates A
* `rra` reverse rotates A

And there are combined operations such as:

* `rr`
* `rrr`

These are especially interesting because they allow both stacks to rotate together for the cost of a single instruction.

That means the project constantly pushes you to think in terms of:

* isolated moves
* combined moves
* local gains
* global gains

For me, the hardest part of the project was clearly the **cost calculation**.

The validations were more direct.

But calculating whether a number should be moved now, how much it costs to bring it to the top, where it should be inserted in the other stack, and whether combined rotations reduce the total cost — that was the most difficult and interesting part.

---

## Mechanical Turk Strategy

To solve the project, I used the **Mechanical Turk algorithm**.

What I liked about it is that it focuses on making **smart insertions** instead of blindly pushing numbers around.

The general reasoning is:

1. move part of stack A to stack B
2. keep evaluating which element in A is the **cheapest** to move next
3. calculate:

   * the cost to bring that element to the top of A
   * the cost to bring its target position to the top of B
4. combine rotations whenever possible
5. push the element
6. repeat until the structure is favorable for rebuilding the final sorted stack

The real strength of this approach is not brute force.

It is the fact that it tries to choose the **cheapest next action** every time.

That is why the algorithm feels much more strategic than a naive sorting routine.

A very important detail is that this type of algorithm may show some variation depending on the input arrangement.

Unlike approaches such as **Radix Sort**, which tend to keep a more predictable movement pattern across highly random inputs, the Mechanical Turk strategy demands more observation and analysis to verify both:

* correctness
* operation efficiency

That made the project more alive for me, because I had to test many different random combinations and observe whether the algorithm was still behaving properly.

---

## Sorting Examples

Below are **conceptual examples** of how the Mechanical Turk approach behaves.

The exact sequence can vary depending on implementation details, indexing strategy, and cost calculation rules, but the examples below capture the core idea of the method.

### Example 1 — Small input

Input:

```text
2 1 3
```

Possible reasoning:

* stack A starts as `[2, 1, 3]`
* only three numbers exist, so the cheapest solution is a tiny direct sort
* a simple swap can solve it

Possible output:

```text
sa
```

Result:

```text
1 2 3
```

This shows an important principle of **push_swap**:

for very small inputs, a specialized small-case strategy is often cheaper than using the full large-input algorithm.

---

### Example 2 — Moving values to B before rebuilding

Input:

```text
4 2 5 1 3
```

A possible high-level strategy:

1. push some elements to stack B
2. keep a reduced structure in A
3. sort the smaller remainder
4. reinsert elements from B into their correct positions with the lowest cost

Possible conceptual flow:

```text
pb
pb
sa
pa
pa
ra
```

This is not meant to represent the only valid exact output, but the reasoning pattern:

* first, reduce stack A
* then fix a small sortable core
* finally reinsert elements carefully

This is one of the foundations that makes the Mechanical Turk style effective.

---

### Example 3 — Cost-based insertion idea

Imagine:

```text
A = [8, 3, 6, 1, 9]
B = [7, 4, 2]
```

Now suppose we are evaluating which number from A should move next.

The algorithm may compute something like:

* moving `8` costs 4 moves total
* moving `3` costs 2 moves total
* moving `6` costs 5 moves total
* moving `1` costs 1 move total
* moving `9` costs 3 moves total

Then the cheapest candidate is:

```text
1
```

So instead of moving numbers in order of appearance, the algorithm chooses the one with the **lowest global cost**.

If both stacks need rotation in the same direction, the algorithm may prefer:

```text
rr
```

instead of:

```text
ra
rb
```

because one combined instruction is cheaper than two separate ones.

That is the heart of the cost model:
**not just moving, but moving with awareness of shared rotations and final insertion position**.

---

### Example 4 — Final rebuild into A

After several pushes and cost-based insertions, stack B gradually becomes a temporary holding structure.

Near the end, the algorithm starts restoring everything into A in sorted order.

Conceptually:

```text
A = [2, 3, 5]
B = [4, 1]
```

The algorithm evaluates where each value from B belongs in A:

* `4` belongs between `3` and `5`
* `1` belongs before `2`

So it rotates A until the target gap is aligned, then performs:

```text
pa
```

at the right moment.

This process continues until:

* all elements return to A
* the smallest element is positioned correctly
* stack B becomes empty

That final state is what defines success.

---

## Checker Bonus

The bonus part of the project is the **checker**.

This was an excellent extension because it transforms the project into a full validation system.

The checker reads the operations produced by `push_swap`, applies them to the stacks, and verifies whether the final result is valid.

That means it checks whether:

* stack A is sorted
* stack B is empty

This bonus reinforces an important mindset:

it is not enough to generate operations.

You also need a way to verify that those operations truly solved the problem.

For me, that makes the project more complete and more professional.

---

## How To Use

To compile the project, run:

```bash
make
```

This generates:

* `push_swap`
* `checker`

### Run push_swap

```bash
./push_swap 2 1 3
```

Example output:

```text
sa
```

### Validate with checker

```bash
./push_swap 4 2 5 1 3 | ./checker 4 2 5 1 3
```

If everything is correct, the checker should confirm that the operations sorted the input successfully.

---

## Team

**push_swap** is an individual project at **École 42**.

So this project was entirely developed by me.

---

## Final Note

For me, **push_swap** was much more than a sorting project.

It was an introduction to:

* algorithmic trade-offs
* optimization under constraints
* stack manipulation
* operation cost analysis
* careful validation
* strategic decision-making

It taught me that a correct algorithm is not always enough.

Sometimes the real challenge is finding one that is correct **and** efficient **within a limited operational budget**.

That is exactly what makes **push_swap** so interesting.

It is not just about sorting numbers.

It is about learning how to think like someone who must solve a problem with very few tools, under strict constraints, and still produce an elegant result.

**Final result:** `push_swap` + `checker` (bonus included) — **125/125**
**Status:** Completed
