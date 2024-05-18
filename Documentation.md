# CustomDataStructures

`CustomDataStructures` provides a collection that maintains a set of elements, allowing access to each element by any of its properties marked as keys.

## Classes

### InterlinkedCollection<T>

Represents a collection that maintains a set of elements, allowing access to each element by any of its properties marked as keys.

#### Properties

- **T this[object key]**: Gets the element in the collection associated with the specified key.

#### Methods

- **Add(T data)**: Adds an element to the collection. All properties of the element marked with the `CanBeKeyAttribute` are used as keys to access the element.
  - **Parameters**:
    - `data`: The element of type `T` to add to the collection.
  - **Exceptions**:
    - `ArgumentNullException`: Thrown if `data` is null.
    - `Exception`: Thrown if a property does not have the `CanBeKeyAttribute`, if a property is not writable, or if a key is not unique.

- **UpdateSingleValue(object key, string propertyName, object newValue)**: Updates the value of a single property of an element in the collection.
  - **Parameters**:
    - `key`: The key of the element to update.
    - `propertyName`: The name of the property to update.
    - `newValue`: The new value for the property.
  - **Exceptions**:
    - `KeyNotFoundException`: Thrown if the element with the specified key is not found.
    - `ArgumentException`: Thrown if `propertyName` is null or empty.
    - `Exception`: Thrown if the property is not found, does not have the `CanBeKeyAttribute`, or if the key is not unique.

- **IEnumerator<T> GetEnumerator()**: Returns an enumerator that iterates through the collection.

### Attributes

#### CanBeKeyAttribute

Specifies that a property of an object can be used as a key in an `InterlinkedCollection`.

- **Properties**:
  - `bool CanBeKey { get; }`: Indicates whether the property can be a key.

- **Constructors**:
  - `CanBeKeyAttribute(bool canBeKey)`: Initializes a new instance of the attribute with the specified value.

## Example Usage

```csharp
using System;
using CustomDataStructures;

public class ExampleDataSet
{
    [CanBeKey(true)]
    public string Name { get; set; }

    [CanBeKey(true)]
    public int Age { get; set; }

    [CanBeKey(false)]
    public bool IsNormal { get; set; }
}

public class Program
{
    public static void Main()
    {
        var collection = new InterlinkedCollection<ExampleDataSet>();

        var data1 = new ExampleDataSet { Name = "Alice", Age = 30, IsNormal = true };
        var data2 = new ExampleDataSet { Name = "Bob", Age = 25, IsNormal = false };

        collection.Add(data1);
        collection.Add(data2);

        var retrievedByName = collection["Alice"];
        Console.WriteLine($"Retrieved by Name: {retrievedByName.Name}, Age: {retrievedByName.Age}, IsNormal: {retrievedByName.IsNormal}");

        var retrievedByAge = collection[25];
        Console.WriteLine($"Retrieved by Age: {retrievedByAge.Name}, Age: {retrievedByAge.Age}, IsNormal: {retrievedByAge.IsNormal}");

        collection.UpdateSingleValue("Alice", "Age", 31);
        var updatedData = collection["Alice"];
        Console.WriteLine($"Updated Age: {updatedData.Name}, Age: {updatedData.Age}, IsNormal: {updatedData.IsNormal}");
    }
}
