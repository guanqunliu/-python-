

```python
class Calculator:
    def calculate(self, expression):
        return eval(expression)
        
class Talker:
    def talk(self, value):
        print('My value is:', value)
        
class TalkingCalculator:
    def __init__(self):
        self.__calculator = Calculator()
        self.__talker = Talker()
        self.__value = None
        
    def calculate(self, expression):
        self.__value = self.__calculator.calculate(expression)
    
    def talk(self):
        self.__talker.talk(self.__value)
    

tc = TalkingCalculator()
tc.calculate('1 + 2 + 3')
tc.talk()
```

    My value is: 6
    


```python
class Calculator:
    def calculate(self, expression):
        return eval(expression)
        
class Talker:
    def talk(self, value):
        print('My value is:', value)
        
class TalkingCalculator(Calculator):
    def __init__(self):
        self.__talker = Talker()
        
    def calculate(self, expression):
        value = Calculator.calculate(self, expression)
        self.__talker.talk(value)
        
tc = TalkingCalculator()
tc.calculate('1 + 2 * 3')
```

    My value is: 7
    


```python
class Screwdriver:
    def screw(self):
        print('screwing...')
        
class Knife:
    def cut(self):
        print('cutting...')
        
class SwissKnife(Screwdriver, Knife):
    pass

sk = SwissKnife()
sk.screw()
sk.cut()
```

    screwing...
    cutting...
    


```python
class Screwdriver:
    def __init__(self, size):
        self.size = size
        
    def screw(self):
        print('screwing with a screwdriver of size {}...'.format(self.size))
        
class Knife:
    def cut(self):
        print('cutting...')
        
class SwissKnife(Screwdriver, Knife):
    def __init__(self, screwdriver_size):
        Screwdriver.__init__(self, screwdriver_size)
        Knife.__init__(self)

sk = SwissKnife(4)
sk.screw()
sk.cut()
```

    screwing with a screwdriver of size 4...
    cutting...
    


```python
class Screwdriver:
    def __init__(self, size):
        self.size = size
        
    def work(self):
        print('screwing with a screwdriver of size {}...'.format(self.size))
        
class Knife:
    def work(self):
        print('cutting...')

class Dog:
    def work(self):
        print('chasing a ball...')

class Fork:
    def pick(self):
        print('picking...')
        
class SwissKnife:
    def __init__(self):
        self.tools = {}
    
    def add_tool(self, name, tool):
        if callable(getattr(tool, 'work', None)):
            self.tools[name] = tool
        else:
            print('{} is not a valid tool, work() not found.'.format(name))
    
    def use_tool(self, name):
        tool = self.tools.get(name)
        if tool is not None:
            tool.work()
        else:
            print('{} not found.'.format(name))
    
sk = SwissKnife()
sk.add_tool('Knife', Knife())
sk.add_tool('Big screwdriver', Screwdriver(10))
sk.add_tool('Small screwdriver', Screwdriver(2))
sk.add_tool('Dog', Dog())
sk.add_tool('Fork', Fork())
sk.use_tool('Big screwdriver')
sk.use_tool('Small screwdriver')
sk.use_tool('Knife')
sk.use_tool('Dog')
sk.use_tool('Fork')
```

    Fork is not a valid tool, work() not found.
    screwing with a screwdriver of size 10...
    screwing with a screwdriver of size 2...
    cutting...
    chasing a ball...
    Fork not found.
    


```python
knife = Knife()
hasattr(knife, 'work')
```




    True




```python
hasattr(knife, 'cut')
```




    False




```python
callable(getattr(knife, 'work'))
```




    True




```python
setattr(knife, 'cut', None)
```


```python
callable(getattr(knife, 'cut'))
```




    False




```python
callable(getattr(knife, 'screw', None))
```




    False




```python
knife.__dict__
```




    {'cut': None}




```python
from abc import ABC, abstractmethod

class Tool(ABC):
    @abstractmethod
    def work(self):
        pass
    
```


```python
class Screwdriver(Tool):
    def __init__(self, size):
        self.size = size
        
    def work(self):
        print('screwing with a screwdriver of size {}...'.format(self.size))
        
class Knife(Tool):
    def work(self):
        print('cutting...')

class Dog:
    def work(self):
        print('chasing a ball...')

class Fork(Tool):
    def pick(self):
        print('picking...')
        
class SwissKnife:
    def __init__(self):
        self.tools = {}
    
    def add_tool(self, name, tool):
        if isinstance(tool, Tool):
            self.tools[name] = tool
        else:
            print('{} is not a valid tool, not derived from class Tool.'.format(name))
    
    def use_tool(self, name):
        tool = self.tools.get(name)
        print('{}: '.format(name), end='')
        if tool is not None:
            tool.work()
        else:
            print('not found.')
    
sk = SwissKnife()
sk.add_tool('Knife', Knife())
sk.add_tool('Big screwdriver', Screwdriver(10))
sk.add_tool('Small screwdriver', Screwdriver(2))
sk.add_tool('Dog', Dog())
# sk.add_tool('Fork', Fork())
sk.use_tool('Big screwdriver')
sk.use_tool('Small screwdriver')
sk.use_tool('Knife')
sk.use_tool('Dog')
# sk.use_tool('Fork')
```

    Dog is not a valid tool, not derived from class Tool.
    Big screwdriver: screwing with a screwdriver of size 10...
    Small screwdriver: screwing with a screwdriver of size 2...
    Knife: cutting...
    Dog: not found.
    


```python
from time import sleep
import random

class Bar:
    __empty = '-'
    def __init__(self, length=80):
        self.bar = list()
        for i in range(0, length):
            self.bar.append(Bar.__empty)
    
    def reset(self):
        for i in range(len(self.bar)):
            self.bar[i] = Bar.__empty
            
    def update(self, ants):
        self.reset()
        for ant in ants:
            if 0 <= ant.position < len(self.bar):
                self.bar[ant.position] = ant.symbol
                
    def is_empty(self):
        for item in self.bar:
            if not item == Bar.__empty:
                return False
        else:
            return True
        
    def show(self):
        for ch in self.bar:
            print(ch, sep='', end='')
        print('', end='\r')
        
    
class Ant:
    symbols = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
    count = 0
    def __init__(self, position, direction, symbol=None):
        self.position = position
        self.direction = direction
        if symbol is None:
            self.symbol = Ant.symbols[Ant.count]
            Ant.count += 1
        else:
            self.symbol = symbol
        
    def move(self):
        self.position += self.direction
        
    def create_random_ant(length=80):
        position = random.randint(0, length - 1)
        direction = random.choice([1, -1])
        return Ant(position, direction)
        
class Ants:
    def __init__(self, count=3):
        self.ants = list()
        for i in range(count):
            self.ants.append(Ant.create_random_ant())

    def move(self):
        for ant in self.ants:
            ant.move()

    def get_ants(self):
        return self.ants

bar = Bar(80)
ants = Ants(5)

bar.update(ants.get_ants())
bar.show()

while not bar.is_empty():
    sleep(0.25)
    ants.move()
    bar.update(ants.get_ants())
    bar.show()      
```

    --------------------------------------------------------------------------------


```python
1 / 0
```


    ---------------------------------------------------------------------------

    ZeroDivisionError                         Traceback (most recent call last)

    <ipython-input-49-bc757c3fda29> in <module>
    ----> 1 1 / 0
    

    ZeroDivisionError: division by zero



```python
raise Exception
```


    ---------------------------------------------------------------------------

    Exception                                 Traceback (most recent call last)

    <ipython-input-50-2aee0157c87b> in <module>
    ----> 1 raise Exception
    

    Exception: 



```python
raise Exception('System fault.')
```


    ---------------------------------------------------------------------------

    Exception                                 Traceback (most recent call last)

    <ipython-input-51-70fa745d48e3> in <module>
    ----> 1 raise Exception('System fault.')
    

    Exception: System fault.



```python
class MyException(Exception): pass

raise MyException()
```


    ---------------------------------------------------------------------------

    MyException                               Traceback (most recent call last)

    <ipython-input-52-691cae0aaae3> in <module>
          1 class MyException(Exception): pass
          2 
    ----> 3 raise MyException()
    

    MyException: 



```python
try:
    x = int(input('Please input a number: '))
    y = int(input('Please input a number: '))
    print('{}/{} = {}'.format(x, y, x / y))
    print('hello')
except ZeroDivisionError:
    print('The second number cannot be 0!')
```

    Please input a number:  1
    Please input a number:  0
    

    The second number cannot be 0!
    


```python
def calc(expression, no_throw=False):
    try:
        return eval(expression)
    except ZeroDivisionError:
        if no_throw:
            print('Division by zero.')
        else: raise ValueError from None
```


```python
calc('10/0')
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-66-0afddcb032f4> in <module>
    ----> 1 calc('10/0')
    

    <ipython-input-65-3d0198937b67> in calc(expression, no_throw)
          5         if no_throw:
          6             print('Division by zero.')
    ----> 7         else: raise ValueError from None
    

    ValueError: 



```python
calc('10/0', True)
```

    Division by zero.
    


```python
try:
    x = int(input('Please input a number: '))
    y = int(input('Please input a number: '))
    print('{}/{} = {}'.format(x, y, x / y))
    print('hello')
except ZeroDivisionError:
    print('The second number cannot be 0!')
except ValueError:
    print('Invalid value.')
```

    Please input a number:  1
    Please input a number:  0
    

    The second number cannot be 0!
    


```python
try:
    x = int(input('Please input a number: '))
    y = int(input('Please input a number: '))
    print('{}/{} = {}'.format(x, y, x / y))
    print('hello')
except (ZeroDivisionError, ValueError) as e:
    print(e)
```

    Please input a number:  1
    Please input a number:  0
    

    division by zero
    


```python
while True:
    try:
        x = int(input('Please input a number: '))
        y = int(input('Please input a number: '))
        print('{}/{} = {}'.format(x, y, x / y))
    except (ZeroDivisionError, ValueError) as e:
        print(e)
    else: break        
```

    Please input a number:  a
    

    invalid literal for int() with base 10: 'a'
    

    Please input a number:  1
    Please input a number:  0
    

    division by zero
    

    Please input a number:  1
    Please input a number:  2
    

    1/2 = 0.5
    hello
    


```python
x = None
try:
    x = 1 / 0
finally:
    print('cleaning up...')
#    del x
```

    cleaning up...
    


    ---------------------------------------------------------------------------

    ZeroDivisionError                         Traceback (most recent call last)

    <ipython-input-77-026ada9cb6d9> in <module>
          1 x = None
          2 try:
    ----> 3     x = 1 / 0
          4 finally:
          5     print('cleaning up...')
    

    ZeroDivisionError: division by zero



```python
def inner():
    print('inner...')
    raise Exception
    
def outer():
    print('outer...before calling inner.')
    inner()
    print('outer...after calling inner.')

outer()
```

    outer...before calling inner.
    inner...
    


    ---------------------------------------------------------------------------

    Exception                                 Traceback (most recent call last)

    <ipython-input-79-8aeb440e3bcd> in <module>
          8     print('outer...after calling inner.')
          9 
    ---> 10 outer()
    

    <ipython-input-79-8aeb440e3bcd> in outer()
          5 def outer():
          6     print('outer...before calling inner.')
    ----> 7     inner()
          8     print('outer...after calling inner.')
          9 
    

    <ipython-input-79-8aeb440e3bcd> in inner()
          1 def inner():
          2     print('inner...')
    ----> 3     raise Exception
          4 
          5 def outer():
    

    Exception: 



```python

```
