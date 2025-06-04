Global state makes program state unpredictable: you can never know the exact result of any function execution that uses global state because the state can be changed by any other function at any moment. It complicates testing and can lead to critical errors or data loss.

It is never obvious what part of the global state is needed or changed by an exact function, or which functions need or change which part of the state.

It increases coupling of components, but since it's impossible to get rid of it at all, the good approach is to restrict global state to a single object that wraps all the others, and which must never be referenced. If a particular object needs a particular state, then it can obtain it as an argument of its constructor or by a setter method. This is known as *Dependency Injection*.
