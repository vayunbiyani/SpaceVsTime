# SpaceVsTime
Just a basic project for school

In programming, just as in other aspects of life, we have many ways of solving problems. Even though there are countless approaches to an issue that produce the same outcomes, some are more effective than others at solving it.

Our project shows how using an iterative approach compares to a recursive approach when it comes to finding the factorial of a number and take a peek into deciding whether to prioritise space or time.

We can start off by writing a simple snippet that calculates the factorial iteratively which initialises a variable for the `result` to 1 and multiplies it with every natural number upto the parameter:

```python
def factorialIterative(n):
    result = 1
    for i in range(1, n+1):
        result = result * i
    return result
```

Followed by one that does the same recursively which calls the function for a smaller number until it reaches 1 :

```python
def factorialRecursive(n):
   if n == 1:
       return n
   else:
       return n*factorialRecursive(n-1)
```

Lastly we can use a simple callback function that check the runtime for a given function with a given input:

```python
import time

def findRunTime(functionToCheck,parameter):
    startingTime = time.time()
    functionToCheck(parameter)
    endingTime = time.time()
    return endingTime-startingTime
```

Note that this program is just intended to display the run time of different algorithms which is why it does not have proper error handling for undesired inputs such as negative numbers etc.

On running a few tests, we notice that the recursive approach is mostly is faster upto 14 after which iterative takes over. Now the question is by how much? 🤔(maybe I’m wrong?)

Let’s now make thinks interesting by adding a function to find the relative difference. 🔥

```python
import time

def factorialIterative(n):
    result = 1
    for i in range(1, n+1):
        result = result * i
    return result

def factorialRecursive(n):
   if n == 1:
       return n
   else:
       return n*factorialRecursive(n-1)

def findRunTime(functionToCheck,parameter):
    startingTime = time.time()
    functionToCheck(parameter)
    endingTime = time.time()
    return endingTime-startingTime

def compareRuntime(parameter):
    timeIterative = findRunTime(factorialIterative,parameter)
    timeRecursive = findRunTime(factorialRecursive,parameter)
    if timeIterative<timeRecursive:
        print("iterative is faster by",timeRecursive*100/timeIterative,"percentage")
    elif timeIterative>timeRecursive:
        print("recursive is faster",timeIterative*100/timeRecursive,"percentage")
    else:
        print("They are equal :0")
```

Now as you may have guessed(or found out on running😂) that calling this once just take a very small sample size.

Let’s maybe ditch this and write something that could actually collect some data with a good sample size 😍

Here’s a crappy function that finds the avg runtime on large sample sizes.

```python
def runtimeData(parameter,sampleSize=10000):#returns an array with first index being the avg runtime for iterative likewise the second for iterative
    sumIterative = 0
    sumRecursive = 0
    for i in range(sampleSize):
        sumIterative+=findRunTime(factorialIterative,parameter)
        sumRecursive+=findRunTime(factorialRecursive,parameter)
    return [sumIterative/sampleSize,sumRecursive/sampleSize]

def displayData(parameter):
    data = runtimeData(parameter)
    if data[0]<data[1]:
        print("Iterative is faster by",data[1]*100/data[0],"percentage")
    elif data[0]>data[1]:
        print("Recursive is faster",data[0]*100/data[1],"percentage")
```

Wow so playing around with this made me realise that recursive is faster only until 3 😳 (I was wrong earlier)

After which ofc iterative FTW!!

- Expand the toggle for the final code
    
    ```python
    import time
    
    def factorialIterative(n):
        result = 1
        for i in range(1, n+1):
            result = result * i
        return result
    
    def factorialRecursive(n):
       if n == 1:
           return n
       else:
           return n*factorialRecursive(n-1)
    
    def findRunTime(functionToCheck,parameter):
        startingTime = time.time()
        functionToCheck(parameter)
        endingTime = time.time()
        return endingTime-startingTime
    
    def compareRuntime(parameter):
        timeIterative = findRunTime(factorialIterative,parameter)
        timeRecursive = findRunTime(factorialRecursive,parameter)
        if timeIterative<timeRecursive:
            print("iterative is faster by",timeRecursive*100/timeIterative,"percentage")
        elif timeIterative>timeRecursive:
            print("recursive is faster",timeIterative*100/timeRecursive,"percentage")
        else:
            print("They are equal :0")
    
    def runtimeData(parameter,sampleSize=10000):#returns an array with first index being the avg runtime for iterative likewise the second for iterative
        sumIterative = 0
        sumRecursive = 0
        for i in range(sampleSize):
            sumIterative+=findRunTime(factorialIterative,parameter)
            sumRecursive+=findRunTime(factorialRecursive,parameter)
        return [sumIterative/sampleSize,sumRecursive/sampleSize]
    
    def displayData(parameter):
        data = runtimeData(parameter)
        if data[0]<data[1]:
            print("Iterative is faster by",data[1]*100/data[0],"percentage")
        elif data[0]>data[1]:
            print("Recursive is faster",data[0]*100/data[1],"percentage")
    ```
    

But the question arises why there’s such a difference in run time even tho the time complexity of both of theses are same 🤔

Well the answer is obvious :

![Screenshot 2022-07-14 at 3.35.08 AM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8375408d-16ea-4cba-913f-d1a1b0968cdc/Screenshot_2022-07-14_at_3.35.08_AM.png)

This is it! It has everything to do with space and very little with time

Accessing the memory is way more time consuming than running iterations at a relatively larger scale.

Let me prove it 🥸

Let’s run this simple snippet to see how doing the same calculation with constants(not memory intensive) compares with using variables(uses space)

```python
import time

sum = 0

tStart=time.time()
for i in range(10000000):
    x = 10
    sum+=x#has to access x from the memory each tim
tEnd=time.time()

runTime1=tEnd-tStart

sum = 0
tStart=time.time()
for i in range(10000000):
    sum+=10
tEnd=time.time()

runTime2=tEnd-tStart

print("Avoiding variables in a simple calculation cuts down the time by",(1-(runTime2/runTime1))*100,"percent")
```

We do notice that avoiding the use of variables does indeed make the program faster. This is a fairly simple calculation for proof of concept but the increase in performance in larger apps with databases would be much higher in them.

This could come in handy while scaling up applications. For small applications, we can make things faster by prioritising time but at scale, space is far more important.

This was hard for me to accept for a very long time but it’s the truth.
