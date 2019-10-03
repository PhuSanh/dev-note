# HEAP vs STACK

Both are created and stored in RAM
## STACK
- Local variable (in function)
- Param of function
- Địa chỉ trả về của hàm
## HEAP
- Pointer

#### 1. Storage size
- `Stack`: 
    - Fixed
    - Depend on OS (Windows 1MB, Linux 8MB,...)
- `Heap`: 
    - Dynamic

#### 2. Đặc điểm vùng nhớ
- `Stack`: 
    - Run by OS, 
    - Variables are deleted automatically after function done
- `Heap`: 
    - Run by developer
    - Developer have to delete manually
    - Some language have Garbage Collection (.Net, Java, Golang)


Performance Pointer and Copy: `https://medium.com/a-journey-with-go/go-should-i-use-a-pointer-instead-of-a-copy-of-my-struct-44b43b104963`