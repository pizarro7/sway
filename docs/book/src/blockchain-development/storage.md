# Storage

<!-- This section should explain storage in Sway -->
<!-- storage:example:start -->
When developing a [smart contract](../sway-program-types/smart_contracts.md), you will typically need some sort of persistent storage. In this case, persistent storage, often just called _storage_ in this context, is a place where you can store values that are persisted inside the contract itself. This is in contrast to a regular value in _memory_, which disappears after the contract exits.

Put in conventional programming terms, contract storage is like saving data to a hard drive. That data is saved even after the program which saved it exits. That data is persistent. Using memory is like declaring a variable in a program: it exists for the duration of the program and is non-persistent.

Some basic use cases of storage include declaring an owner address for a contract and saving balances in a wallet.
<!-- storage:example:end -->

## Storage Accesses Via the `storage` Keyword

Declaring variables in storage requires a `storage` declaration that contains a list of all your variables, their types, and their initial values. The initial value can be any expression that can be evaluated to a constant during compilation, as follows:

```sway
{{#include ../../../../examples/storage_variables/src/main.sw:storage_declaration}}
```

Imported structs with private fields can be initialized in a `storage` declaration only if they provide a public [constructor](../basics/methods_and_associated_functions.md#constructors) that can be evaluated to a constant during compilation. Otherwise, to store such structs in the contract storage, the [manual storage management](#manual-storage-management) must be used.

To write into a storage variable, you need to use the `storage` keyword as follows:

```sway
{{#include ../../../../examples/storage_variables/src/main.sw:storage_write}}
```

To read a storage variable, you also need to use the `storage` keyword as follows:

```sway
{{#include ../../../../examples/storage_variables/src/main.sw:storage_read}}
```

## Storage Maps

Generic storage maps are available in the standard library as `StorageMap<K, V>` which have to be defined inside a `storage` block and allow you to call `insert()` and `get()` to insert values at specific keys and get those values respectively. Refer to [Storage Maps](../common-collections/storage_map.md) for more information about `StorageMap<K, V>`.

## Storage Namespace

If you want the values in storage to be positioned differently, for instance to avoid collisions with storage from another contract when loading code, you can use the namespace annotation to add a salt to the slot calculations.

```sway
{{#include ../../../../examples/storage_namespace/src/main.sw:storage_namespace}}
```

## Manual Storage Management

It is possible to leverage FuelVM storage operations directly using the `std::storage::storage_api::write` and `std::storage::storage_api::read` functions provided in the standard library. With this approach you will have to manually assign the internal key used for storage. An example is as follows:

```sway
{{#include ../../../../examples/storage_example/src/main.sw}}
```

> **Note**: Though these functions can be used for any data type, they should mostly be used for arrays because arrays are not yet supported in `storage` blocks. Note, however, that _all_ data types can be used as types for keys and/or values in `StorageMap<K, V>` without any restrictions.
