# ⚡ Async Library

*Elegant, non-blocking coroutine management for StarfallEx.*

Async transforms "Callback Hell" into clean, linear logic.
It allows you to run long-tail tasks (like HTTP requests or heavy iterations) without freezing the chip or nesting five levels of functions.

## 🚀 The Core Workflow

The power of this library lies in the **Async/Await** pattern.

```lua
-- 1. Define an async process
local fetchCurrentTemperature = async( function()
    print( "Fetching temperature..." )

    local response = await( http.getAsync( "https://api.example.com/temperature" ) )
    if response.code ~= 200 then
        print( "Failed to fetch temperature: " .. response.content )
    end

    print( "Temperature: " .. response.content )
end )

-- 2. Fire and forget it
fetchCurrentTemperature()
```

## 🛠️ The Reference

`async( func, trigger? )`
Wraps a function so that calling it spawns a `Thread` instead of blocking. 
- `func`: The function you want to run asynchroneously.
- `trigger`: (Optional) Defines what "wakes" the thread to keep moving. Defaults to `runs.onHook( "tick" )`.

`await( thread )`
Pauses the current async process until the target `thread` finishes, then returns that thread's values.
- `thread` : `Thread` The thread you want to wait for.

> [!WARNING]
> **Context Matters:** `await` can only be used inside a function that was wrapped with `async`.
> Using it in a standard hook or callback will throw an error.

`wait( ms )`
A cleaner way to pause. Suspends execution for `ms` milliseconds without stopping other scripts.
- `ms` : The time to wait for in milliseconds.

`yield()`
A helper for yielding from the current thread. (Equivalent to `coroutine.yield()`)

`waitFor( condition )`
Suspends the current thread until a condition is met (truthful).
- `condition` : `function` A function that, upon returning a truthful value, will resume the thread.

## Resumes & Triggers

| Trigger   | Usage                     | Description                           |
| --------- | ------------------------- | ------------------------------------- |
| **Hook**  | `runs.onHook( "render" )` | Resumes when a hook is fired.         |
| **Timer** | `runs.onTimer( 0.5 )`     | Resumes on an interval in seconds.    |
| **Net**   | `runs.onNet( "Msg" )`     | Resumes when receiving a net message. |

## 💡 Advanced Patterns

### Concurrent Tasks

You can start multiple tasks at once and await them later.

```lua
local thread1 = http.getAsync( url1 )
local thread2 = http.getAsync( url2 )

-- Both requests are now running at the same time!
local response1 = await( thread1 )
local response2 = await( thread2 )
```

## 🧪 Why use ths library?
1. **Zero latency:** When a child thread finishes, it "kicks" the parent immediately. No 1-tick delay.
2. **Auto-Cleanup:** Hooks and timers are automatically destroyed when the thread dies. No memory leaks.
3. **Clean Code:** Your logic reads from top-to-bottom, exactly how it executes.
