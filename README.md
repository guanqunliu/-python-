# -python-2019.10.31课堂练习
def Fib(i):  
    if i==1 or i==0:  
        return 1  
    else:  
        return Fib(i-1)+Fib(i-2) 
    
print(Fib(36))
