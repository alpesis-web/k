---
layout: post_tech
title: "Programming Languages Comparison: Class and Instance"
date: 2016-04-10 02:54:41 +0800
comments: true
categories: [tech]
tags: [programming, c, c++, java, python]
toc: true
---

## Task

- Write a class `Person` with an instance variable `InitialAge` with type `int`:
  - if `initialAge` <= 0, set `age` = 0;
  - if `initialAge` > 0, set `age` = `initialAge`.
- Write a method `amIOld`:
  - if `age` < 13, print `You are young.`;
  - if `age` >= 13 and `age` < 18, print "You are a teenager.";
  - if `age` > 18, print "You are old."
- Write a method `yearPasses`:
  - increases the instance variable `age` by 1.

## Solutions

### C

Not Valid

### C++

```cpp
using namespace std;
#include <iostream>

class Person{
public:
   int age ;
   Person(int initialAge);
   void amIOld();
   void yearPasses();
};
Person::Person(int initialAge){
  // Add some more code to run some checks on initialAge
    if (initialAge <= 0) {
        cout << "Age is not valid, setting age to 0." << endl;
        age = 0;
    }
    else {
        age = initialAge;
    }

}
void Person::amIOld(){
    // Do some computations in here and print out the correct statement to the console 
    if (age < 13) {
        cout << "You are young." << endl;
    }
    else if (age >= 13 && age < 18) {
        cout << "You are a teenager." << endl;
    }
    else {
        cout << "You are old." << endl;
    }
}
    

void Person::yearPasses(){
  // Increment the age of the person in here
  age += 1;

}

int main(){
    int T,age;
    cin>>T;
    for(int i=0;i<T;i++)
    {
        cin>>age;
        Person p(age);
        p.amIOld();
        for(int j=0;j<3;j++)
        {
            p.yearPasses();
            
        }
        p.amIOld();
        cout<<"\n";
    }
    return 0;
}
```


### Java

```java
public class Person {
    private int age;	
  
	public Person(int initialAge) {
  		// Add some more code to run some checks on initialAge
        if (initialAge <= 0){
            System.out.println("Age is not valid, setting age to 0.");
            age = 0;
        }
        else {
            age = initialAge;
        }
	}

	public void amIOld() {
  		// Write code determining if this person's age is old and print the correct statement:
        String output;
        if (age < 13) {
            output = "You are young.";
        } 
        else if (age >= 13 && age < 18) {
            output = "You are a teenager.";
        }
        else {
            output = "You are old.";
        }
        System.out.println(output);
	}

	public void yearPasses() {
  		// Increment this person's age.
        age += 1;
	}

	public static void main(String[] args) {
        	Scanner sc = new Scanner(System.in);
        	int T = sc.nextInt();
      		for (int i = 0; i < T; i++) {
        		int age = sc.nextInt();
          		Person p = new Person(age);
          		p.amIOld();
          		for (int j = 0; j < 3; j++) {
            		p.yearPasses();
          		}
          		p.amIOld();
                System.out.println();
        }
    }
}
```

### Python

Python 2

```python
class Person:
    def __init__(self,initialAge):
        # Add some more code to run some checks on initialAge
        if initialAge <= 0:
            print "Age is not valid, setting age to 0."
            self.age = 0
        else:
            self.age = initialAge
        
    def amIOld(self):
        # Do some computations in here and print out the correct statement to the console
        if self.age < 13:
            print "You are young."
        elif 13 <= self.age < 18:
            print "You are a teenager."
        else:
            print "You are old."
        
    def yearPasses(self):
        # Increment the age of the person in here  
        self.age += 1
        return self.age

T=int(raw_input())
for i in range(0,T):
    age=int(raw_input())         
    p=Person(age)  
    p.amIOld()
    for j in range(0,3):
        p.yearPasses();        
    p.amIOld();
    print ""
```

Python 3

```python
class Person:
        def __init__(self,initialAge):
       		# Add some more code to run some checks on initialAge
            if initialAge <= 0:
                print("Age is not valid, setting age to 0.")
                self.age = 0
            else:
                self.age = initialAge
                
        def amIOld(self):
            # Do some computations in here and print out the correct statement to the console
            if self.age < 13:
                print("You are young.")
            elif 13 <= self.age < 18:
                print("You are a teenager.")
            else:
                print("You are old.")

        def yearPasses(self):
            # Increment the age of the person in here     
            self.age += 1

T=int(input())
for i in range(0,T):
    age=int(input())         
    p=Person(age)  
    p.amIOld()
    for j in range(0,3):
        p.yearPasses();        
    p.amIOld();
    print ("")
```
