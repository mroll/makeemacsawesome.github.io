---
layout: default
---

# Emacs as a Calculator

You know when you’re in the middle of some task — maybe planning your budget, figuring out how much you have to save for those nice shoes, or deciding how many books you can read this year — and you just need to do some quick math?

What do you reach for? You could grab your phone, there’s a handy calculator app on there. You could switch over to Google, which does simple math pretty well. Or you could whip out Emacs and run the numbers using the built-in `calc` program.

Start the Calc program by running `M-x calc`. You’ll see a buffer pop up from the bottom of your Emacs window that looks like this.

![Screen Shot 2023-01-14 at 5.38.31 PM.png](Emacs%20as%20a%20Calculator%20b7d69978b2c941f1941edc0f61fe4832/Screen_Shot_2023-01-14_at_5.38.31_PM.png)

This is the “calc buffer.” It has two side-by-side panes. The left is a “stack” where we put numbers waiting to be operated on, and the right shows you a history of everything that has been evaluated.

# Reverse Polish Notation

The Emacs calculator by default uses a syntax called [Reverse Polish Notation](https://en.wikipedia.org/wiki/Reverse_Polish_notation) (RPN). This means you type the operators (+, -, /, *) *****after***** you type the numbers. This is in contrast to [Infix Notation](https://en.wikipedia.org/wiki/Infix_notation), which is the more common way of putting operators between the numbers they act on. I don’t know the historical reasons RPN was chosen over infix, but my guess is that it was because there is no need for parentheses in RPN. It’s an interesting sort of elegance, if not super practical for those of us used to using infix notation.

Let’s try out an RPN expression, and then we will see how to tell Emacs calc to use the more comfortable infix style.

Suppose we are looking into a company, Emacs Inc., to determine whether their stock is worth investing in. One of the things we want to know is how much money they make every year per share of stock. Well, let’s say we know their average earnings is $1,000,000. And the number of shares outstanding is 346,998.

To get earnings per share we want to divide earnings by the number of shares. So first we type into Emacs calc the earnings of 1,000,000. Then we hit space to enter that value onto the stack. Next we type in the number of shares, 346,998. And we hit space again to enter ****that**** value onto the stack. To carry out the division, we simply hit `/`. The stack is transformed to show us our answer of `2.88`.

![Screen Shot 2023-01-14 at 5.54.32 PM.png](Emacs%20as%20a%20Calculator%20b7d69978b2c941f1941edc0f61fe4832/Screen_Shot_2023-01-14_at_5.54.32_PM.png)

# Infix Style

Ok, that’s all great but in order to do math quick you probably want to be able to use the more comfortable infix style. Emacs calc can handle that, you just need to tell it that’s what you want to do.

In order to use infix style (also known as “algebraic style”), begin your expression with a single apostrophe, `'`. Then type out your whole expression. So if we wanted to evaluate the earnings per share example from above, but in infix style, we’d write this in the calc buffer: `'1000000 / 346998`.

Hitting the apostrophe will immediately turn the input into an `Algebraic:` prompt, which looks like this.

![Screen Shot 2023-01-14 at 5.58.19 PM.png](Emacs%20as%20a%20Calculator%20b7d69978b2c941f1941edc0f61fe4832/Screen_Shot_2023-01-14_at_5.58.19_PM.png)

To evaluate the expression, just hit `Enter` when you’re done.

## Quick-Calc

There is an even faster way to do simple math in algebraic style, and that is the `quick-calc` command. Quick-calc understands infix notation by default, and is useful for running a single calculation at a time.

To use quick-calc, run `M-x quick-calc`. You’ll be greeted with a **********************Quick calc:********************** prompt in the minibuffer where you can type in your expression.

![Screen Shot 2023-01-15 at 12.09.31 PM.png](Emacs%20as%20a%20Calculator%20b7d69978b2c941f1941edc0f61fe4832/Screen_Shot_2023-01-15_at_12.09.31_PM.png)

Let’s evaluate the earnings per share expression using quick-calc to see what that would look like. Just type in `1000000 / 346998` and hit `Enter`.

The result is printed directly to the minibuffer.

![Screen Shot 2023-01-15 at 12.10.54 PM.png](Emacs%20as%20a%20Calculator%20b7d69978b2c941f1941edc0f61fe4832/Screen_Shot_2023-01-15_at_12.10.54_PM.png)

# Calculator Superpowers

Emacs calc is no ordinary desktop calculator. It comes loaded with support for manipulating [complex numbers](https://www.gnu.org/software/emacs/manual/html_node/calc/Complex-Numbers.html), [financial functions](https://www.gnu.org/software/emacs/manual/html_node/calc/Financial-Functions.html), [vectors and matrices](https://www.gnu.org/software/emacs/manual/html_node/calc/Vectors-and-Matrices.html), [algebraic expressions](https://www.gnu.org/software/emacs/manual/html_node/calc/Algebraic-Manipulation.html), [differentials](https://www.gnu.org/software/emacs/manual/html_node/calc/Differentiation.html), and [units of measurement](https://www.gnu.org/software/emacs/manual/html_node/calc/Units.html), among other things. In this section we’ll go over one cool example of what Emacs calc can do, and you can find a full reference of what it is capable of [here](https://www.gnu.org/software/emacs/manual/html_mono/calc.html#Units).

## Financial Functions

So you just landed a great new job, and as a result you have some more money left over from your paycheck at the end of each month. It occurs to you that it might be a good idea to invest it in order to save up for a down payment on a house. First of all, congratulations! That’s very exciting 🙂

It turns out Emacs can help us plan the investment. Let’s say you’ll have an extra $500 at the end of every month that can be invested. And you’ve found a high-yield savings account with a 3.5% annual percentage yield (this is the rate of return, per year).

To figure out how much money you’ll have by following this investment schedule for five years, we will use the [Future Value](https://www.gnu.org/software/emacs/manual/html_node/calc/Future-Value.html) function. This function takes three arguments:

- *Rate*
- *N*
- *Payment*

****Rate**** is the percent interest earned for each interval. *N* is how many intervals we want to run the calculation for. And *******Payment******* is how much money will be invested per interval.

The way Emacs calc functions work is you enter the arguments in order on the stack, and then run the function. The function knows how to pick the arguments off the stack, and it will use them to compute a result, and then it will return that result to the stack.

For this calculation, we first need to transform the annual interest rate into a monthly interest rate. Enter `3.3` onto the stack and then type `M-%` to turn this decimal value into a percentage value. Then enter `12` followed by `/`. This will give us our monthly interest rate of `2.75e-3`.

We now have our *****Rate***** argument. The other two arguments are easier, just enter `60` (12 months times 5 years), followed by `500`, our monthly payment.

Our stack should look like this.

![Screen Shot 2023-01-15 at 12.58.41 PM.png](Emacs%20as%20a%20Calculator%20b7d69978b2c941f1941edc0f61fe4832/Screen_Shot_2023-01-15_at_12.58.41_PM.png)

Now type the sequence `b F`. This executes the `calc-fin-fv` function, which calculates the future value of this investment using our values from the stack.

Emacs is telling us this investment plan will result in $32,000 over the course of five years. A nice chunk of change! You could of course experiment with other values. Maybe you could find ways to increase the amount you save every month, or perhaps the interest rate environment will change and you’ll want to model what the investment would look like in that case.

![Screen Shot 2023-01-15 at 12.59.50 PM.png](Emacs%20as%20a%20Calculator%20b7d69978b2c941f1941edc0f61fe4832/Screen_Shot_2023-01-15_at_12.59.50_PM.png)
