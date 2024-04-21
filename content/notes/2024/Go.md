+++
title = 'Go'
date = 2024-01-01T11:39:28.2828+05:30
draft = false
+++ 

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

```jsx
function name (params type) returnType {
	cmd..
}
//type is not mandatory

name(params)
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



## others 


Split your application into **components** written as regular Go interfaces. Don't fuss with any networking or serialization code. Focus on your business logic. (https://serviceweaver.dev/)



1. Go routine -> make threads simple
2. channel