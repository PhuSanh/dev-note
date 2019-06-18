# Strong vs weak - static vs dynamic type

## Strong
You have to use cast function
> Strong typing means that the type of a value doesn't suddenly change.
```
>>> "abc" + 3
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: Can't convert 'int' object to str implicitly

>>> "abc" + str(3)
'abc3'
```

### Weak
Language auto change variable type
```
$a = "3" + "5"; // $a === 8
$b = 101 . " dogs"; // $b === "101 dogs"
```

### Static
- Each variable link to permanent (kiểu cố định) type
- Variable type is checked in compile process -> have to declare variable type clearly
- Can not change variable type (assign the other value have different type)
```
int a;  // the type of a is integer
a = 3;  // OK, assign 3 to a
a = "Hello"; // Error, cannot assign string value to int
```

### Dynamic
- Variable just like a label link to object
- Object has own type, but not variable (label)
- Type of object is checked in runtime
```
b = 3   # variable b is bound to integer object
isinstance(b, int)  # True

b = 'hello world!'   # Now b is bound to string object
isinstance(b, str)   # True

b = ['blue', 'black', 'brown'] # Now b is a list
```