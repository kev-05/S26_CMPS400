Problem 2: 

Q 1)
for (int i = 0; i < 15; i++) {
    sum += i;
}

Time Complexity: O(1)

The loop runs exactly 15 times regardless of any input size. There is no variable n driving the number 
of iterations. It is a hard coded constant.

Q 2)
for (int i = 0; i < n; i++) {
    sum += a[i];
}

Time complexity: O(n)

The loop executes n times, once for each element in the array. The body performs a constant amount of 
work per iteration. Total work grows linearly with n, so the complexity is O(n).

Q 3)
for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        count++;
    }
}

Time Complexity: O(n^2)

The outer loop runs n times for every one of those iterations the inner loop also runs n times. 
The body executes n*n=n^2 times in tidal. Because both loops are bounded by n, the overall complexity
is quadratic, O(n^2)

Q 4)
int i = 1;
while (i < n) {
    i *= 2;
}

Time complexity: O(log n)

Starting at 1, the variable i is doubled each iteration: 1 - 2 - 4 - 8 -... after k iterations, i = 2^k.
The loop stops when 2^k ≥ n. So k = [log2n]. The number of iterations grows logarithmically with n,
giving O(log n)

Q 5) 
int i = 2;
while (i <= n) {
    i = i * i;
}

Time Complexity O(log log n)

Starting at 2, it is squared each iteration : 2 - 4 - 16 - 256 - … After k iterations, i = 2^(2^k). 
The loop stops when 2^(2^k) > n, when 2^k > log2 n, when k > log2(log2 n). The number of iterations is 
therefore O(log log n)

Problem 3: 

Q1) Why is asymptotic analysis usually more useful than timing two programs on a single computer? 

Timing depends on the machine, compiler, and what else is running, none of that has anything to do with 
the algorithm itself. Asymptotic analysis focuses on how the algorithm scales as input grows, which holds
true on any machine. An O(n log n) algorithm will always beat an O(n^2) one for large enough inputs, 
no matter where it runs.

Q2) Why does Binary Search require sorted data? 

Binary Search cuts the search space in half by deciding whether to go left or right based on the middle 
element. That only works if the data is in order if it’s not sorted, eliminating half the array could 
throw away the element you’re looking for.

Q3) For a nearly sorted array, which algorithm is usually a better choice: Selection Sort or Insertion Sort? 

Insertion Sort. When the array is nearly sorted, most elements are already close to where they belong, 
so very few shifts are needed. In the best case it runs in O(n). Selection Sort always does O(n^2) 
comparisons no matter how sorted the data already is it never takes advantage of existing order

Q4) If memory writes are expensive, which algorithm is usually more 
attractive: Selection Sort or Insertion Sort? 

Selection Sort. It makes at most n-1 swaps total, one per pass keeping writes to a minimum. 
Insertion Sort can shift elements O(n^2) times in the worst case. When writes are costly, fewer is better. 
