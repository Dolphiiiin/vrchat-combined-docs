# data-containers Documentation

This document contains automatically collected documentation with metadata headers for each section.



---

## Document: byte-and-bit-operations.md

Path: /worlds/udon/data-containers/byte-and-bit-operations.md
---
title: "Byte and Bit Operations"
slug: "byte-and-bit-operations"
hidden: false
sidebar_position: 10
createdAt: "2024-02-26T18:00:00.000Z"
updatedAt: "2024-02-26T18:00:00.000Z"
---
# Byte and Bit Operations

:::info

This page describes lower level concepts of working with binary data and is intended for advanced creators.
:::

You can use the `Bitcast` method on DataTokens to do value-preserving type changes ("unsafe casts") on primitive data types in Udon.

Some standard C# classes for operating on raw binary data are also available, including `BitConverter` and `Buffer`.

## Example Code

```csharp title="Byte and Bit Operations Example, Basic Serializer"
using System;
using System.Text;
using UdonSharp;
using UnityEngine;
using VRC.SDK3.Data;

public class BitConverterExample : UdonSharpBehaviour
{
    void Start()
    {
        //Create test data
        int originalInt = 63;
        double originalDouble = 734531.433d;
        string originalString = "Test string";
        float[] originalFloatArray = new float[] { 543, 12.6f, 63.1231f };

        //Serialize and then deserialize
        byte[] serialized = Serialize(originalInt, originalDouble, originalString, originalFloatArray);
        Deserialize(serialized, out int newInt, out double newDouble, out string newString, out float[] newFloatArray); 
        
        //Print the results to see if everything matches
        Debug.Log($"{originalInt} - {newInt}");
        Debug.Log($"{originalDouble} - {newDouble}");
        Debug.Log($"{originalString} - {newString}");
        Debug.Log($"{originalFloatArray.Length} - {newFloatArray.Length}");
        for (int i = 0; i < originalFloatArray.Length && i < newFloatArray.Length; i++)
        {
            Debug.Log($"{originalFloatArray[i]} - {newFloatArray[i]}");
        }

        //For individual values we can also use DataToken Bitcasting to get bit access
        double doubleValue = 123.456d;
        DataToken doubleToken = new DataToken(doubleValue);
        //We used ulong because it has the same size as a double (8 bytes)
        DataToken ulongToken = doubleToken.Bitcast(TokenType.ULong);
        DataToken resultDoubleToken = ulongToken.Bitcast(TokenType.Double);
        Debug.Log($"{doubleToken} - 0x{ulongToken:02X} - {resultDoubleToken}");
    }

    /// <summary>
    /// An example function which serializes a pre-determined set of data into a byte array
    /// </summary>
    /// <param name="intValue">Integer which will be encoded into the output</param>
    /// <param name="doubleValue">Double which will be encoded into the output</param>
    /// <param name="stringValue">String which will be encoded into the output</param>
    /// <param name="floatArrayValues">Float array which will be encoded into the output</param>
    /// <returns></returns>
    byte[] Serialize(int intValue, double doubleValue, string stringValue, float[] floatArrayValues)
    {
        int size = 0;
        byte[] intBytes = BitConverter.GetBytes(intValue); //Convert int to bytes
        size += intBytes.Length;
        
        byte[] doubleBytes = BitConverter.GetBytes(doubleValue); //Convert int to bytes
        size += doubleBytes.Length;
        
        byte[] stringBytes = Encoding.UTF8.GetBytes(stringValue); //Convert string to bytes
        size += stringBytes.Length;
        Debug.Log($"String byte length {stringBytes.Length}");
        
        byte[] stringLengthBytes = BitConverter.GetBytes(stringBytes.Length); //Convert string length to bytes
        size += stringLengthBytes.Length;
        
        byte[] floatArrayLengthBytes = BitConverter.GetBytes(Buffer.ByteLength(floatArrayValues));
        size += floatArrayLengthBytes.Length;
        
        //It is not necessary to convert the float array into a byte array because we can BlockCopy it directly
        size += Buffer.ByteLength(floatArrayValues);

        byte[] output = new byte[size]; //Allocate an array of the correct size that should fit all of our items
        int offset = 0;

        Buffer.BlockCopy(intBytes, 0, output, offset, intBytes.Length); //Write int - this should take up 4 bytes
        offset += intBytes.Length; //Increment offset so the next item can write to the correct location
        
        Buffer.BlockCopy(doubleBytes, 0, output, offset, doubleBytes.Length); //Write double - this should take up 8 bytes
        offset += doubleBytes.Length;
        
        Buffer.BlockCopy(stringLengthBytes, 0, output, offset, stringLengthBytes.Length); //Write the length of the string so the decoder knows how much to decode - this should take up 4 bytes
        offset += stringLengthBytes.Length;

        Buffer.BlockCopy(stringBytes, 0, output, offset, stringBytes.Length); //Write string - this is variable, which is why we need to encode the length of the string above
        offset += stringBytes.Length;

        Buffer.BlockCopy(floatArrayLengthBytes, 0, output, offset, floatArrayLengthBytes.Length); //Write the length of the float array so the decoder knows how much to decode - this should take up 4 bytes
        offset += floatArrayLengthBytes.Length;
        
        Buffer.BlockCopy(floatArrayValues, 0, output, offset, Buffer.ByteLength(floatArrayValues)); //Write float array - this can be done directly without a byte array conversion because it's already an Array
        offset += Buffer.ByteLength(floatArrayValues);
        
        Debug.Log($"Encoded data in {output.Length} bytes");
        return output;
    }

    /// <summary>
    /// An example function which deserializes a pre-determined set of data described in the Serialize function above
    /// </summary>
    /// <param name="input">Input bytes - must be formatted in the expected manner by the Serialize function above</param>
    /// <param name="intValue">Output int value deserialized from the data inside the input</param>
    /// <param name="doubleValue">Output double value deserialized from the data inside the input</param>
    /// <param name="stringValue">Output string value deserialized from the data inside the input</param>
    /// <param name="floatArrayValues">Output float array value deserialized from the data inside the input</param>
    /// <returns></returns>
    bool Deserialize(byte[] input, out int intValue, out double doubleValue, out string stringValue, out float[] floatArrayValues)
    {
        int offset = 0;
        
        intValue = BitConverter.ToInt32(input, offset);
        offset += 4; //Increment the offset so the next item reads from the correct location. Ints should be 4 bytes long
        
        doubleValue = BitConverter.ToDouble(input, offset);
        offset += 8; //Doubles should be 8 bytes long
        
        int stringLength = BitConverter.ToInt32(input, offset);
        offset += 4; //String length is an int, which should be 4 bytes long
        
        Debug.Log($"Decoding string length {stringLength} at offset {offset} for buffer length {input.Length}");
        stringValue = Encoding.UTF8.GetString(input, offset, stringLength);
        offset += stringLength; //Strings are variable length

        int floatArrayByteLength = BitConverter.ToInt32(input, offset);
        offset += 4; //Float array length is an int, which should be 4 bytes long
        
        floatArrayValues = new float[floatArrayByteLength / 4]; //Create a new float array of the correct size to receive the data
        Buffer.BlockCopy(input, offset, floatArrayValues, 0, floatArrayByteLength); //Copy the data from the input into the float array
        
        return true;
    }
}
```

---

## Document: data-dictionaries.md

Path: /worlds/udon/data-containers/data-dictionaries.md
---
title: "Data Dictionaries"
slug: "data-dictionaries"
hidden: false
createdAt: "2023-04-24T23:48:21.052Z"
updatedAt: "2023-05-04T21:39:11.809Z"
---
# Data Dictionaries

Data Dictionaries store [Data Tokens](/worlds/udon/data-containers/data-tokens) by key-value pair, similarly to [C# Dictionaries](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2?view=net-7.0). Most Data Dictionary functions are just wrappers for the underlying C# dictionary, so the C# dictionary documentation also applies if you are looking for more specific details.

Both the keys and the values of a Data Dictionary are Data Tokens. This means that you can effectively use anything for your keys. However, if you intend to serialize to [VRCJSON](/worlds/udon/data-containers/vrcjson), only string keys are supported.

If you are using UdonSharp, include the `using VRC.SDK3.Data;` directive to use data dictionaries.

## Properties

| Property | Result                                       |
| -------- | -------------------------------------------- |
| Count    | Get the number of elements in the dictionary |

## Functions

| Function         | Input                             | Output                         | Result                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ---------------- | --------------------------------- | ------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Add              | DataToken key, DataToken value    |                                | Adds the value at the specified key. The entire purpose of this function that sets it apart from SetValue is that an exception will be thrown if the key already exists. This is useful for initialization because it will cause a compile error, but it's not recommended for normal usage where it could cause a runtime error and halt your UdonBehaviour.                                                                         |
| Clear            |                                   |                                | Removes all keys and values from this dictionary                                                                                                                                                                                                                                                                                                                                                                                       |
| ContainsKey      | DataToken key                     | bool result                    | Returns true if the specified key exists on this dictionary.                                                                                                                                                                                                                                                                                                                                                                           |
| ContainsValue  | DataToken key                     | bool result                    | Returns true if the specified value exists on this dictionary.                                                                                                                                                                                                                                                                                                                                                                         |
| DeepClone      |                                   | DataDictionary result          | Clones the DataDictionary into a new DataDictionary that contains all the same values. Unlike ShallowClone, deep clone means that it will recursively navigate inside each DataList or DataDictionary and copy their contents as well. Items with the TokenType "Reference" will maintain the same reference as the original and not be deep cloned, which includes arrays.                                                            |
| GetKeys          |                                   | DataList keys                  | Returns a [Data List](/worlds/udon/data-containers/data-lists) of all keys that exist in this Data Dictionary. **Use this to iterate over all items in a Data Dictionary in a for loop.**                                                                                                                                                                                                                                    |
| GetValues      |                                   | DataList values                | Returns a [Data List](/worlds/udon/data-containers/data-lists) of all values that exist in this Data Dictionary.                                                                                                                                                                                                                                                                                                             |
| Remove           | DataToken key                     | bool success                   | Removes a specific key from this dictionary. Returns true if anything was successfully removed.                                                                                                                                                                                                                                                                                                                                        |
| Remove           | DataToken key                     | bool success, DataToken value  | Removes a specific key from this dictionary. Returns true if anything was successfully removed. If successful, copies the value that was removed into the `out DataToken`.                                                                                                                                                                                                                                                             |
| SetValue         | DataToken key, DataToken value    |                                | Sets the value at the specified key. If that key does not exist, a new one will be added.                                                                                                                                                                                                                                                                                                                                              |
| ShallowClone   |                                   | DataDictionary result          | Clones the DataDictionary into a new DataDictionary that contains all the same values. Unlike DeepClone, this means that if the DataDictionary contains other DataLists and DataDictionaries, those will still be the same reference.                                                                                                                                                                                                  |
| TryGetValue      | DataToken key                     | bool success, DataToken output | Gets a token from the specified key and puts it in the `out DataToken`. Returns true if the retrieval was successful, and false if it was not. Failing to retrieve a value will put a DataError in the `out DataToken` instead of a result.                                                                                                                                                                                            |
| TryGetValue      | DataToken key, TokenType expected | bool success, DataToken output | Gets a token from the specified key and puts it in the `out DataToken`. Returns true if the retrieval was successful, and false if it was not. Failing to retrieve a value will put a DataError in the `out DataToken` instead of a result. This version of `TryGetValue` includes a TokenType, which means it will do an automatic type check for you. If the type does not match, it will return false with `DataError.TypeMismatch` |

 Note that calling functions which affect or look at all values such as `ContainsValue`, `ShallowClone`, and `GetValues` on a Data Dictionary generated from Json will parse all top-level values that have not already been parsed, which may be expensive with many values. Once they are parsed, future operations will be cheaper.

## Getting a value from a DataDictionary

There are several different ways to get a value out of a dictionary. Each one has it's own use case, and it is up to you to choose which one you want to use.

- [TryGetValue](/worlds/udon/data-containers/data-dictionaries#trygetvalue)
- [TryGetValue with TokenType](/worlds/udon/data-containers/data-dictionaries#trygetvalue-with-tokentype)
- [Shorthand bracket syntax](/worlds/udon/data-containers/data-dictionaries#shorthand-bracket-syntax)

### TryGetValue

If you want to get a value out of a dictionary safely, but you don't care what type exists at that location, it is recommended to use `TryGetValue`. This is a function that returns true or false depending on whether or not getting the value was successful. It is intended to put this inside of the conditions for an `if` or a `branch` so that it is clear what happens when it succeeds and what happens when it fails.

```csharp title="Example of TryGetValue"
if (dictionary.TryGetValue("key", out DataToken value)) {
    Debug.Log($"Success! {value}");
} else {
    Debug.Log($"Failed! {value}");
}
```

If this does fail, the DataToken you receive is still valid, but rather than containing your data it will contain an [error](/worlds/udon/data-containers/data-tokens#errors).

This method is good for when you want to get some value from some location, but you don't care what it is exactly.

As this function does not have a type check built in, you should pair this function with some form of type checking, whether that be an `if`, `branch`, or `switch`. If you only care about one specific type, then it is recommended to just use the version of TryGetValue with TokenType, which does an automatic type check.

### TryGetValue with TokenType

If you want to get a value from a dictionary and you don't know what type it could be, it is important to do type checks. You could do that yourself in your own code, but that can get messy. Instead, you can use the version of TryGetValue that includes a TokenType. When you do this, it indicates that you only want to retrieve the value if it is the type you expect. Otherwise, it returns false and that can be handled gracefully. 

This method is good for when you want to get a specific value from a specific location, but the data is coming from an outside source so you are not confident that the source has the right data.

```csharp title="Example of TryGetValue with TokenType"
// You could do it this way, but it's a bit ugly
if (dictionary.TryGetValue("key", out DataToken value)) {
    if (value.TokenType == TokenType.DataDictionary)
    {
        Debug.Log($"Success! Matching dictionary has {value.DataDictionary.Count} items");
    }
}

// This approach has a type check built in! It's functionally the same, but streamlined.
if (dictionary.TryGetValue("key", TokenType.DataDictionary, out value)) {
    Debug.Log($"Success! Matching dictionary has {value.DataDictionary.Count} items");
}
```

### Shorthand Bracket syntax

You can also set and get items from a Data Dictionary using bracket syntax such as `dictionary["key"] = "value";` in UdonSharp or `DataDictionary Get Item` node in Udon graph. This method is smaller and easier to use. However, be aware that this is not completely safe and may halt your udonbehaviour if you attempt to perform an invalid operation such as trying to get a value from a key that does not exist. 

This method is good for when you have complete control over your data, can guarantee that it exists, and that it is the type you expect. Otherwise, it is recommended to use some form of `TryGetValue`.

```csharp title="Example of Shorthand Bracket syntax"
dictionary["A"] = 5;
dictionary["B"] = 10;

// This makes the assumption that A and B will always contain integers.
// This is a safe assumption to make since we set them just above in a controlled environment.
// If the data is coming from an external source, we shouldn't make these assumptions!
int sum = dictionary["A"].Int + dictionary["B"].Int;
```

## Initializing a Data Dictionary

In Udonsharp, Data Dictionaries can be initialized in private variables. This allows you to have a pre-existing set of data that is defined before your code runs. This also supports nested dictionaries and anything else that DataTokens support. Here is an example of how you should use this syntax:

```csharp title="Example of initializing a Data Dictionary"
private DataDictionary users = new DataDictionary()
    {
        { "John Doe", new DataDictionary()
            {
                {"email", "johndoe@example.com"},
                {"age", 35},
                {"address", new DataDictionary()
                    {
                        {"street", "123 Main St"},
                        {"city", "Anytown"},
                        {"state", "CA"},
                        {"zip", 12345}
                    }
                }
            }
        },
        { "Jane Smith", new DataDictionary()
            {
                {"email", "janesmith@example.com"},
                {"age", 28},
                {"address", new DataDictionary()
                    {
                        {"street", "456 Elm St"},
                        {"city", "Anytown"},
                        {"state", "CA"},
                        {"zip", 12345}
                    }
                }
            }
        },
        { "Bob Johnson", new DataDictionary()
            {
                {"email", "bobjohnson@example.com"},
                {"age", 42},
                {"address", new DataDictionary()
                    {
                        {"street", "789 Oak St"},
                        {"city", "Anytown"},
                        {"state", "CA"},
                        {"zip", 12345}
                    }
                }
            }
        }
    };
```

At the moment, Udonsharp does not support initializers of this type inside a function. This would be a feature request for Udonsharp.

At the moment, Unity does not serialize DataDictionaries, which means that **this is not recommended for serialized public variables.** It should be used for `private` or `[NonSerialized] public` variables only. This is an addition to the feature that we are still working on.

## Iterating over all entries in a dictionary

Iterating over all the entries in a dictionary is a bit different from a list because a dictionary is not ordered. You can't index directly into a dictionary, you must use a key. To do this, we have the `GetKeys()` function. This function gives you a DataList of all the keys in a dictionary. Once you have that, you can use a for loop to iterate over all the keys and access the value at each key.

```csharp title="Example of iterating over all entries in a dictionary"
// First get all the keys in the dictionary
DataList keys = dictionary.GetKeys();

// For loop over all the keys
for (int i = 0; i < keys.Count; i++)
{
    // Get the key at the current index
    DataToken key = keys[i];
    
    // Access the entry connected to that key
    Debug.Log(dictionary[key].ToString());
}
```

GetKeys may appear to be expensive at first glance, but it is cached so long as a key is not added or removed so it is generally performant to access frequently, aside from the overhead of Udon itself.

If you need to iterate over a dictionary in a sorted manner, a neat trick you can do thanks to this method is to `GetKeys()` then `Sort()` the keys, then go through the for loop. The dictionary itself doesn't have any context of ordering and cannot be sorted, but you can sort the list that you use to access it!

If you want to work with the values of the dictionary separately from the keys, you can also use `GetValues()`. This can be useful for some applications where you explicitly need all the values laid out, but it is worth mentioning that if you are not familiar with dictionaries, this method can be deceptive. Among other reasons, dictionaries have no order, and so you should never rely on a particular item to always be at a particular index when retrieved by `GetValues()`, nor should you expect the index of these items to correctly match with the index found in `GetKeys()`. In most cases, `GetKeys()` is all you need, and `GetValues()` is more of an option for people who need more advanced control over dictionaries.

## Syncing a Data Dictionary with other players over the network

Data Dictionaries cannot be directly synced. However, they can be serialized to/from JSON strings using [VRCJson](/worlds/udon/data-containers/vrcjson). This is the current recommended method of syncing Data Dictionaries with UdonSync. 

One way to do this is to use OnPreSerialization and OnDeserialization to Serialize and Deserialize the json string. Using this method, you won't need to worry about the serialization within the rest of your code, and you can simply set values and forget.

```csharp title="Example of syncing a Data Dictionary with other players over the network"
[UdonSynced]
private string _json = "";
private DataDictionary _dictionary;

public override void OnPreSerialization()
{
    if (VRCJson.TrySerializeToJson(_dictionary, JsonExportType.Minify, out DataToken result))
    {
        _json = result.String;
    }
    else
    {
        Debug.LogError(result.ToString());
    }
}

public override void OnDeserialization()
{
    if(VRCJson.TryDeserializeFromJson(_json, out DataToken result))
    {
        _dictionary = result.DataDictionary;
    }
    else
    {
        Debug.LogError(result.ToString());
    }
}
```

---

## Document: data-lists.md

Path: /worlds/udon/data-containers/data-lists.md
---
title: "Data Lists"
slug: "data-lists"
hidden: false
createdAt: "2023-04-24T23:48:11.427Z"
updatedAt: "2023-04-26T15:18:56.307Z"
---
# Data Lists

Data Lists store [Data Tokens](/worlds/udon/data-containers/data-tokens) by index, similarly to [C# Lists](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1?view=net-7.0). Most Data List functions are just wrappers for the underlying C# list, so the C# list documentation also applies if you are looking for more specific details.

Data Lists can be serialized to/from JSON strings using [VRCJSON](/worlds/udon/data-containers/vrcjson). This is the current recommended method of syncing Data Lists over the network.

If you are using UdonSharp, include the `using VRC.SDK3.Data;` directive to use data lists.

## Properties

| Property | Result                                                                                                                                                                                 |
| -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Capacity | Set or get the capacity of the list. See [C# documentation](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1.capacity?view=net-8.0) for further details. |
| Count    | Get the number of elements in the list                                                                                                                                                 |

## Functions

| Function        | Input                                      | Output                         | Result                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| --------------- | ------------------------------------------ | ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Add             | DataToken                                  |                                | Adds a token at the end of the list                                                                                                                                                                                                                                                                                                                                                                                                           |
| AddRange      | DataList                                   |                                | Adds the values of another Data List at the end of this Data List.                                                                                                                                                                                                                                                                                                                                                                            |
| BinarySearch    | DataToken value                            | int index                      | Uses a binary search algorithm to locate a specific element in the List by comparison. **In order to perform a binary search, the list must be sorted**. See [C# documentation](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1.binarysearch?view=net-8.0) for further details on binary search.                                                                                                               |
| BinarySearch    | DataToken value, int startIndex, int count | int index                      | Performs a binary search within the specified range, starting at the provided index and extending toward the end of the list by the provided count. Note that this function searches by comparison, and will compare dictionaries and lists by count rather than contents. This makes it unsuitable for finding a specific dictionary or list within a list.                                                                                  |
| Clear           |                                            |                                | Removes all values from this list.                                                                                                                                                                                                                                                                                                                                                                                                            |
| Contains        | DataToken value                            | bool result                    | Returns true if the Data List contains the specified value.                                                                                                                                                                                                                                                                                                                                                                                   |
| DeepClone     |                                            | DataList result                | Clones the DataList into a new DataList that contains all the same values. This does do a deep clone, which means that it will recursively navigate inside each DataList or DataDictionary and copy their contents as well. However, it will not look inside other structures such as arrays, and those will still have the same reference to the original.                                                                                   |
| GetRange        | int index, int count                       | DataList output                | Copies a segment of the DataList out to another DataList. Returns false if index or count were out of range.                                                                                                                                                                                                                                                                                                                                  |
| IndexOf       | DataToken item                             | int index                      | Searches for the specified object and returns the zero-based index of the first occurrence within the entire DataList. If not found, returns -1.                                                                                                                                                                                                                                                                                              |
| IndexOf       | DataToken item, int startIndex             | int index                      | Searches for the specified object and returns the zero-based index of the first occurrence within the range of elements in the DataList that extends from the specified index to the last element. If not found, returns -1.                                                                                                                                                                                                                  |
| IndexOf       | DataToken item, int startIndex, int count  | int index                      | Searches for the specified object and returns the zero-based index of the first occurrence within the range of elements in the DataList that starts at the specified index and contains the specified number of elements. If not found, returns -1.                                                                                                                                                                                           |
| Insert          | int index, DataToken input                 | bool success                   | Inserts a token into the middle of the list. All entries above the specified index will be shifted up one. Returns false if index was out of range.                                                                                                                                                                                                                                                                                           |
| InsertRange     | int index, DataList input                  |                                | Inserts a DataList into the middle of the DataList. All entries above the specified index will be shifted up. Returns false if index was out of range.                                                                                                                                                                                                                                                                                        |
| LastIndexOf   | DataToken item                             | int index                      | Searches for the specified object and returns the zero-based index of the last occurrence within the DataList. If not found, returns -1.                                                                                                                                                                                                                                                                                                      |
| LastIndexOf   | DataToken item, int startIndex             | int index                      | Searches for the specified object and returns the zero-based index of the last occurrence within the range of elements in the DataList that extends from the first element to the specified index. If not found, returns -1.                                                                                                                                                                                                                  |
| LastIndexOf   | DataToken item, int startIndex, int count  | int index                      | Searches for the specified object and returns the zero-based index of the last occurrence within the range of elements in the DataList that contains the specified number of elements and ends at the specified index. If not found, returns -1.                                                                                                                                                                                              |
| Remove        | DataToken value                            | bool success                   | Removes the first occurrence of the specified value. Returns true if a matching value was found, returns false if not.                                                                                                                                                                                                                                                                                                                        |
| RemoveAll     | DataToken value                            | bool success                   | Removes all occurrences of the specified value. Returns true if any matching values were found, returns false if not.                                                                                                                                                                                                                                                                                                                         |
| RemoveAt        | int index                                  |                                | Removes the element at the specified index.                                                                                                                                                                                                                                                                                                                                                                                                   |
| RemoveRange     | int index, int count                       |                                | Removes a range of elements from the list.                                                                                                                                                                                                                                                                                                                                                                                                    |
| Reverse         |                                            |                                | Reverses the order of all elements in the list.                                                                                                                                                                                                                                                                                                                                                                                               |
| Reverse         | int index, int count                       |                                | Reverses the order of elements within the range specified, starting at the provided index and extending toward the end of the list by the provided count.                                                                                                                                                                                                                                                                                     |
| SetValue        | int index, DataToken input                 |                                | Sets a DataToken at the specified index.                                                                                                                                                                                                                                                                                                                                                                                                      |
| ShallowClone  |                                            | DataList result                | Clones the DataList into a new DataList that contains all the same values. This does not do a deep clone, which means that if the DataList contains references to other Data Containers, those will still be the same reference.                                                                                                                                                                                                              |
| Sort          |                                            |                                | Sorts all the elements in the list. If all elements are the same type, then they will be sorted by that type's native comparison operation. If a DataList contains multiple different types but those types are all numbers, then it will sort them with a numeric conversion. If a DataList contains multiple different non-numeric types, then it will sort them in this order: `Null, Number, String, DataList, DataDictionary, Reference` |
| Sort          | int index, int count                       |                                | Performs the same operation as Sort but only within the range specified, starting at the provided index and extending toward the end of the list by the provided count.                                                                                                                                                                                                                                                                       |
| ToArray         |                                            | DataToken\[] output            | Converts the DataList into a DataToken array                                                                                                                                                                                                                                                                                                                                                                                                  |
| TrimExcess      |                                            |                                | Sets the capacity to the actual number of elements in the DataList, if that number is less than a threshold value.                                                                                                                                                                                                                                                                                                                            |
| TryGetValue     | int index                                  | DataToken output               | Gets a token from the specified index and puts it in the `out DataToken`. Returns true if successful.                                                                                                                                                                                                                                                                                                                                         |
| TryGetValue     | int index, TokenType expected              | bool success, DataToken output | Gets a token from the specified index and puts it in the `out DataToken`. Returns true if successful. This version of `TryGetValue` includes a TokenType, which means it will do an automatic type check for you. If the type does not match, it will return false with `DataError.TypeMismatch`                                                                                                                                              |

 Note that calling functions which affect or look at all values such as `Contains`, `IndexOf`, and `LastIndexOf` on a Data List generated from Json will parse all top-level values that have not already been parsed, which may be expensive with many values. Once they are parsed, future operations will be cheaper.

## Getting a value from a DataList

There are several different ways to get a value out of a DataList. Each one has it's own use case, and it is up to you to choose which one you want to use.

- [TryGetValue](/worlds/udon/data-containers/data-lists#trygetvalue)
- [TryGetValue with TokenType](/worlds/udon/data-containers/data-lists#trygetvalue-with-tokentype)
- [Shorthand bracket syntax](/worlds/udon/data-containers/data-lists#shorthand-bracket-syntax)

### TryGetValue

If you want to get a value out of a list safely, it is recommended to use `TryGetValue`. This is a function that returns true or false depending on whether or not getting the value was successful. It is intended to put this inside of the conditions for an `if` or a `branch` so that it is clear what happens when it succeeds and what happens when it fails.

```csharp title="Example of TryGetValue"
if (list.TryGetValue(0, out DataToken value))
{
    Debug.Log($"Success! {value}");
}
else
{
    Debug.Log("Failed! {value}");
}

```

If this does fail, the DataToken you receive is still valid, but rather than containing your data it will contain an [error](/worlds/udon/data-containers/data-tokens#errors).

This method is good for when you want to get some value from some location, but you don't care what it is exactly.

As this function does not have a type check built in, you should pair this function with some form of type checking, whether that be an `if`, `branch`, or `switch`. If you only care about one specific type, then it is recommended to just use the version of TryGetValue with TokenType, which does an automatic type check.

### TryGetValue with TokenType

If you want to get a value from a list and you don't know what type it could be, it is important to do type checks. You could do that yourself in your own code, but that can get messy. Instead, you can use the version of TryGetValue that includes a TokenType. When you do this, it indicates that you only want to retrieve the value if it is the type you expect. Otherwise, it returns false and that can be handled gracefully. 

This method is good for when you want to get a specific value from a specific location, but the data is coming from an outside source so you are not confident that the source has the right data.

```csharp title="Example of TryGetValue with TokenType"
// You could do it this way, but it's a bit ugly
if (list.TryGetValue(0, out DataToken value)) {
    if (value.TokenType == TokenType.DataDictionary)
    {
        Debug.Log($"Success! Matching dictionary has {value.DataDictionary.Count} items");
    }
}

// This approach has a type check built in! It's functionally the same, but streamlined.
if (list.TryGetValue(0, TokenType.DataDictionary, out value)) {
    Debug.Log($"Success! Matching dictionary has {value.DataDictionary.Count} items");
}
```

### Shorthand Bracket syntax

You can also set and get items from a DataList using bracket syntax such as `list[5] = "value";` in UdonSharp or `DataList Get Item` node in Udon graph. This method is smaller and easier to use. However, be aware that this is not completely safe and may halt your udonbehaviour if you attempt to perform an invalid operation. You should only use this if you have complete control over the data and can guarantee that it exists and is the type you expect. Otherwise, it is recommended to use some form of `TryGetValue`.

```csharp title="Example of Shorthand Bracket syntax"
list[0] = 5;
list[1] = 10;

// This makes the assumption that index 0 and 1 will always contain integers.
// This is a safe assumption to make since we set them just above in a controlled environment.
// If the data is coming from an external source, we shouldn't make these assumptions!
int sum = list[0].Int + list[1].Int;
```

## Initializing A Data List

In Udonsharp, Data Lists can be initialized in private variables. This allows you to have a pre-existing set of data that is defined before your code runs. This also supports nested dictionaries and anything else that DataTokens support. Here is an example of how you should use this syntax:

```csharp title="Example of initializing a Data List"
private DataList _groceries = new DataList()
{
    "Bananas",
    "Grapes",
    "Milk",
    "Soda",
    "Turkey",
    "Ham",
    "Roast Beef"
}
```

At the moment, Udonsharp does not support initializers of this type inside a function. This would be a feature request for Udonsharp.

At the moment, Unity does not serialize DataLists, which means that **this is not recommended for serialized public variables.** It should be used for `private` or `[NonSerialized] public` variables only. This is an addition to the feature that we are still working on.

## Syncing a Data List with other players over the network

Data Lists cannot be directly synced. However, they can be serialized to/from JSON strings using [VRCJson](/worlds/udon/data-containers/vrcjson). This is the current recommended method of syncing Data Lists with UdonSync.

One way to do this is to use OnPreSerialization and OnDeserialization to Serialize and Deserialize the json string. Using this method, you won't need to worry about the serialization within the rest of your code, and you can simply set values and forget.

```csharp title="Example of syncing a Data List with other players over the network"
[UdonSynced]
private string _json;
private DataList _list;

public override void OnPreSerialization()
{
    if (VRCJson.TrySerializeToJson(_list, JsonExportType.Minify, out DataToken result))
    {
        _json = result.String;
    }
    else
    {
        Debug.LogError(result.ToString());
    }
}

public override void OnDeserialization()
{
    if(VRCJson.TryDeserializeFromJson(_json, out DataToken result))
    {
        _list = result.DataList;
    }
    else
    {
        Debug.LogError(result.ToString());
    }
}
```

## FAQ

### Why not have a ToArray for each type?

It would be desirable to have a ToArray method for each data type. However, this is not currently feasible due to the lack of support for generics within Udon. While it is technically possible to create individual methods such as ToStringArray, ToFloatArray, ToDoubleArray, and others, this approach would result in excessive bloat to cover every possible type. Additionally, these methods would become deprecated once Udon 2 introduces support for generics. Furthermore, the basic ToArray methods would not provide significant added value. The real advantage would arise from the ability to execute ToArray on an object type, such as ToArray(typeof(Collider)), which would eliminate the need for casting. Nevertheless, supporting a ToArray of all possible objects is not practical, and supporting a ToArray of object specifically would be even worse than working with tokens.

Despite the fact that retrieving values from DataTokens can be a somewhat cumbersome process, they are specifically designed for this purpose and have several utilities to assist with this task.

### Arrays are similar, what's the difference?

Arrays are a similar structure, used for storing lots of values in a sequential order, accessed by index. They are also very simple, and highly efficient at doing _exactly_ that and nothing else. DataLists are a more complex type that can do a lot more. For example, an array has to be initialized with a specific length when it is first created, and you cannot add more items to it unless you create a new array to replace it. But don't be fooled - there are still good reasons to use an array instead of a list.

### When should I use a DataList instead of an array?

Picking a DataList over an array should be done when there is a particular feature you need, so not everything needs to be switched.

- When you want to add new items or remove items to the container dynamically. Arrays cannot do this.
- When you want a single container to contain multiple different types at the same time. Arrays cannot do this.
- When you want a container to contain containers arbitrarily. Arrays can do this, but they have to have a strict depth defined. DataLists can be nested arbitrarily deep however you want.

### When should I use an array instead of a DataList?

- When performance is critical, such as iterating over a container every frame. DataLists may have a very small amount of performance overhead due to pulling the value out of the token.
- When you want to sync the container over the network. DataLists technically support this through JSON if there is a reason you absolutely need a DataList, but that is going to be much more expensive on both performance and bandwidth than normal array syncing.
- When your container only needs to contain one specific type. DataLists can do this of course, but they bypass the strict typing nature of C#. This means that your code editor will be unable to know exactly what type the container contains, and this may allow you to write bugs that would have otherwise been compile errors.
- When you want to contain a type that is not directly supported by Data Tokens. Data Tokens can contain any type through the use of object references and boxing, but it's not ideal. You need to pull the reference out _and_ cast it to your desired type.

---

## Document: data-tokens.md

Path: /worlds/udon/data-containers/data-tokens.md
---
title: "Data Tokens"
slug: "data-tokens"
hidden: false
createdAt: "2023-04-24T23:48:30.309Z"
updatedAt: "2023-04-25T00:01:54.599Z"
---
# Data Tokens

Data Tokens store data. Each token stores one and only one variable. Data Tokens are used in [Data Dictionaries](/worlds/udon/data-containers/data-dictionaries) and [Data Lists](/worlds/udon/data-containers/data-lists).

Data Tokens can contain the following Token Types:

- Null
- Boolean
- SByte
- Byte
- Short
- UShort
- Int
- UInt
- Long
- ULong
- Float
- Double
- String
- Data Lists (Stores other DataTokens)
- Data Dictionaries (Stores other DataTokens)
- Object Reference (Able to store **anything** through boxing, but cannot be serialized)
- Data Errors (An enum that indicates what went wrong)

## Properties

| Property       | Result                                                                                                                                                                                                                                                                                                           |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| TokenType      | Returns the current TokenType of variable that this DataToken contains                                                                                                                                                                                                                                           |
| IsNumber       | Returns true if the DataToken contains any numeric type. Otherwise, returns false.                                                                                                                                                                                                                               |
| IsNull         | Returns true if the value contained within this DataToken is null in any form. Numbers and bools are never null, TokenType.Null is always null, strings check for null but not emptiness, and references use Utilities.IsValid internally to handle players that have left and objects that have been destroyed. |
| Boolean        | Returns a bool if the DataToken contains a bool. Otherwise, throws an exception.                                                                                                                                                                                                                                 |
| Number         | Returns a double if the DataToken contains any numeric type. Otherwise, throws an exception.                                                                                                                                                                                                                     |
| SByte          | Returns an 8-bit signed sbyte if the DataToken contains an sbyte. Otherwise, throws an exception.                                                                                                                                                                                                                |
| Byte           | Returns an 8-bit unsigned byte if the DataToken contains a byte. Otherwise, throws an exception.                                                                                                                                                                                                                 |
| Short          | Returns a 16-bit signed short if the DataToken contains a short, sbyte, or byte. Otherwise, throws an exception.                                                                                                                                                                                                 |
| UShort         | Returns a 16-bit unsigned ushort if the DataToken contains a ushort or byte. Otherwise, throws an exception.                                                                                                                                                                                                     |
| Int            | Returns a 32-bit signed int if the DataToken contains an int, sbyte, byte, short, or ushort. Otherwise, throws an exception.                                                                                                                                                                                     |
| UInt           | Returns a 32-bit unsigned uint if the DataToken contains a uint, byte, or ushort. Otherwise, throws an exception.                                                                                                                                                                                                |
| Long           | Returns a 64-bit signed long if the DataToken contains a long, sbyte, byte, short, ushort, or uint. Otherwise, throws an exception.                                                                                                                                                                              |
| ULong          | Returns a 64-bit unsigned ulong if the DataToken contains a ulong, byte, ushort, or uint. Otherwise, throws an exception.                                                                                                                                                                                        |
| Float          | Returns a 32-bit float if the DataToken contains a float, sbyte, byte, short, ushort, int, uint, long, or ulong. Otherwise, throws an exception.                                                                                                                                                                 |
| Double         | Returns a 32-bit double if the DataToken contains a double or any other numeric type. Otherwise, throws an exception.                                                                                                                                                                                            |
| String         | Returns a string if the DataToken contains a string. Otherwise, throws an exception.                                                                                                                                                                                                                             |
| DataDictionary | Returns a Data Dictionary if the DataToken contains a Data Dictionary. Otherwise, throws an exception.                                                                                                                                                                                                           |
| DataList       | Returns a Data List if the DataToken contains a Data List. Otherwise, throws an exception.                                                                                                                                                                                                                       |
| Reference      | Returns an object reference if the DataToken contains an object reference. Otherwise, throws an exception.                                                                                                                                                                                                       |
| Error          | Returns the error associated with this token. Otherwise, returns DataError.None. Unlike others, accessing this property will never throw an exception. If you attempt to access Error from a token that is not an error, it will simply return DataError.None.                                                   |

## Functions

| Function    | Result                                                                                                                                                                                                                                                                                                                                           |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Bitcast     | Reuse the existing data in the DataToken but change the type. Like reinterpret_cast in C++, or BitConverter in C#. Will truncate the value if the destination type is smaller than the source type, or zero-extend if the destination type is larger than the source type. Only works on primitive types and returns a copy. |
| ToString    | Converts the contents of the token to a string. Unlike accessing the String property, this function will always succeed because it will use the underlying value's ToString                                                                                                                                                                      |
| GetHashCode | Returns a hashcode of the contents of the token. This is mostly used for internal operations of dictionary keys.                                                                                                                                                                                                                                 |
| CompareTo   | Compares this token to another token, returning -1 if the other token is larger, 0 if they are equal, and 1 if the other token is smaller. Containers such as lists and dictionaries will be compared by count. When comparing two tokens that are not the same type and not numerical values, they will use the ordering of the TokenType enum. |

## Creating Data Tokens

### UdonSharp

In UdonSharp, DataTokens can be created "implicitly" which means that when a function asks for a DataToken, you do not need to do `new DataToken(value)`. Instead you can just pass the value in directly and it will create a DataToken for you automatically.

```csharp title="DataToken Creation"
// You could do this
DataToken _explicitFloat = new DataToken(5.3f);
DataToken _explicitInt = new DataToken(5);
DataToken _explicitString = new DataToken("value");
DataToken _explicitBool = new DataToken(true);

// But this is easier and simpler
DataToken _float = 5.3f;
DataToken _Int = 5;
DataToken _String = "value";
DataToken _Bool = true;
```

:::warning

Don't use `nameof()` to implicitly create DataTokens in UdonSharp, or you may run into errors.

:::

### Udon Graph

In Udon Graph, you'll need to use the `DataToken Implicit` or `DataToken Constructor` nodes to create a DataToken with the value inside.  
[IMAGE: data-tokens-7GAcVrY.png]

## Getting values out of a Data Token

Before getting a value out of a DataToken you need to be sure of what type it contains because if you try to pull an incompatible type, it will halt your UdonBehaviour. There are several ways to ensure that the type contained is compatible with what you want to pull out.

- You can check the `DataToken.TokenType` property to get the exact type
- When retrieving a value out of a Data List or Data Dictionary, you can use `TryGetValue` and specify a TokenType. If the TokenType is incorrect, that function will return false.
- You can check the `DataToken.IsNumber` property to get if it is a number. If it is, then you can safely pull the `Number` property which will give you a double upcasted from whichever type it actually was. This may lose precision if the type was `long` or `ulong`.
- Regardless of the type of the token, `ToString` is always a valid option and will never throw errors.

```csharp title="DataToken Retrieval in U#"
// If we know that it's a string, we can safely pull the string out of the token
if (unknownToken.TokenType == TokenType.String)
{
    Debug.Log(unknownToken.String);
}

// We can use IsNumber to see if it's some type of number, even if we don't know which.
if (unknownToken.IsNumber)
{
    Debug.Log(unknownToken.Number);
}

// If we're pulling a value from a container, we can use the version that does its own type check
if (dictionary.TryGetValue("key", TokenType.String, out DataToken value))
{
    Debug.Log(value.String);
}
```

[IMAGE: data-tokens-SqQqE5w.png]

Once you are sure that you have the right type, you can get the value out of the DataToken by accessing value properties such as `DataToken.Float` and `DataToken.Boolean`. Each type has it's own property which can be used to pull that specific type out.

If you have complete control over the data that you're working with, then you can skip all the TokenType checking and just get the value from the token directly. This can save some extra code, but make sure that you're not doing this if the data is coming from an outside source or there is any possibility that the type could be something else.

```csharp title="Example of Shorthand Bracket syntax"
dictionary["A"] = 5;
dictionary["B"] = 10;

// This makes the assumption that A and B will always contain integers.
// This is a safe assumption to make since we set them just above in a controlled environment.
// If the data is coming from an external source, we shouldn't make these assumptions!
int sum = dictionary["A"].Int + dictionary["B"].Int;
```

## Errors

When operations on a Data List or Data Dictionary fail and give a DataToken back, they will produce an error token. Error tokens contain both an enum classifying the type of error, as well as a string that elaborates on the error more specifically. Not all errors will produce a string because there is no need to elaborate further.

If you have an Error token, you can use `DataToken.Error` to get the error enum and `DataToken.String` to get the message. You can also use `DataToken.ToString()` which will automatically combine the enum and the string into a nice complete message, which makes it convenient to simply call `Debug.Log(token)`

An error token can be one of several different things:

| Value            | Meaning                                                                                                                                                                                                            |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| KeyDoesNotExist  | You tried to access a key from a data dictionary but that key does not exist                                                                                                                                       |
| IndexOutOfRange  | You tried to access an index from a data array but that index was either less than 0 or greater than or equal to the count of the array.                                                                           |
| TypeMismatch     | You tried to access a value but the value was not the type you expected. Note that this can only happen if you are using a version of TryGetValue that accepts a type.                                             |
| TypeUnsupported  | The data container had a type that is not supported by the serialization format you tried to use. This can happen if you put reference tokens into a Data Container and then try to serialize it into Json.        |
| ValueUnsupported | The data container had a value that is not supported by the serialization format you tried to use. This can happen if you put NaN or Infinity floats into a Data Container and then try to serialize it into Json. |
| UnableToParse    | The serialized format could not be parsed. This happens if the source Json is invalid.                                                                                                                             |

```csharp title="TryGetValue with TokenType"
if (dictionary.TryGetValue("key", TokenType.Float out DataToken value)) {
    // If TryGetValue succeeds, we can do things with the token
    Debug.Log($"Successfully retrieved value {token.Float}");
} else {
    // If TryGetValue fails, the token will instead be an error
    Debug.Log($"Failed to retrieve value with error {token.Error}");
}
```

[IMAGE: data-tokens-zcqKePv.png]

## FAQ

### What is the difference between String and ToString?

`DataToken.String` and `DataToken.ToString()` are similar but not quite the same, because `DataToken.String` is specifically accessing the string value inside the DataToken, while `DataToken.ToString()` is converting whatever exists into a string. 

As a result, `ToString` is always valid no matter what the DataToken contains and will never halt your UdonBehaviour. If it contains a bool, then it will give you either true or false. If it contains a number, it will create a string representation of that number using `ToString("G", CultureInfo.InvariantCulture)`.

On the other hand, accessing `DataToken.String` is only valid if the DataToken contains a string. If the DataToken contains a float and you attempt to access `DataToken.String`, then an exception will be thrown and your UdonBehaviour will halt. 

DataErrors are unique in that they contain both an Error enum and a string. It is recommended to use `ToString()` on DataErrors simply because `ToString()` will combine the enum and the string together into a single message that contains both the error and the reason for the error.

### Why not have a TryGetValue that gives the value directly, skipping tokens?

It would be beneficial to have a TryGetValue method that directly provides the value, bypassing the need for tokens. This is a valid question as it can be tedious to constantly retrieve the token from the container and then access the value within the container separately. There have been several options considered to simplify this process, and one such solution that has been implemented is to include a built-in type check with TryGetValue.

Another approach that was contemplated involved creating a generic version of TryGetValue where a system type is specified using a T argument. Although UdonSharp supports this approach (at least within static methods), Udon itself does not. Additionally, while this option would be advantageous, it would prevent the return of a DataError through the DataToken in case of an error, resulting in the default value being returned instead. Ultimately, we chose not to implement this approach as it would conceal the error from the user and make it more difficult to identify the problem. Fortunately, users have the ability to create their own solution for generic value retrieval using UdonSharp. This is made possible due to the availability of Generics, Statics, and extension methods within UdonSharp.

---

## Document: index.md

Path: /worlds/udon/data-containers/index.md
# Data Containers

Data Containers store [Data Tokens](/worlds/udon/data-containers/data-tokens) in various different formats, either as a sequential [Data List](/worlds/udon/data-containers/data-lists) or as a key-value pair [Data Dictionary](/worlds/udon/data-containers/data-dictionaries). They are functionally similar to C# Lists and Dictionaries.

DataTokens, in turn, store any other values that you could need. Each token stores one and only one variable, but that could include a whole other DataDictionary or DataList, in order to support nesting.

Additionally, Data Containers are compatible with [VRCJSON](/worlds/udon/data-containers/vrcjson), which allows you to convert data to/from the standard JSON format, which may come from external sources such as [remote string loading](/worlds/udon/string-loading).

import DocCardList from '@theme/DocCardList';

<DocCardList />

---

## Document: vrcjson.md

Path: /worlds/udon/data-containers/vrcjson.md
---
title: "VRCJSON"
slug: "vrcjson"
hidden: false
createdAt: "2023-04-24T23:48:39.230Z"
updatedAt: "2023-04-26T15:25:05.803Z"
---
# VRC JSON

[Data Dictionaries](/worlds/udon/data-containers/data-dictionaries) and [Data Lists](/worlds/udon/data-containers/data-lists) include functions to convert to and from JSON. A Data List is equivalent to a JSON array, and a JSON object is equivalent to a Data Dictionary with string keys.

See [Json documentation](https://www.json.org/json-en.html) for further details on the JSON schema itself. Everything in this page is relating to this particular implementation of the JSON schema.

## JSON functions

| Function                       | Inputs                          | Outputs                        | Result                                                                                                                                                                                                                                                                           |
| ------------------------------ | ------------------------------- | ------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| VRCJson.TryDeserializeFromJson | String input                    | success bool, DataToken result | Creates a DataList or DataDictionary from JSON string input. If successful, this returns true and the result token will be either a DataDictionary or DataList. If not successful, this returns false and puts an error explaining what the issue was in the result token.       |
| VRCJson.TrySerializeToJson     | DataToken input, JsonExportType | success bool, DataToken result | Attempts to convert a DataDictionary or DataList into JSON string output. If successful, this returns true and the result token will be a string with the final Json. If not successful, this returns false and puts an error explaining what the issue was in the result token. |

- Note that in Udon Graph, "VRC" is removed from the beginning of all class names, so you need to search for "Json" to find these functions.
- In UdonSharp, these classes are found in the `VRC.SDK3.Data` namespace.

## Supported types and values

JSON is a small, simple, and strict specification. DataLists and DataDictionaries are capable of supporting a much wider range of configurations, which means that you may face some limitations when going from Data container to JSON. Make sure you are aware of these limitations and avoid using these configurations in situations where you intend to use JSON.

**JSON does not support `Object Reference` Data Tokens.** If you have any object references in your Data containers when you try to serialize to JSON, the TrySerializeToJson function will return false with `DataError.TypeUnsupported`

**JSON only supports string-keyed dictionaries.** If you have any keys in your DataDictionaries that are not strings when you try to serialize to JSON, the TrySerializeToJson function will return false with `DataError.TypeUnsupported`.

**JSON does not support NaN or Infinity.** If you have any floats or doubles that contain NaN or Infinity, the TrySerializeToJson function will return false with `DataError.ValueUnsupported`.

**JSON does not support anything other than Dictionary or List as the root.** If you use a simple value DataToken without any children, the TrySerializeToJson function will return false with `DataError.TypeUnsupported`.

**JSON only supports one type of number.** It does not differentiate between all the different types. As a result, deserializing from JSON will store all numbers in `Double` format. If you have Data Tokens containing other types of numbers such as `int`, `byte`, or `float` then they can serialize to JSON, however if you Deserialize that same JSON back into Data Containers, you will find that they are now `Doubles` instead.

## Deserializing from JSON

`VRCJson.TryDeserializeFromJson` is the function you should use when you want to go from Json to Data containers. It is recommended to use it as the condition for an `if` or `branch` so that you can choose what happens if it fails and what happens if it succeeds.

If TryDeserializeFromJson returns true, then that means it has successfully turned your Json string into a DataList or DataDictionary. You should then do a type check on the result to determine what happens for each case. 

If this returns false, then the string you provided was not valid JSON. The DataToken you are given will be a DataError, and if you run DataToken.ToString on it, it will give you both the error and a string explaining more details about exactly what went wrong.

For performance reasons, VRCJSON does not parse everything immediately. Instead, it only parses the top level of JSON first. if the top level is valid, but you have have invalid JSON further down inside a nested structure, it is possible for the initial TryDeserializeFromJson to return true. Later, if you use TryGetValue to pull values from something that was invalid, it will return false and give you DataError.UnableToParse.

```csharp title="Deserializing from JSON"
if (VRCJson.TryDeserializeFromJson(json, out DataToken result))
{
    // Deserialization succeeded! Let's figure out what we've got.
    if (result.TokenType == TokenType.DataDictionary)
    {
        Debug.Log($"Successfully deserialized as a dictionary with {result.DataDictionary.Count} items.");
    }
    else if (result.TokenType == TokenType.DataList)
    {
        Debug.Log($"Successfully deserialized as a list with {result.DataList.Count} items.");
    }
    else 
    {
        // This should not be possible. If TryDeserializeFromJson returns true, this it *must* be either a dictionary or a list.
    }
} else {
    // Deserialization failed. Let's see what the error was.
    Debug.Log($"Failed to Deserialize json {json} - {result.ToString()}");
}
```

## Serializing to JSON

`VRCJson.TrySerializeToJson` is the function you should use when you want to go from Data containers to Json. It is recommended to use it as the condition for an `if` or `branch` so that you can choose what happens if it fails and what happens if it succeeds.

If TrySerializeToJson returns true, then that means it has successfully converted your DataList or DataDictionary into a Json string, and you can safely pull the string out of the result token.

```csharp title="Serializing to JSON"
if (VRCJson.TrySerializeToJson(dictionary, JsonExportType.Beautify, out DataToken json))
{
    // Successfully serialized! We can immediately get the string out of the token and do something with it.
    Debug.Log($"Successfully serialized to json: {json.String}");
} 
else 
{
    // Failed to serialize for some reason, running ToString on the result should tell us why.
    Debug.Log(json.ToString());
}
```

### JsonExportType

When serializing to Json, you can choose the JsonExportType you would like. If you want something human-readable, Beautify is better. If you want something compact for sending over the network, Minify is better.

- Beautify: Expands each element to a new line and adds one tab per depth.
- Minify: Keeps everything in one line and minimizes whitespace.


---

# End of Documentation
