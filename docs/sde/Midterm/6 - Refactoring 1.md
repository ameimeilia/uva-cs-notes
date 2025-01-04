---
layout: default
title: Refactoring 1
parent: Midterm Notes
nav_order: 6
---
# Defensive Programming

## Understanding Defensiveness
- Anticipates user errors (e.g., wrong input files) and handles them gracefully.
- **User errors**: Not the programmer’s fault but need handling (e.g., missing arguments, invalid files).
- Users should never see programmer errors (e.g., NullPointerException).
- Inform users when their error occurs with clear messages.

## Client Class
- **Client class**: Any class or user interacting with our code.
- Ensure client uses class correctly, not allowing erroneous states.
- Notify clients when they use the class incorrectly.

### Example: `BankAccount`

``` Java
public class BankAccount {
    private final int id;
    private double balance;
    
    public BankAccount(int id, double startingBalance) {
        this.id = id;
        this.balance = startingBalance;
    }
    
    public BankAccount(int id) {
        this(id, 0);
    }
}
```

#### Creating our Exception

``` Java
// exception for creating account with negative balance
public class NegativeBalanceException extends IllegalArgumentException{
    public NegativeBalanceException(String message) {
        super(message);
    }
}
```


``` Java
// updated contructor
    public BankAccount(int id, double startingBalance) {
        this.id = id;
        if (startingBalance < 0) {
            throw new NegativeBalanceException(getNegativeBalanceMessage());
        }
        this.balance = startingBalance;
    }

    private String getNegativeBalanceMessage() {
        return "Error: attempted to create account #" + id + " with a negative balance.";
    }
```

#### withdraw and deposit

``` Java
    public void withdraw(double amount) {
        balance -= amount;
    }
    
    public void deposit(double amount) {
        balance += amount;
    }
```

##### Error codes

``` Java
// error code to handle withdraw
    public boolean withdraw(double amount) {
        if (amount > balance) {
            return false;
        }
        balance -= amount;
        return true;
    }
```

```Java
// return value of error codes might be ignored -> use exceptions instead
    public void withdraw(double amount) {
        if (amount > balance) {
            throw new InsufficientFundsException(getInsufficientFundsMessage(amount));
        }
        balance -= amount;
    }

    private String getInsufficientFundsMessage(double amount) {
        return "Error: insufficient funds in account #" + id + " - balance: " + balance + 
                " for transaction amount: " + amount;
    }
```

```Java
public class InsufficientFundsException extends RuntimeException {
    public InsufficientFundsException(String message) {
        super(message);
    }
}
```

### Rollback and Throw
- If an error is detected after the operation, rollback changes before throwing an exception.
- Ensures no erroneous states occur even when post-conditions fail.

## assert keyword
- Used during development to check conditions.
- Syntax: `assert [boolean_statement]`
- If the statement is `false`, an **`AssertionError`** is thrown.
- **AssertionError** is not an exception and cannot be caught with `try-catch`.

### Example:

```Java
public void withdraw(double amount) {
    assert amount > 0 && amount <= balance;
    balance -= amount;
}
```
    
- By default, **assertions** are disabled in Java.
- To enable assertions, add `-ea` to the **VM arguments** in the run configurations window.
- **Do not rely on `assert`** statements in deployed software.
- Replace `assert` statements with **exceptions** for production code, especially in libraries for client use.

# Exceptions

## When to throw exceptions
- Use exceptions to enforce **pre-conditions** and **post-conditions**.

*example - pre-condition: prime number must be positive*

```Java
    public boolean isPrime(int number) {
        if (number <= 0) {
            throw new RuntimeException("isPrime can only be called positive inputs. Was called with " + number);
        }
        for (int i = 2; i <= Math.sqrt(number); i++) {
            if (number % i == 0) {
                return false;
            }
        }
        return true;
    }
```

## When not to throw exceptions
- Avoid using exceptions for **control-flow** (e.g., using try-catch for index validation).
- Instead, use simple logical checks (e.g., validating list indices with a boolean statement).

## When to use try-catch
Use try-catch when:
- You can **meaningfully handle** the exception.
- Dealing with **checked exceptions** (e.g., `IOException`).
- In general, when dealing with live user input, you never want to allow the program to crash.

- never catch exception you can’t handle meaningfully

### Checked Exceptions
- **Checked exceptions** (e.g., `IOException`, `FileNotFoundException`) can be difficult to handle.
- **Encapsulate file I/O** and exception handling in the method
- **Convert checked exceptions** into unchecked `RuntimeException` to avoid forcing clients to handle them.
- Pass the original exception into the `RuntimeException` constructor to preserve the error message.
- Handling exceptions in the client (e.g., `Main`) **increases coupling**.
- Clients should only handle exceptions when they can **meaningfully handle** them

```Java
    public String readTextFile(String filename) {
        try {
            BufferedReader bufferedReader = getBufferedReader(filename);
            return getFileContents(bufferedReader);
        // pass original exception to RuntimeException
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

    private BufferedReader getBufferedReader(String filename) throws FileNotFoundException {
        FileReader fileReader = new FileReader(filename);
        return new BufferedReader(fileReader);
        }
    
    private String getFileContents(BufferedReader bufferedReader) throws IOException {
        StringBuilder fileContents = new StringBuilder();
        for (String line = bufferedReader.readLine(); line != null; line = bufferedReader.readLine()) {
            fileContents.append(line);
        }
        return fileContents.toString();
    }
```

## Writing your own Exceptions
- Custom exceptions help **communicate specific issues**
- In general, `RuntimeException` is a good class for custom exceptions to extend

*example*: `InsufficientFundsException` explains the account balance and transaction amount.

```
public class InsufficientFundsException extends RuntimeException {
    public InsufficientFundsException(String message) {
        super(message);
    }
}
```

### Exception error messages
Messages should communicate:
1. What caused the exception.
2. How to prevent it in the future.

*example -* `InsufficientFundsException`

```Java
    private String getInsufficientFundsMessage(BankAccount account, double amount) {
        return "Error: insufficient funds in account #" + id + " - balance: " + balance +
        " for transaction amount: " + amount;
    }
/* example output: 
 * `Exception in thread "main" edu.virginia.cs.exceptions.InsufficientFundsException:  * Error: insufficient funds in account #1234 - balance: 500.0 for transaction amount  * 700.0`
 */
```

- Clients are responsible for **handling** exceptions (e.g., canceling a transaction if funds are insufficient).

```Java
    try {
        account.withdraw(amount);
    } catch (InsufficientFundsException e) {
        screen.display("We're sorry, but you have insufficient funds for that transaction. The transaction has been canceled.");
    }   
```

# Testing with Exceptions
- Best practice: Throw exceptions for incorrect/invalid inputs.
- Tests should expect exceptions when preconditions are violated.

*example - testing BankAccount `withdraw`*
preconditions:
- `amount` cannot be negative
- `amount` cannot exceed `balance`

*example - equivalence test*

```Java
import org.junit.jupiter.api.*;

import static org.junit.jupiter.api.Assertions.*;

public class BankAccountTest {
    private BankAccount testAccount;
    
    @BeforeEach
    public void setup() {
        testAccount = new BankAccount(1234, 500);
    }
    
    @Test
    public void withdrawEquivalance() {
        testAccount.withdraw(300);
        assertEquals(200, testAccount.getBalance(),1e-4);
    }
}
```

## AssertThrows
- Used to verify that a specific exception is thrown.

*example - expect `RuntimeException`*

```Java
    @Test
    public void withdrawInsufficientFundsException() {
        assertThrows(RuntimeException.class, () -> testAccount.withdraw(600));
        assertEquals(500, testAccount.getBalance(),1e-4);
    }
```

## The Importance of Rolling Back
- Ensure exceptions do not create side effects (e.g., adding votes when exception occurs).
- Explicitly check preconditions before modifying state to prevent such bugs.