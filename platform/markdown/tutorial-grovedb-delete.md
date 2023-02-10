# Overview

GroveDB uses the `delete()` function to [delete](https://github.com/dashpay/grovedb/blob/009787b79538dae833ed6711253852046dfcc59d/grovedb/src/operations/delete/mod.rs) key-values from storage. It takes four arguments as shown below.

``` rust
// Delete Element from GroveDB
pub fn delete<'p, P>(
    &self,
    // Tree path to where the key is located ([] for root)
    path: P,
    // Key to be deleted
    key: &'p [u8],
    // Delete options
    options: Option<DeleteOptions>,
    // Transaction that the delete operation should be included in
    transaction: TransactionArg,
) -> CostResult<(), Error>
```

# Code

This tutorial assumes the reader has already inserted the two key-values from Tutorial 2, but should be easy to follow anyways. It uses `get()` to check if the values are there first, then deletes them, and checks if they’re there again.

In “grovedb-tutorials” create a new Rust project using `cargo new delete`. Navigate to the new project, add GroveDB to the Cargo.toml file, and paste the following code into the main.rs file. Do `cargo run`.

``` rust
use grovedb::GroveDb;

fn main() {

   // Specify a path and open GroveDB at the path as db
   let path = String::from("../storage");
   let db = GroveDb::open(path).unwrap();

   // Check the key-values are there
   let KEY1 = b"hello";
   let KEY2 = b"grovedb";
   let result1 = db.get([], KEY1, None).unwrap();
   let result2 = db.get([], KEY2, None).unwrap();
   println!("Before deleting, we have KEY1: {:?}", result1);
   println!("Before deleting, we have KEY2: {:?}", result2);

   // Delete the values
   db.delete([], KEY1, None, None)
      .unwrap()
      .expect("successfully deleted key1");
   db.delete([], KEY2, None, None)
      .unwrap()
      .expect("successfully deleted key2");

   // Check the key-values again
   let result3 = db.get([], KEY1, None).unwrap();
   let result4 = db.get([], KEY2, None).unwrap();
   println!("After deleting, we have KEY1: {:?}", result3);
   println!("After deleting, we have KEY2: {:?}", result4);
}
```

The terminal should output:

``` text
Before deleting, we have KEY1 Ok(item: [hex: 776f726c64, str: world])
Before deleting, we have KEY2 Ok(item: [hex: 726f636b73, str: rocks])
After deleting, we have KEY1 Err(PathKeyNotFound("key not found in Merk for get: 68656c6c6f"))
After deleting, we have KEY2 Err(PathKeyNotFound("key not found in Merk for get: 67726f76656462"))
```