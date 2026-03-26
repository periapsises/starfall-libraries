# Async Library

The **Async Library** is essentially a wrapper for coroutines that streamline the creation and resuming process.

## How it works

Asynchroneous functions are functions that have been wrapped using `async()` to make them create a new `Thread` upon being called.  
These threads will automatically be resumed depending on the `Trigger` provided or every `Tick` by default. 

Threads may take time to complete but you can spawn multiple threads at once.  
If you need to wait for one to finish, the `await` function can be used.

## Reference

### Type: `AsyncFunction`
Can be called like a normal function with arguments but always returns a `Thread`.

### Type: `Thread`
Represents a running coroutine. Can be awaited.

### Function: `async( func, trigger? ): AsyncFunction`
Converts a function into an async equivalent.  
- `func` : `function` The function to convert.
- `trigger` : `function` A trigger function to control what resumes and clears the threads.
- `returns` : `AsyncFunction` The wrapped function.

### Function: `await( thread ): ...`
Waits for a thread to finish and returns the values from the thread.
- `thread` : `Thread` The thread to wait for.
- `returns` : `...` The return values from the thread.

> [!WARNING]
> The `await` function should only be used within an asynchroneous thread and *not* the main thread.

### Function: `wait( time )`
Yields the current thread until the specified amount of time has passed.
- `time` : `number` The time to wait for in milliseconds.

### Function: `yield()`
Helper for yielding from the coroutine. (Equivalent to `coroutine.yield()`)

### Function: `waitFor( condition )`
Yields the current thread until the condition is met (truthful).
- `condition` : `function` A function that, upon returning a truthful value, will resume the thread.

## Trigger reference

The following are the default triggers that can be used to specify how threads resume.

**Hook event**: `runs.onHook( event )`  
Resumes the thread whenever the specified event is triggered.

**Timer**: `runs.onTimer( time )`  
Resumes the thread every `time` seconds.

**Net message**: `runs.onNet( identifier )`
Resumes the thread when the specified net message is received.

> [!NOTE]
> Adding a receiver to the same net identifier will prevent the thread from being resumed.
