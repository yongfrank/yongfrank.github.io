---
categories: study
date: "2022-10-02T16:37:00Z"
title: 'Swift: Math Algorithm'
---

<!-- markdownlint-disable -->
<!--
 * @Author: Frank Chu
 * @Date: 2022-10-02 10:02:18
 * @LastEditors: Frank Chu
 * @LastEditTime: 2022-10-04 11:30:10
 * @FilePath: /blog/_posts/2022-10-02-math-algorithm.md
 * @Description: 
 * 
 * Copyright (c) 2022 by Frank Chu, All Rights Reserved. 
-->

## Prime Number

A prime number is a whole number greater than 1 whose only factors are 1 and itself.

2, 3, 5, 7, 11, 13, 17, 19, 23, 29 are prime numbers.

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/f0/Primes-vs-composites.svg/440px-Primes-vs-composites.svg.png" alt="Primes vs composites" height="200"/>

### [Fastest Prime Checker by Noah Wilder on the stack overflow](https://stackoverflow.com/questions/31105664/check-if-a-number-is-prime)

> Swift 4.2, Xcode 10.1
>
> This prime checking function and extension is the most efficient as it checks the divisibility of only $\frac{1}{2}\sqrt{n}$ integers.
>
> Complexity: $O(\frac{1}{2} \sqrt{n})$

```swift
///  https://stackoverflow.com/questions/31105664/check-if-a-number-is-prime
///  Check if a number is prime?
import Foundation
func isPrime(for number: Int) -> Bool {
    guard number >= 2 else { return false }
    guard number != 2 else { return true }
    guard number % 2 != 0 else { return false }
    
    /// `stride(from: 3, to: Int(sqrt(Double(number))), by: 2)`
    /// traverse 3 5 7 9 11.... odd number
    ///
    /// `stride(from:to:by:).contains(where:)`
    /// If number % index = 1, then it's not prime.
    /// If the odd number contains number that the specific number can be divided by. Then it's not prime.
    /// So there is a negation operator before stride.
    /// True -> Prime False -> Not Prime
    return !stride(from: 3, to: Int(sqrt(Double(number))), by: 2).contains(where: { index in
        return number % index == 0
    })
}

print(isPrime(for: 4))
```
### Extension
```swift
extension Int {
    ///  A prime number is a whole number greater than 1 whose only factors are 1 and itself.
    var isPrime: Bool {
        guard self >= 2 else { return false }
        guard self != 2 else { return true }
        guard self % 2 != 0 else { return false }
        
        return !stride(from: 3, to: Int(sqrt(Double(self))), by: 2).contains(where: { index in
            return self % index == 0
        })
    }
}

var number = 3
number.isPrime
```

### Get odd number in C

> * Store the size of the array to be returned in the `result_count` variable
> * Allocate the array statically or dynamically.
> 
> [C get odd numbers between a range and store it in int array](https://stackoverflow.com/questions/52989394/c-get-odd-numbers-between-a-range-and-store-it-in-int-array) - by user10543939 on the stack overflow

```c
int* oddNumbers(int start, int end, int* result_count) {
    int *result;
    int i;
    
    /// @brief #define	assert(e) \
    /// (__builtin_expect(!(e), 0) ? __assert_rtn(__func__, __ASSERT_FILE_NAME, __LINE__, #e) : (void)0)
    /// @param start 
    /// @param end 
    /// @param result_count 
    /// @return 
    assert(result_count && start <= end && INT8_MIN < end);

    // if start is even, then add 1 to be odd
    start += !(start % 2);
    // if end is even, then minus 1 to be odd
    end -= !(end % 2);

    // calculate array size
    *result_count = (end - start) / 2 + 1;
    result = malloc(*result_count * sizeof(result));
    
    if(!result) return NULL;

    // fill in numbers in the result
    for (i = 0; start <= end; i++, start += 2) result[i] = start;

    return result;
}

int main() {
    int min = 30;
    int max = 85;
    // address of count
    int count;
    int *numbers = oddNumbers(min, max, &count);
    
    for (int i = 0; i < count; i++) {
        printf("%d ", numbers[i]);
    }
    // Don't forget to `free()` the returned pointer when you are done using it.
    free(numbers);
}
```

### Stride and generics template in C++

```c++
template<typename T>
T * stride(T start, T end, T step) {
    T * arrayOfOutPut;

    int sizeOfArray = (end - start) / step + 1;
    arrayOfOutPut = (T *)malloc(sizeOfArray * sizeof(T));

    for(int i = 0; i < sizeOfArray; i++) {
        arrayOfOutPut[i] = start + i * step;
        std::cout << arrayOfOutPut[i] << " ";
    }

    return arrayOfOutPut;
}

int main() {
    double start = 2;
    double end = 5;
    double step = 0.5;

    int sizeOfArray = (end - start) / step + 1;
    double* a = stride(start, end, step);
    std::cout << std::endl;
    for(int i = 0; i < sizeOfArray; i++) {
        std::cout << a[i] << " ";
    }
}
```

### valarray

```c++
#include <iostream>
#include <valarray>

bool isPrime(int number) {
    if (!(number > 2)) return false;
    if (!(number != 2)) return true;
    if (!(number % 2 != 0)) return false;

    int end = number;
    int start = 3;
    int step = 2;
    int sizeOfArray = sizeOfArray = (end - start) / step + 1;
    
    std::valarray<int> array(sizeOfArray);
    for(int i = 0; i < sizeOfArray; i++) {
        array[i] = i;
    }

    std::slice slcOfArray(start, end, step);

    std::valarray<int> valArrayOutput = std::valarray<int>(array[slcOfArray]);
    
    for(int i = 0; i < sizeOfArray; i++) {
        if (number % valArrayOutput[i] == 0) return false;
    }
    return true;
}

int main() {
    int numberCount = 0;

    int startNumber = 200;
    int endNumber = 299;

    for(int i = startNumber; i <= endNumber; i++) {
        if(isPrime(i)) {
            numberCount += 1;
            std::cout << i;
            if(numberCount == 8) {
                std::cout << std::endl;
                numberCount = 0;
            } else {
                std::cout << " ";
            }
        }
    }
}
```

### Sieve of Eratosthenes

[Sieve of Eratosthenes - GeeksForGeeks](https://www.geeksforgeeks.org/sieve-of-eratosthenes/)

[Khan Academy - Sieve of Eratosthenes](https://www.khanacademy.org/computing/computer-science/cryptography/comp-number-theory/v/sieve-of-eratosthenes-prime-adventure-part-4)

```pseudo
For all numbers a: from 2 to sqrt(n)
    IF a is unmarked THEN
        a is prime
        for all multiples of a (a < n)
            mark multiples as composite

All unmarked numbers are prime!
```

```c++
#include <stdio.h>

void SieveOfEratosthenes(int n) {
    
    bool prime[n + 1];
}

```