# CustomDataStructures

## Classes

### ObservableConcurrentQueue<T>

Represents a thread-safe, observable concurrent queue. Allows enqueueing items from any thread and processes them in a separate thread. Enables additional processing through an event mechanism.

#### Events

- **event Action<T> AdditionalProcessing**: An event that triggers additional processing, such as logging or debugging.

#### Constructors

- **ObservableConcurrentQueue(ProcessItemDelegate processItem)**: Initializes a new instance of the queue with the specified processing function.
  - **Parameters**:
    - `processItem`: The main processing function for the queue items.
  - **Exceptions**:
    - `ArgumentNullException`: Thrown if `processItem` is null.

#### Methods

- **void Enqueue(T item)**: Adds an item to the queue and signals the worker thread for processing.
  - **Parameters**:
    - `item`: The item to add to the queue.

- **void Stop()**: Stops the processing thread but ensures the worker thread finishes all tasks.

- **void Dispose()**: Disposes the semaphore and stops the processing thread.

## Example Usage

```csharp
using System;
using CustomDataStructures;
using System.Threading;

public class Program
{
    public static void Main()
    {
        var queue = new ObservableConcurrentQueue<string>(ProcessItem);
        queue.AdditionalProcessing += LogItem;

        queue.Enqueue("Item 1");
        queue.Enqueue("Item 2");

        Thread.Sleep(1000); // Give some time for processing

        queue.Stop();
        queue.Dispose();
    }

    private static void ProcessItem(string item)
    {
        Console.WriteLine($"Processing: {item}");
    }

    private static void LogItem(string item)
    {
        Console.WriteLine($"Logged: {item}");
    }
}
