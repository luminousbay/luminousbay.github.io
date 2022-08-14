---
layout: post
title: Digression on Equality
published: true
---
_Python has two versions of equality; they are == (plain equality) and is identity. Actual is identity, as in ‘a is b‘, tests whether the two things are in fact the exact same object.== can compare different types of objects and return useful results if desired. a == b checks if a and b have same value. a is b checks if a and b refer to same object._
{: .text-justify}

_What happen when we assign names_


![rpi](https://lh3.googleusercontent.com/q1ab3T3vASE5AUpIoU2Uoiyze_b0uTWxoL21EZvLScHkisy_jmltYYOYgpTqFQodNgk=w2400)
<!--more-->


```python
x = 7
 
y = x
 
x = 5
```


Has the value of y changed? The answer is NO, why should it? Values of type **int, float, bool, str** are immutable. For immutable values, we can assume that assignment makes a fresh copy of a value. Updating one value does not affect the copy.
{: .text-justify}

Consider the below case:

```python
one = [1,2,3,4,5]
 
two = one
 
one[2] = 0
```

This will result in the variables one and two to contain the same newly edited list.


![pic1](https://lh5.googleusercontent.com/JTM8GFS8pSyPr1keHvexgRx2PQS59xjxx12EQUhCWtZA5cE1NG02s0TzMb-jr_BGITQ=w2400)


On editing any one list in this case list one the other list two also changed because both of them points the same list [1,2,3,4,5]. one and two are two names for the same list.
{: .text-justify}


![list](https://lh4.googleusercontent.com/A2xcI_EdL3qkEk7si5HzE1xFjbZsJAHynYVNiwTLuGtLsr-R8zCwfrYjjZhcGznIAvs=w2400)


For mutable values, assignment does not make a fresh copy. One other instant a correction can be applied by generating a copy of list while assigning.
{: .text-justify}


```python
one = [1,2,3,4,5]
 
two = one[::]
 
one[2] = 0
```

This will result in the variables one and two to contain two different list.


![list_1](https://lh5.googleusercontent.com/fp2zcqioMAB-ItAk5ugMQltRyHGIFfSA7z2LEuoyWpiD_yhiQtavcsOiV5ApYXx4mXk=w2400)


Here in this case on editing list one doesn’t alter the elements of list two. The variable list two is assigned a full slice of list one. A slice creates a new (sub)list from an old one. Both the variable points two different copy of lists but of same value.
{: .text-justify}


![list_2](https://lh4.googleusercontent.com/l-atTLsW84zax0Qnmvvz3AE5VaisJJ010NIlN7cZToJVxfGiPNer1BTyXGuPdJ9i_jw=w2400)


Now we can easily understand what’s the digression on equality:


```python
one = [1,2,3,4,5]
 
two = [1,2,3,4,5]
 
three = two
```


Here,

_one == two_  is  **True**
_two == three_  is  **True**

and

_two is three_  is  **True**
_one is two_  is  **False**

_one is two_ is _false_ because list one and two are not the same object but two different object of same value and type.
{: .text-justify}

