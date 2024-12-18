# 3.2 Access modifiers, friend functions and classes. Constructors and initialization lists. 

## Access modifiers
....
```cpp
class C {
private:
	int x{5};
public:
	void f(int y){
		std::cout << x + y;
	}
}
```
## `friend` keyword

### friend methods
....
```cpp
class C {
private:
	int x{5};
public:
	void f(int y){
		std::cout << x + y;
	}

	friend void g(C, int);
}

void g(C c, int y) {
	std::cout << c.x + y;
}
```

we can specify definition of friend function inside of class

```cpp
class C {
private:
	int x{5};
public:
	void f(int y){
		std::cout << x + y;
	}

	friend void g(C c, int y) {
		std::cout << c.x + y;
	}
}
```
But this is not recomended


### friend classes
...
```cpp
class C {
private:
	int x{5};
public:
	void f(int y){
		std::cout << x + y;
	}

	friend class CC;
}
```

class `CC` have access to private fields of `C` 
but `C` have no access to private fields of `CC`

