
---
title : A Philosophy of Software Design Book Notes'
date  : 2024-01-18T08:19:32.3232+05:30
tags  : ['book']
draft : false
---

## The Nature of Complexity

Complexity is anything related to the structure of a software systemthat makes it hard to understand and modify the system.

#### Symptoms of complexity

1. **Change amplification**: The first symptom of complexity is that a seemingly simple change requires code modifications in many different places.
2. **Cognitive load:** how much a developer needs to know in order to complete a task
3. **Unknown unknowns:**  it is not obvious which pieces of code must be modified to complete a task, or whatinformation a developer must have to carry out the task successfully


## Strategic vs. Tactical Programming

#### Tactical programming
In the tactical approach, your main focus is to get something working, such as a new feature or a bug fix.
#### Strategic programming
Strategic programming requires an investment mindset. Rather than
taking the fastest path to finish your current project, you must invest time to
improve the design of the system.

## Modules Should Be Deep

A  software system is decomposed into a collection of modules that are relatively independent.
#### Deep modules

A deep module are the modules that take care of the most of things 
The mechanism for file I/O provided by the Unix operating system and its descendants, such as Linux, is a beautiful example of a deep interface.

There are only five basic system calls for I/O, with simple signatures:

```
int open(const char* path, int flags, mode_t permissions);
ssize_t read(int fd, void* buffer, size_t count);
ssize_t write(int fd, const void* buffer, size_t count);
off_t lseek(int fd, off_t offset, int referencePosition);
int close(int fd);
```

The abstraction will take care of bellow thing

- How are files represented on disk in order to allow efficient access?
- How are directories stored, and how are hierarchical path names processed to find the files they refer to?
- How are permissions enforced, so that one user cannot modify or delete another user’s files?
- How are file accesses implemented? For example, how is functionality divided between interrupt handlers and background code, and how do these two elements communicate safely?
- What scheduling policies are used when there are concurrent accesses to multiple files?
- How can recently accessed file data be cached in memory in order to reduce the number of disk accesses?

Don't expose the thing that no need for user. try to abstract the things as much as possible

## Information Hiding (and Leakage)

#### Information hiding

The basic idea is that each module should encapsulate a few pieces of knowledge, which represent design decisions. The knowledge is embedded in the module’s implementation but does not appear in its interface, so it is not visible to other modules.

examples of information that might be hidden within a module (it not about using private method .it is about hiding the hard implementation from user)

- How to store information in a B-tree, and how to access it efficiently.
- How to identify the physical disk block corresponding to each logical block within a file.
- How to implement the TCP network protocol.
- How to schedule threads on a multi-core processor.
- How to parse JSON documents.
#### Information leakage

when a design decision is reflected in multiple modules. This creates a dependency between the modules: any change to that design decision will require changes to all of the involved modules

Example: Imagine a scenario in a document processing application where there are two classes: one for reading documents from various file formats and another for writing documents in those formats.

```python
class DocumentReader:
    def __init__(self, file_path):
        # Initialization logic

    def read_document(self):
        # Read document logic
        pass

    # Other methods related to reading documents


class DocumentWriter:
    def __init__(self, file_path):
        # Initialization logic

    def write_document(self, document):
        # Write document logic
        pass

    # Other methods related to writing documents
```

In this design, both `DocumentReader` and `DocumentWriter` classes have knowledge of specific file formats. If the file format changes, both classes would need to be modified, leading to information leakage. The dependency on the file format is not explicitly part of the interface, but it exists implicitly in the implementation details.


Improved Design to Mitigate Information Leakage:

```python
class DocumentProcessor:
    def __init__(self, file_path):
        # Initialization logic

    def read_document(self):
        # Read document logic
        pass

    def write_document(self, document):
        # Write document logic
        pass

    # Other methods related to document processing
```

**Temporal decomposition**

Common cause for the information leakage. Consider an application that reads a file in a particular format, modifies the contents ofthe file, and then writes the file out again. With temporal decomposition, this application might be broken into three classes: one to read the file,another to perform the modifications, and a third to write out the new version. Both the file reading and file writing steps have knowledge about the file format, which results in information leakage.


`Note:` **When designing modules, focus on the knowledge that’s needed to perform each task, not the order in which tasks occur.**

## General-Purpose Modules are Deeper

#### Make classes somewhat general purpose

general-purpose interface could potentially be used for other purposes besides what it supposed to do think for future how it will adpat if we want to changes.Generality leads to better information hiding

special purpose interface are the inteface only for solving current specific problem it won't for future case.
#### Questions to ask yourself
1. What is the simplest interface that will cover all my current needs?
2. In how many situations will this method be used? (if it is one time then it is special purpose. try to combine multipl special purpose. in to single genral purpose)
3. Is this API easy to use for my current needs?

## Different Layer, Different Abstraction

#### Pass-through methods

```java

// Service Layer
public class BusinessLogicService {
    public void performComplexOperation(String data) {
        // Complex business logic implementation
        System.out.println("Performing complex operation with data: " + data);
    }
}

// Facade Layer (Pass-through layer)
public class FacadeLayer {
    private BusinessLogicService businessLogicService;

    public FacadeLayer() {
        this.businessLogicService = new BusinessLogicService();
    }

    public void exposeSimilarOperation(String data) {
        // Pass-through method
        businessLogicService.performComplexOperation(data);
    }
}

// Client or User Interface Layer
public class UserInterface {
    public static void main(String[] args) {
        FacadeLayer facadeLayer = new FacadeLayer();
        
        // Client invokes a method on the facade layer
        facadeLayer.exposeSimilarOperation("Some data");
    }
}

```

While this design might be reasonable in some cases, it can lead to problems when the facade layer becomes a mere pass-through, offering little additional value. In situations like this:

- The facade layer adds minimal or no logic of its own.
- The client could potentially interact directly with the service layer, bypassing the facade layer.

This scenario might indicate a lack of clear separation of concerns or an unnecessary abstraction layer. If the facade layer doesn't add meaningful functionality or abstraction beyond simply invoking methods from the service layer, it might be worth reconsidering the design to ensure a more effective and maintainable architecture. The goal is to have each layer in the system contribute value and have a clear purpose, rather than serving as a simple pass-through.

#### Pass-through variables

Variable that is passed down through a long chain of methods.Pass-through variables add complexity because they force all of the intermediate methods to be aware of their existence, even though the methods have no use for the variables.

**Solution:** use context (A context stores all of the application’s global state)


## Pull Complexity Downwards

Suppose that you are developing a new module, and you discover a piece of unavoidable complexity. Which is better: should you let users of the module deal with the complexity, or should you handle the complexity internally within the module?

If the complexity is related to the functionality provided by the module handle internally else the users to handle the complexity.

**Example:** Avoid Configuration parameters for the module even though it give control to users but In many cases, it’s difficult or impossible for users or administrators to determine the right values for the parameters . consider network protocol where it implemented the retry logic with perodic time by analysing own

Before exporting a configuration parameter, ask yourself `will users (or higher-level modules) be able to determine a better value than we can determine here? `

## Better Together Or Better Apart?

If the pieces are unrelated, they are probably better off apart. Here are a few indications that two pieces of code are related:

- They share information; for example, both pieces of code might depend on the syntax of a particular type of document
- They are used together: anyone using one of the pieces of code is likely to use the other as well and vice versa and make sure the module not used by other if it can be used by multiple it need to be sepreate.
- It is hard to understand one of the pieces of code without looking at the other.


**Repetition  (RED FLAG)**

If the same piece of code (or code that is almost the same) appears over and over again,  that’s a red flag that you haven’t found the right abstractions.

**Special-General Mixture  (RED FLAG)**

when a general-purpose mechanism also contains code specialized for a particular use of that mechanism. This makes the mechanism more complicated and creates information leakage between the mechanism and the particular use case: future modifications to the use case are likely to require changes to the underlying mechanism as well.

#### Splitting and joining methods

Methods containing hundreds of lines of code are fine if they have a simple signature and are easy to read. These methods are deep (lots of functionality, simple interface), which is good.

If a method has all of these properties, then it probably doesn’t matter whether it is long or not

1. Each method should do one thing and do it completely
2. The method should have a simple interface, so that users don’t need to have much information in their heads in order to use it correctly.
3. its interface should be much simpler than its implementation

**Example:** we used to write a wrapper for loggin purpose but it just one line instead of writing that we can directly log where it needed.

When spliting large function in to small function make sure the below things

-  someone reading the child method doesn’t need to know anything about the parent method and vice versa. (`child method is relatively general-purpose`)

**Conjoined Methods (RED FLAG)**

It should be possible to understand each method independently. If you can’t understand the implementation of one method without also understanding the implementation of another

## Define Errors Out Of Existence

The best way to eliminate exception handling complexity is to define your APIs so that there are no exceptions to handle: define errors out of existence

**Example:** File Deletion in window throw error if file used by some other process. but in linux it will not it marked for deletion and wait for the process to compelete and once it done the file get removed.  

Java substring function will throw error if the index is out of range it better the function to hadle the case and return accordingly. 

#### Mask exceptions

Reducing the number of places where exceptions must be handled is exception masking.With this approach, an exceptional condition is detected and handled at a low level in the system, so that higher levels of software need not be aware of the condition.

**Example:** NFS network file system will not throw error if the NFS server crashed it will retry until it get connected or the threshold time reach.

`NOTE:` Exception masking doesn’t work in all situations, but it is a powerful tool in the situations where it works.

#### Exception aggregation

exception aggregation is to handle many exceptions with a single piece of code; rather than writing distinct handlers for many individual exceptions, handle them all in one place with a single handler.


## Design it Twice

your first thoughts about how to structure a module or system will produce the best design. You’ll end up with a much better result if you consider multiple options for each major design decision **design it twice**.

## Why Write Comments? The Four Excuses

the process of writing comments, if done correctly, will actually improve a system’s design

- Good code is self-documenting
- Comments get out of date and become misleading
- I don’t have time to write comments
- All the comments I have seen are worthless

## Comments Should Describe Things that Aren’t Obvious from the Code

Developers should be able to understand the abstraction provided by a module without reading any code other than its externally visible declarations. The only way to do this is by supplementing the declarations with comments.

**how to write good comments**
- Pick conventions
- Don’t repeat the code
- Lower-level comments add precision (Some comments provide information at a lower, more detailed, level than the code; these comments add precision by clarifying the exact meaning of the code)
- Higher-level comments enhance intuition
- Implementation comments: what and why, not how

## Choosing Names
 
 - Names should be precise
 - Use names consistently
 - Avoid extra words

## Write The Comments First

Use Comments As Part Of The Design Process

## Modifying Existing Code

If you want to maintain a clean design for a system, you must take a strategic approach when modifying existing code. **Ideally, when you have finished with each change, the system will have the structure it would have had if you had designed it from the start with that change in mind. To achieve this goal, you must resist the temptation to make a quick fix. Instead, think about whether the current system design is still the best one, in light of the desired change.** If not, refactor the system so that you end up with the best possible design. With this approach, the system design improves with every modification.

## Consistency

Consistency can be applied at many levels in a system; here are a few
examples.
- Names.
- Coding style
- Design patterns

#### Ensuring consistency

Create a document that lists the most important overall conventions, such as coding style guidelines. Place the document in a spot where developers are likely to see it

**Don’t change existing conventions**

Resist the urge to “improve” on existing conventions. **Having a “better idea” is not a sufficient excuse to introduce inconsistencies.** Your new idea may indeed be better, but the value of consistency over inconsistency is almost always greater than the value of one approach over another.

Before introducing inconsistent behavior, ask yourself two questions

1. do you have significant new information justifying your approach that wasn’t available when the old convention was established
2. is the new approach so much better that it is worth taking the time to update all of the old uses

## Code Should be Obvious

If code is obvious, it means that someone can read the code quickly, without much thought, and their first guesses about the behavior or meaning of the code will be correct. If code is obvious, a reader doesn’t need to spend much time or effort to gather all the information they need to work with the code.

**Things that make code less obvious**

- Event-driven programming
- Generic containers
- Code that violates reader expectations

## Software Trends

- Object-oriented programming and inheritance
- Agile development (development should be incremental and iterative)
- Unit tests
- Test-driven development
- Design patterns
## Designing for Performance

have a general sense for what is expensive and what is cheap, you can use that information to choose cheap operations whenever possible

## Decide What Matters

- Minimize what matters
- Thinking more broadly