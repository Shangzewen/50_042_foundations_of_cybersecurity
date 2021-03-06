# 1. Part 1: Algebraic Structures

## 1.1 Polynomial 2 class

### 1.1.1 add and sub

- An important thing to note is that the array may be of different size. There are 2 options for dealing with this
- Hence, make the 2 arrays the same size by padding the shorter one with zeroes with an `append`

```python
if len_p1 > len_p2:
    for zero_counter in range(difference_length):
        p2._coeffs.append(0)
else:
    for zero_counter in range(difference_length):
        self._coeffs.append(0)
```

From this, we can then do the XOR

```python
for index, indiv_bit in enumerate(self._coeffs):
    add_result.append(p2._coeffs[index] ^ indiv_bit)
```

### 1.1.2 Multiplication

- For this part we will use the test case to illustrate
  - p1 = x<sup>5</sup>+x<sup>2</sup>+x  
  - p4 = x<sup>7</sup>+x<sup>4</sup>+x<sup>3</sup>+x<sup>2</sup>+x  
  - modp = x<sup>8</sup>+x<sup>7</sup>+x<sup>5</sup>+x<sup>4</sup>+1

```python
print ('p1=x^5+x^2+x')  # self
print ('p4=x^7+x^4+x^3+x^2+x')
print ('modp=x^8+x^7+x^5+x^4+1')
p1=Polynomial2([0,1,1,0,0,1])
p4=Polynomial2([0,1,1,1,1,0,0,1])
modp=Polynomial2([1,0,0,0,1,1,0,1,1]) # This is different from the one provided in the notes 
p5=p1.mul(p4,modp)
```

| Row  | Powers                        | Operation                                                    | New Result                                                   | Reduction | After reduction (XOR)                                        |
| ---- | ----------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | --------- | ------------------------------------------------------------ |
| 1    | x<sup>0</sup> . P<sub>4</sub> |                                                              | x<sup>7</sup>+x<sup>4</sup>+x<sup>3</sup>+x<sup>2</sup>+x    | N         |                                                              |
| 2    | x<sup>1</sup> . P<sub>4</sub> | x . x<sup>7</sup>+x<sup>4</sup>+x<sup>3</sup>+x<sup>2</sup>+x | x<sup>8</sup>+x<sup>5</sup>+x<sup>4</sup>+x<sup>3</sup>+x<sup>2</sup> | Y         | x<sup>7</sup>+x<sup>3</sup>+x<sup>2</sup>+1                  |
| 3    | x<sup>2</sup> . P<sub>4</sub> | x . x<sup>7</sup>+x<sup>3</sup>+x<sup>2</sup>+1              | x<sup>8</sup>+x<sup>4</sup>+x<sup>3</sup>+x                  | Y         | x<sup>7</sup>+x<sup>5</sup>+x<sup>3</sup>+x+1                |
| 4    | x<sup>3</sup> . P<sub>4</sub> | x . x<sup>7</sup>+x<sup>5</sup>+x<sup>3</sup>+x+1            | x<sup>8</sup>+x<sup>6</sup>+x<sup>4</sup>+x<sup>2</sup>+x    | Y         | x<sup>7</sup>+x<sup>6</sup>+x<sup>5</sup>+x<sup>2</sup>+x+1  |
| 5    | x<sup>4</sup> . P<sub>4</sub> | x . x<sup>7</sup>+x<sup>6</sup>+x<sup>5</sup>+x<sup>2</sup>+x+1 | x<sup>8</sup>+x<sup>7</sup>+x<sup>6</sup>+x<sup>3</sup>+x<sup>2</sup>+x | Y         | x<sup>6</sup>+x<sup>5</sup>+x<sup>4</sup>+x<sup>3</sup>+x<sup>2</sup>+x+1 |
| 6    | x<sup>5</sup> . P<sub>4</sub> | x . x<sup>6</sup>+x<sup>5</sup>+x<sup>4</sup>+x<sup>3</sup>+x<sup>2</sup>+x+1 | x<sup>7</sup>+x<sup>6</sup>+x<sup>5</sup>+x<sup>4</sup>+x<sup>3</sup>+x<sup>2</sup>+x | N         |                                                              |

We then take the `After reduction` results associated with row 2, 3, 6

Result = (x<sup>7</sup>+x<sup>3</sup>+x<sup>2</sup>+1) + (x<sup>7</sup>+x<sup>5</sup>+x<sup>3</sup>+x+1) + (x<sup>7</sup>+x<sup>6</sup>+x<sup>5</sup>+x<sup>4</sup>+x<sup>3</sup>+x<sup>2</sup>+x) = x<sup>7</sup>+x<sup>6</sup>+x<sup>4</sup>+x<sup>3</sup>

<u>**Guide to doing the addition**</u>

Doing the first addition

|                                               | x<sup>0</sup> | x<sup>1</sup> | x<sup>2</sup> | x<sup>3</sup> | x<sup>4</sup> | x<sup>5</sup> | x<sup>6</sup> | x<sup>7</sup> |
| --------------------------------------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| x<sup>7</sup>+x<sup>3</sup>+x<sup>2</sup>+1   | 1             |               | 1             | 1             |               |               |               | 1             |
| x<sup>7</sup>+x<sup>5</sup>+x<sup>3</sup>+x+1 | 1             | 1             |               | 1             |               | 1             |               | 1             |
| Result                                        | 0             | 1             | 1             | 0             | 0             | 1             | 0             | 0             |

Doing the second addition

|                                                              | x<sup>0</sup> | x<sup>1</sup> | x<sup>2</sup> | x<sup>3</sup> | x<sup>4</sup> | x<sup>5</sup> | x<sup>6</sup> | x<sup>7</sup> |
| ------------------------------------------------------------ | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| x<sup>5</sup>+x<sup>2</sup>+x                                |               | 1             | 1             |               |               | 1             |               |               |
| x<sup>7</sup>+x<sup>6</sup>+x<sup>5</sup>+x<sup>4</sup>+x<sup>3</sup>+x<sup>2</sup>+x |               | 1             | 1             | 1             | 1             | 1             | 1             | 1             |
| Result                                                       | 0             | 0             | 0             | 1             | 1             | 0             | 1             | 1             |

The result is x<sup>7</sup>+x<sup>6</sup>+x<sup>4</sup>+x<sup>3</sup>

### 1.1.3 Division

Essentially this one requires us to follow the formula as shown in the notes. No explanation required.

## 1.2 Galois field class

### 1.2.1 Multiplication

<u>**Code's test case**</u>

<u>**VERSION 1**</u>

For this part we will use the test case to illustrate

- g4 = x<sup>3</sup>+x<sup>2</sup>+1  
- g5 = x<sup>2</sup>+x
- modp = x<sup>4</sup>+x+1

```python
ip=Polynomial2([1,1,0,0,1])
print ('irreducible polynomial {}'.format(ip))
g4=GF2N(0b1101,4,ip)  # 13 = [1, 0, 1, 1]
g5=GF2N(0b110,4,ip)  # 6 = [0, 1, 1]
print ('g4 = {}'.format(g4.getPolynomial2()))  # x^3+x^2+x^0
print ('g5 = {}'.format(g5.getPolynomial2()))  # x^2+x^1
g6=g4.mul(g5)
print ('g4 x g5 = {}'.format(g6.p))
```

| Row  | Powers                        | Operation                         | New Result                    | Reduction | After reduction (XOR) |
| ---- | ----------------------------- | --------------------------------- | ----------------------------- | --------- | --------------------- |
| 1    | x<sup>0</sup> . g<sub>4</sub> |                                   | x<sup>3</sup>+x<sup>2</sup>+1 | N         |                       |
| 2    | x<sup>1</sup> . g<sub>4</sub> | x . x<sup>3</sup>+x<sup>2</sup>+1 | x<sup>4</sup>+x<sup>3</sup>+x | Y         | x<sup>3</sup> + 1     |
| 3    | x<sup>2</sup> . g<sub>4</sub> | x . x<sup>3</sup>+1               | x<sup>4</sup>+x               | Y         | 1                     |

We then take the `After reduction` results associated with row 2, 3

Result = (x<sup>3</sup> + 1) + (1)  = x<sup>3</sup>

<u>**VERSION 2: Swap g4 and g5**</u>

For this part we will use the test case to illustrate

- g4 = x<sup>3</sup>+x<sup>2</sup>+1  
- g5 = x<sup>2</sup>+x
- modp = x<sup>4</sup>+x+1

```python
ip=Polynomial2([1,1,0,0,1])
print ('irreducible polynomial {}'.format(ip))
g4=GF2N(0b1101,4,ip)
g5=GF2N(0b110,4,ip)
print ('g4 = {}'.format(g4.getPolynomial2()))
print ('g5 = {}'.format(g5.getPolynomial2()))
g6=g5.mul(g4)  # Changed this part
print ('g4 x g5 = {}'.format(g6.p))
```

| Row  | Powers                        | Operation                       | New Result                    | Reduction | After reduction (XOR) |
| ---- | ----------------------------- | ------------------------------- | ----------------------------- | --------- | --------------------- |
| 1    | x<sup>0</sup> . g<sub>5</sub> |                                 | x<sup>2</sup>+x               | N         |                       |
| 2    | x<sup>1</sup> . g<sub>5</sub> | x . x<sup>2</sup>+x             | x<sup>3</sup>+x<sup>2</sup>   | N         |                       |
| 3    | x<sup>2</sup> . g<sub>5</sub> | x . x<sup>3</sup>+x<sup>2</sup> | x<sup>4</sup>+x<sup>3</sup>   | Y         | x<sup>3</sup>+x+1     |
| 4    | x<sup>3</sup> . g<sub>5</sub> | x. x<sup>3</sup>+x+1            | x<sup>4</sup>+x<sup>2</sup>+x | N         | x<sup>2</sup>+1       |

We then take the `After reduction` results associated with row 1, 3, 4

Result = (x<sup>2</sup>+x) + ( x<sup>3</sup>+x+1) + (x<sup>2</sup>+1) = x<sup>3</sup>

<u>**Submission test case**</u>

Create a table for addition and multiplication for GF(2<sup>4</sup>), using (x<sup>4</sup>+x<sup>3</sup>+1) as the modulus.

For this test case we will use 

- g4 = x<sup>3</sup>+x<sup>2</sup>+1  
- g5 = x<sup>2</sup>+x

| Row  | Powers                        | Operation                         | New Result                    | Reduction | After reduction (XOR) |
| ---- | ----------------------------- | --------------------------------- | ----------------------------- | --------- | --------------------- |
| 1    | x<sup>0</sup> . g<sub>4</sub> |                                   | x<sup>3</sup>+x<sup>2</sup>+1 | N         |                       |
| 2    | x<sup>1</sup> . g<sub>4</sub> | x . x<sup>3</sup>+x<sup>2</sup>+1 | x<sup>4</sup>+x<sup>3</sup>+x | Y         | x + 1                 |
| 3    | x<sup>2</sup> . g<sub>4</sub> | x . x+1                           | x<sup>2</sup>+x               | N         |                       |

We then take the `After reduction` results associated with row 2, 3

Result = (x<sup>2</sup> + x) + (x+1)  = x<sup>2</sup>+1

Addition table

|                   | x<sup>0</sup> | x<sup>1</sup> | x<sup>2</sup> | x<sup>3</sup> |
| ----------------- | ------------- | ------------- | ------------- | ------------- |
| x<sup>2</sup> + x | 0             | 1             | 1             | 0             |
| x+1               | 1             | 1             | 0             | 0             |
| Result            | 1             | 0             | 1             | 0             |

### 1.2.2 Division

The only problem I faced was an infinite loop because the subtraction and multiplication methods were setting the new object created to the default `n=8` and `ip=Polynomial2([1,1,0,1,1,0,0,0,1])` values. 

To overcome this, when returning an object from the add, subtract, and multiplication methods, I had to set the `n` and `ip` values to the parents' values 

```python
GF2N(value, self.n, self.ip)
```

# Misc: Bug fixes

## 1. Using the class' `__str__()` method

I could not get it to work initially because I was returning the result as `return add_result` 

Instead, I need to return an Object which will be captured by the test case and then printed. Previously I returned an array and when we print it, it just printed an array.

Line 9 is the issue and we change it to the commented code 

To format the way that the lab wants, it is convenient to reverse the list. However, we have to `deepcopy` it to prevent errors.

```python
import copy

class Polynomial2:
    def __init__(self,coeffs):
        self._coeffs = coeffs
        
    def add(self,p2):
        # Some logic here
        return add_result # Polynomial2(add_result) 
    
    def __str__(self):
        formatted_polynomial = ''
        temporary_coeffs = copy.deepcopy(self._coeffs)
        temporary_coeffs.reverse()
        for index_coeff, indiv_coeff in enumerate(temporary_coeffs):
            if index_coeff == len(temporary_coeffs) - 1:
                if indiv_coeff == 1:
                    formatted_polynomial += 'x^{}'.format(0)
                else:
                    formatted_polynomial = formatted_polynomial[: -1]
            else:
                if indiv_coeff == 1:
                    formatted_polynomial += 'x^{}+'.format(len(temporary_coeffs) - 1 - index_coeff)
        return formatted_polynomial

p1=Polynomial2([0,1,1,0,0,1])
p2=Polynomial2([1,0,1,1])
p3=p1.add(p2)  # OVER HERE WE SHOULD BE RETURNING AN OBJECT OF THE CLASS
print ('p3= p1+p2 = {}'.format(p3))  # WHEN WE PRINT THE OBJECT IT WILL PRINT THE STRING RETURNED FROM THE __str()__ METHOD
```

