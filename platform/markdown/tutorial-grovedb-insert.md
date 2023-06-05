# Tutorial

GroveDB uses the `insert()` function to [insert](https://github.com/dashpay/grovedb/blob/009787b79538dae833ed6711253852046dfcc59d/grovedb/src/operations/insert/mod.rs) items into storage. Insert will also overwrite values unless the value is a tree. The function takes five arguments as shown below.

``` rust
/// Insert Element into GroveDB
pub fn insert<'p, P>(
   &self,
   // Tree path to the subtree where the key-value should be inserted
   path: P,
   // The key
   key: &'p [u8],
   // The value
   element: Element,
   // Insert options
   options: Option<InsertOptions>,
   // Transaction that the insert operation should be included in
   transaction: TransactionArg,
)
```

This tutorial inserts two key-values into the root tree of the GroveDB instance created in Open Tutorial (or a new instance if you skipped Open Tutorial). The keys and values of both KVs are strings. Then for the purpose of showing they are there, it uses the `get()` function to retrieve the values and print them to the terminal.

The following code can be run with ```cargo run --bin insert```.

``` rust
use grovedb::GroveDb;
use grovedb::Element;

fn main() {

   // Specify a path and open GroveDB at the path as db
   let path = String::from("../storage");
   let db = GroveDb::open(path).unwrap();

   // Define key-values for insertion
   let key1 = b"hello";
   let val1 = b"world";
   let key2 = b"grovedb";
   let val2 = b"rocks";

   // Insert key-value 1 into the root tree
   db.insert([], key1, Element::Item(val1.to_vec(), None), None, None)
       .unwrap()
       .expect("successful root tree leaf insert");

   // Insert key-value 2 into the root tree
   db.insert([], key2, Element::Item(val2.to_vec(), None), None, None)
       .unwrap()
       .expect("successful root tree leaf 2 insert");

   // At this point the Items are fully inserted into the database.
   // No other steps are required.

   // To show that the Items are there, we will use the get()
   // function to get them from the RocksDB backing store.

   // Get value 1
   let result1 = db.get([], key1, None).unwrap();

   // Get value 2
   let result2 = db.get([], key2, None).unwrap();

   // Print the values to terminal
   println!("{:?}", result1);
   println!("{:?}", result2);
}
```

The terminal should output:

``` text
Ok(item: [hex: 776f726c64, str: world])
Ok(item: [hex: 726f636b73, str: rocks])
```

This tells us that at the given keys exist items with the shown hex and str values.

# Element Enum

In the previous tutorial, we inserted strings as the values. However, values may be any of the five variants of the `Element` enum:

``` rust
pub enum Element {
    /// An ordinary value
    Item(Vec<u8>, Option<ElementFlags>),
    /// A reference to an object by its path
    Reference(ReferencePathType, MaxReferenceHop, Option<ElementFlags>),
    /// A subtree, contains the a prefixed key representing the root of the
    /// subtree.
    Tree(Option<Vec<u8>>, Option<ElementFlags>),
    /// Signed integer value that can be totaled in a sum tree
    SumItem(SumValue, Option<ElementFlags>),
    /// Same as Element::Tree but underlying Merk sums value of it's summable
    /// nodes
    SumTree(Option<Vec<u8>>, SumValue, Option<ElementFlags>),
}
```

For example, instead of using `let val1 = b"world"`, we could have put `let val1 = Element::Tree(None, None)` and then put `val1` in the insert function instead of `Element::Item(val1.to_vec(), None)`.

In that case, the output would have returned:

``` text
Ok(tree: None)
Ok(item: [hex: 726f636b73, str: rocks])
```

# Element Functions

Instead of passing `Element::Item()` or some other `Element` enum to the `insert` function, we could also pass one of the functions implemented in `Element`, `new_item()`. Given an item value, the `new_item` function returns `Element::Item(item_value, None)`. It looks like this:

``` rust
impl Element {
    /// Set element to an item with a default flag value
    pub fn new_item(item_value: Vec<u8>) -> Self {
        Element::Item(item_value, None)
    }
}
```

And would be used like so:

``` rust
// Insert key-value 1 into the root tree
db.insert([], key1, Element::new_item(val1.to_vec()), None, None)
   .unwrap()
   .expect("successful root tree leaf insert");
```

The `new_item()` function is useful because it allows developers to set a default value for the flags. In our case it’s `None`.

There are a few other [functions](https://github.com/dashpay/grovedb/blob/master/grovedb/src/element/constructor.rs) implemented in `Element` that can be used similarly. The `Element::empty_tree()` function is quite useful as well.