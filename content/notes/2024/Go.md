---
title: Go
date: 2024-01-01T11:39:28.2828+05:30
draft: false
tags: programming
---

## Introduction

- From c++ by google c++ author will convert to exe
    
- go get githublink→ to download pkg and put under folder pkg
    
- it has main function to run `go run filename.go`
    
    ```jsx
    package main //-> our package name 
    
    imort "fmt"
    
    import ("fmt","math") //-> to import multiple pkg
    
    func main(){
    	fmt.Println("hello")
    }
    ```
    

## Data types

- string, bool, int, byte , float32 ,float64, arrays
    
- using var keyword and const → to not reassign
    
- var name string = “d” (if directly assing vaule we don’t need to mention the data type)
    
- var age = 23
    
- name := “test” → this kind of declaration we can be only defined inside function
    
- name,email := “test”,”email”
    
- Arrays
    
    ```jsx
     var name [2] string 
    name[0] = "1"
    
    name := [2]string("1","2") //size optional
    //slice 
    name := [2]string{"1","2"}
    
    len(name) //-> to get length
    
    name[1:2] //-> slice the array
    ```
    
- Struct
    
    ```jsx
    type Animal struct {
    class string
    age int
    }
    
    var teddy = Animal{ class:"bear",age:14}
    or 
    Animal{"bear",14}
    
    //teddy.age
    ```
    
- Map
    
    ```jsx
     emails :=  make(map[string]string)
    
     := map[stringstring{"key":"value"}
    
    emails["name"]= "key"
    ```
    

## Function

```go
function name (params type) returnType {
	cmd..
}
//type is not mandatory

name(params)

#multiple return vaule
function name (params type) (int,bool..) {
	return 4,true,..
}
 number ,bool := name()
we can also give name for the return type of function like 

function name () (number int , ..) -> it is purely for doc
#Error Handling

```

- Arguments are passed as copy use pointer for call by reference


## Pointers

```go
package main

import "fmt"

func main() {
    // Declare a variable
    var num int = 10
    
    // Declare a pointer variable
    var ptr *int
    
    // Assign the address of num to ptr
    ptr = &num
    
    // Access the value through the pointer
    fmt.Println("Value of num:", num)
    fmt.Println("Value of num through pointer:", *ptr)
    
    // Modify the value through the pointer
    *ptr = 20
    fmt.Println("New value of num:", num)
}

```
## Conditions

```jsx
if x<y || cond2  {
	//code
}  else if {
	//code
} else {
	//code
}

//switch
color := "red"

switch color {
	case "red":
		//code
  case "":
			//code
	default:
		//code
}
```

## Loops

```jsx
i :=1
for i<=10 {
	//code 
  i++
}

for i:=1 i<10;i++ {
 //code
}

//using range
ids := []int{33,33}

for i,id := range ids {
	
}

//range with map

for k,v := range map { }

```



## Runes
Go’s runes are used to represent single characters. 'A' ->65
Runes are kept as numeric codes

If you declare a variable without assigning it a value, that variable will
contain the zero value

## short variable declaration

instead of explicitly declaring the type of the variable and later assigning to it with
= , you do both at once using := .

```Go
quantity := 4
```

If the name of a variable, function, or type begins with a capital letter, it is considered exported and can be accessed from packages outside the current one.
## Conversions
Only same type can be do math operation float and int cannot to convert
- int(3.0)
- float64(9)




## Packages

Go tools look for package code in a special directory (folder) on your computer called the workspace. By default, the workspace is a directory named go in the current user’s home directory.

The workspace directory contains three subdirectories
- bin, which holds compiled binary executable programs.
- pkg, which holds compiled binary package files. 
- src, which holds Go source code.




## others 


Split your application into **components** written as regular Go interfaces. Don't fuss with any networking or serialization code. Focus on your business logic. (https://serviceweaver.dev/)



1. Go routine -> make threads simple
2. channel