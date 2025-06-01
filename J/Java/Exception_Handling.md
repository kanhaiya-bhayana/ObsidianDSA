### 🔹 Error
- Represent critical issues that a program generally cannot recover from.
	- OutofMemoryError
	- StackOverflowError --> Infinite recursion (no break condition)

### 🔹 Exception
- Represent conditions that can occur during a program's execution that may be handled.
- Two  Types
	- 1. Un-Checked / Runtime Exception
		- ArrayIndexOutOfBoundException
		- NullPointerException
		- ArithmeticExcepiton
		- IllegalArgumentException
	- 2. Checked / Compile Time Exception
		- ClassNotFoundException
		- InterruptedException
		- IOException
			- FileNotFound
			- EOFException


### Why need Exception handling?

- It makes our code clean by separating the error handling code from the regular code.
- It allows program to recover from the error.
- It allows us to add more information, which support debugging.
- Improves security, by hiding the sensitive information.


#### Disadvantages
- Exception handling is little expensive, if stack trace is huge and it is not handled or handled at the parent class