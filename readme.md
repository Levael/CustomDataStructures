# CustomDataStructures

`CustomDataStructures` provides advanced data structures for managing and processing collections of elements in a concurrent environment.

## Classes

### InterlinkedCollection<T>

A collection that maintains a set of elements, allowing access to each element by any of its properties marked as keys.

#### Features
- Access elements by any property marked with the `CanBeKeyAttribute`.
- Add elements with multiple key properties.
- Update individual properties of elements.

### ObservableConcurrentQueue<T>

A thread-safe, observable concurrent queue that allows enqueueing items from any thread and processes them in a separate thread. Enables additional processing through an event mechanism.

#### Features
- Enqueue items from any thread.
- Process items in a separate worker thread.
- Trigger additional processing events for each item.
