Reference: https://google.github.io/googletest/gmock_for_dummies.html



### NOT YET TEST ###
Example
### Summary of Section 2: Getting Started

#### Simple Example
1. **Creating a Mock Class**:
    - Define a mock class by inheriting from the interface you want to mock.
    - Use the `MOCK_METHOD` macro to declare mock methods.
    ```cpp
    class MockFoo : public Foo {
    public:
        MOCK_METHOD(int, Bar, (int x), (override));
    };
    ```
2. **Setting Expectations**:
    - Set expectations on the mock object using the `EXPECT_CALL` macro.
    - Specify the method to be called, argument matchers, and the expected number of calls.
    ```cpp
    MockFoo mock;
    EXPECT_CALL(mock, Bar(5)).Times(1);
    ```
3. **Running the Test**:
    - Call the method and verify the behavior.
    ```cpp
    mock.Bar(5); // call method Bar() of mock object with parameter 5
    ```
### Summary of Section 3: Using Matchers

Matchers are used to specify criteria for arguments in mock method calls. They make tests more flexible and readable.

#### Basic Matchers
1. **Equality Matcher**:
   - Checks if the argument equals a specific value.
    ```cpp
    EXPECT_CALL(mock, Bar(Eq(5)));
    ```

3. **Range Matchers**:
   - Check if the argument is within a range.
    ```cpp
    EXPECT_CALL(mock, Bar(Ge(5)));
    EXPECT_CALL(mock, Bar(Le(10)));
    ```

5. **Composite Matchers**:
   - Combine multiple matchers for complex conditions
   ```cpp
    EXPECT_CALL(mock, Bar(AllOf(Ge(5), Le(10))));
    ```


### Summary of Section 4: Setting Expectations
#### Basic Expectations
- Use `EXPECT_CALL` to set expectations for mock method calls.
    ```cpp
    EXPECT_CALL(mock, Bar(5)).Times(1); // call one times with parameter = 5
    ```
#### Cardinalities
- Specify how many times a method should be called: `Times(AtLeast(1))`, `Times(Exactly(2))`.
    ```cpp
    EXPECT_CALL(mock, Bar(5)).Times(AtLeast(1)); // call n >=1 with parameter = 5
    ```
#### Repeated Expectations
- Expectations can be set to allow methods to be called multiple times.
    ```cpp
    EXPECT_CALL(mock, Bar(_)).Times(AnyNumber()); // call n>=0 with any parameter = 5
    ```
#### Default Actions
- Define what should happen if no expectation is set.
    ```cpp
    ON_CALL(mock, Bar(_)).WillByDefault(Return(0)); // Use case ????, always return the same value
    ```
  
Here are some of the optional methods available in Google Mock for setting expectations and assertions:

1. **Times**: Specifies the number of times a method is expected to be called.
   ```cpp
   EXPECT_CALL(mockObj, methodName())
       .Times(2);  // Expected to be called exactly twice
   ```

2. **With**: Specifies matchers for the arguments of a method call.
   ```cpp
   EXPECT_CALL(mockObj, methodName(_, 5))
       .With(Args(/* specific arguments */)); // ?????????????
   ```

3. **WillOnce / WillRepeatedly / WillOnce(Return(value))**: Defines the actions to take when a method is called. `WillOnce` specifies the action for the next call, `WillRepeatedly` specifies for all subsequent calls, and `WillOnce(Return(value))` specifies returning a value for a single call.
   ```cpp
   EXPECT_CALL(mockObj, methodName())
       .WillOnce(Return(1))
       .WillRepeatedly(Return(2)); // At least one times,
   ```

4. **AtLeast / AtMost**: Specifies minimum and maximum number of times a method should be called.
   ```cpp
   EXPECT_CALL(mockObj, methodName())
       .AtLeast(2)
       .AtMost(5); // n>=2 n<=5
   ```

5. **DoAll**: Executes a list of actions in sequence.
   ```cpp
   EXPECT_CALL(mockObj, methodName())
       .WillOnce(DoAll(Action1(), Action2()));
   ```

6. **Times / After**: Specifies the order and frequency of method calls.
   ```cpp
   InSequence seq;
   EXPECT_CALL(mockObj, method1())
       .Times(2);
   EXPECT_CALL(mockObj, method2())
       .After(mockObj, method1())
       .Times(1);
   ```

These methods provide flexibility in defining the expected behavior of mock objects during testing. Adjust them according to your specific testing needs and scenarios.

In Google Mock, `Return` and `ReturnRef` are actions that specify what value or reference a mock method should return when called. Here’s how they differ:

1. **Return**:
   - **Usage**: `Return(value)` specifies that the mock method should return the given value when called.
   - **Example**:
     ```cpp
     EXPECT_CALL(mockObj, methodName())
         .WillOnce(Return(42));
     ```
     This sets up `methodName` to return `42` when it is called once during the test.

2. **ReturnRef**:
   - **Usage**: `ReturnRef(variable)` specifies that the mock method should return a reference to the provided variable.
   - **Example**:
     ```cpp
     int value = 10;
     EXPECT_CALL(mockObj, methodName())
         .WillOnce(ReturnRef(value));
     ```
     This sets up `methodName` to return a reference to the variable `value`. Any changes made to the returned reference during the mock method call will affect `value` directly.

### Key Differences:

- **Return** returns a copy of the specified value, so changes to the return value do not affect the original variable.
- **ReturnRef** returns a reference to the specified variable, allowing changes made to the return value to directly affect the original variable.

These actions provide flexibility in defining mock method behaviors, especially when dealing with methods that return values or references in your testing scenarios.

In Google Mock, when setting expectations on how many times a mock method should be called, you have several options to specify the expected number of calls. Here’s an explanation of each:

1. **AnyNumber()**:
   - Specifies that the method can be called any number of times, including zero times.
   - Example:
     ```cpp
     EXPECT_CALL(mockObj, methodName())
         .Times(AnyNumber());
     ```

2. **AtLeast(n)**:
   - Specifies that the method should be called at least `n` times.
   - Example:
     ```cpp
     EXPECT_CALL(mockObj, methodName())
         .Times(AtLeast(3));
     ```

3. **AtMost(n)**:
   - Specifies that the method should be called at most `n` times.
   - Example:
     ```cpp
     EXPECT_CALL(mockObj, methodName())
         .Times(AtMost(5));
     ```

4. **Between(m, n)**:
   - Specifies that the method should be called between `m` and `n` times (inclusive).
   - Example:
     ```cpp
     EXPECT_CALL(mockObj, methodName())
         .Times(Between(2, 4)); // n>=2 and n<=4
     ```

5. **Exactly(n)**:
   - Specifies that the method should be called exactly `n` times.
   - Example:
     ```cpp
     EXPECT_CALL(mockObj, methodName())
         .Times(Exactly(2)); // More common, .Times(2)
     ```

6. **Times(n)**:
   - This is not a method itself, but it's used in conjunction with the above methods to specify the exact number of times a method should be called.
   - Example:
     ```cpp
     EXPECT_CALL(mockObj, methodName())
         .Times(AtLeast(2))
         .Times(AtMost(5));
     ```

These methods provide fine-grained control over how many times a mock method should be invoked during testing, allowing you to accurately specify and verify the behavior of your mocked objects.
