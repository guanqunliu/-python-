

```python
class songbird(bird):
    def __init__(self):
        bird.__init__(self)
        self.sound = 'squawkil'
        
    def sing(self):
        print(self.sound)
        
s = songbird()
s.sing()

```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-1-784aac71cfec> in <module>
    ----> 1 class songbird(bird):
          2     def __init__(self):
          3         bird.__init__(self)
          4         self.sound = 'squawkil'
          5 
    

    NameError: name 'bird' is not defined



```python
class A:
    def __init__(self):
        self.a = 'a'
        
class B(A):
    def __init__(self):
        super().__init__()
        self.b='b'
        
class C(B):
    def __init__(self):
        super().__init__()   # 继承基类的初始化
        self.c='c'
        
    def test(self):
            print(self.a,self.b,self.c)
            
c=C()
c.test()
```

    a b c
    


```python
# 当可以用不同的对象解决时，不要用类 也就是用has a 的方式
class hamburger:
    def __init__(self):
        self.meat = 'beef'
        self.toppings =['tomato','bacon','onion']
        condiments = ['lime sour sauce']
        
beaf_hamburger = hamburger()
beaf_hamburger.toppings =['tomato','lettuce']
beaf_hamburger.condiments =['mayo']

chicken_hamburger= hamburger()
chicken_hamburger.meat='chicken'
chicken_hamburger.toppings =['red pepper','lettuce']
chicken_hamburger.condiments =['BBQ sauce']


```


```python
# 容器对象的访问

# __len(self)__
len('hello')
```




    5




```python
# __getitem__(self,key)
my_list= [1,2,3]
print(my_list[0])
    
```

    1
    


```python
class ArithmeticSequence:
    def __init__(self,start=0,step=1):
        self.start=start
        self.step=step
        step.changed={}
    
    def check_key(key):
            if not isinstance (key,int):raise TypeError
            if key < 0 : raise IndexError
        
    def __getitem__(self,key):
        ArithmeticSequence.check_key(key)
        try : 
            return self.changed[key]
        except keyerror:
            return self.start + key = self.step
        
    def __setitem__(self,key,value):
        ArithmeticSequence.check_key(key)
        self.changed[key] = value
    
```


      File "<ipython-input-13-b8251db2242d>", line 16
        return self.start + key = self.step
                                ^
    SyntaxError: invalid syntax
    



```python
s= ArithmeticSequence (1,2)
print(s[4])
print(s[1000])
s[4]=100
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-14-0edecd533cab> in <module>
    ----> 1 s= ArithmeticSequence (1,2)
          2 print(s[4])
          3 print(s[1000])
          4 s[4]=100
    

    NameError: name 'ArithmeticSequence' is not defined



```python

```
