

```c++

int x = 3; 
int *y = x; // y points to x
std::cout << &x; // 0x8928391
std::cout << x;

std::cout << y; // 0x8928391
std::cout << *y; // 3


```


```c++

Iterator
- Node* node_ptr


Iterator(Node* n_in)  {
	node_ptr = n_in;
}

```