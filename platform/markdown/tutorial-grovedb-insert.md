# Overview

GroveDB uses the `insert()` function to [insert](https://github.com/dashpay/grovedb/blob/009787b79538dae833ed6711253852046dfcc59d/grovedb/src/operations/insert/mod.rs) items into storage. The function takes five arguments as shown below.

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

This tutorial inserts two key-values into the root tree of the GroveDB instance created in Tutorial 1 (or a new instance if you skipped Tutorial 1). The keys and values of both KVs are strings. Then for the purpose of showing they are there, it uses the `get()` function to retrieve the values and print them to the terminal.

# Code

In “grovedb-tutorials” create a new Rust project using `cargo new insert`. Navigate to the new project, add GroveDB to the Cargo.toml file, and paste the following code into the main.rs file. Do `cargo run`.

``` rust
use grovedb::GroveDb;
use grovedb::Element;

fn main() {

   // Specify a path and open GroveDB at the path as db
   let path = String::from("../storage");
   let db = GroveDb::open(path).unwrap();

   // Define key-values for insertion
   let KEY1 = b"hello";
   let VAL1 = b"world";
   let KEY2 = b"grovedb";
   let VAL2 = b"rocks";

   // Insert key-value 1 into the root tree
   db.insert([], KEY1, Element::Item(VAL1.to_vec(), None), None, None)
       .unwrap()
       .expect("successful root tree leaf insert");

   // Insert key-value 2 into the root tree
   db.insert([], KEY2, Element::Item(VAL2.to_vec(), None), None, None)
       .unwrap()
       .expect("successful root tree leaf 2 insert");

   // At this point the Items are fully inserted into the database.
   // No other steps are required.

   // To show that the Items are there, we will use the get()
   // function to get them from the RocksDB backing store.

   // Get value 1
   let result1 = db.get([], KEY1, None).unwrap();

   // Get value 2
   let result2 = db.get([], KEY2, None).unwrap();

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

Repo with this example: [https://github.com/thephez/grovedb-tutorials](https://github.com/thephez/grovedb-tutorials)

## Element Enum

In Tutorial 2, we inserted strings as the values. However, values may be any of the five variants of the `Element` enum:

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

Note that if you actually want to try this in your existing GroveDB instance, you’ll have to change the keys as well, as you can’t overwrite the existing values.

## Element Functions

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

# Tutorial 3 - Delete

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

# Tutorial 4 - Query

GroveDB generally uses the `query()` function to perform [queries](https://github.com/dashpay/grovedb/blob/009787b79538dae833ed6711253852046dfcc59d/grovedb/src/operations/get/query.rs) on storage. It takes four arguments as shown below.

```
impl GroveDb {
    /// Returns given path query results
    pub fn query(
        &self,
        path_query: &PathQuery,
        allow_cache: bool,
        result_type: QueryResultType,
        transaction: TransactionArg,
    ) -> CostResult<(QueryResultElements, u16)
}
```

## Explanations

GroveDB queries can be very complex. This section gives explanations of each component of a path query. Deep understanding of each component isn’t necessary to follow and learn from the tutorials, so, if you’d rather learn by doing, feel free to [skip it](#tutorial-4-1-simple-range-query).

A **path query**, which is the first argument of `query()`, has a path and a sized query as parameters. The path points to the highest-level subtree you want to query. You can traverse and recurse into lower-level subtrees within a path query using subqueries, as will be explained later.

``` rust
/// Path query
pub struct PathQuery {
    /// Path
    pub path: Vec<Vec<u8>>,
    /// Query
    pub query: SizedQuery,
}
```

A **sized query**, which is the second argument of path queries, takes a query, limit, and offset as parameters. The limit is an integer which specifies the maximum number of results to return in the final result set. The offset is an integer which specifies how many items in the front of the raw result set to exclude from the final result set. In other words, the limit specifies a cut-off at the end of the raw result set, and offset specifies a cut-off at the beginning of the raw result set which, after applied, will compose the final result set. As implied, there is an ordering of the raw (and final) result set, which is defined in the other, first, parameter of a sized query: query.

``` rust
/// Sized query
pub struct SizedQuery {
    /// Query
    pub query: Query,
    /// Limit
    pub limit: Option<u16>,
    /// Offset
    pub offset: Option<u16>,
}
```

A **query** takes a vector of query items as its first argument. A **query item** is either a key, set of keys, or a range. Items in these keys or ranges of keys will be added to the raw result set. Subtrees in these keys or ranges of keys are optionally handled with the next two parameters: the default subquery branch and the conditional subquery branch.

``` rust
/// A `Query` represents one or more keys or ranges of keys, which can be 
/// used to resolve a proof which will include all of the requested values.
pub struct Query {
    /// Items
    pub items: Vec<QueryItem>,
    /// Default subquery branch
    pub default_subquery_branch: SubqueryBranch,
    /// Conditional subquery branches
    pub conditional_subquery_branches: Option<IndexMap<QueryItem, SubqueryBranch>>,
    /// Left to right?
    pub left_to_right: bool,
}

/// A `QueryItem` represents a key or range of keys to be included in a proof.
pub enum QueryItem {
    Key(Vec<u8>),
    Range(Range<Vec<u8>>),
    RangeInclusive(RangeInclusive<Vec<u8>>),
    RangeFull(RangeFull),
    RangeFrom(RangeFrom<Vec<u8>>),
    RangeTo(RangeTo<Vec<u8>>),
    RangeToInclusive(RangeToInclusive<Vec<u8>>),
    RangeAfter(RangeFrom<Vec<u8>>),
    RangeAfterTo(Range<Vec<u8>>),
    RangeAfterToInclusive(RangeInclusive<Vec<u8>>),
}
```

A **default subquery branch** has two parameters: a subquery path and a subquery. The subquery path is a path that is applied to all the subtrees in the result set from the higher-level query we just mentioned. The subquery is a query of the same type as the higher-level query which is applied to the subtrees at the end of the subquery path. The result set of the subquery branch is added to the overall result set. Since subqueries are the same as queries, they can recurse, so you can have subqueries within subqueries. Both subquery path and subquery are optional parameters of a subquery branch. If no subquery path is defined, the subquery applies to all the subtrees in the higher-level query result set. If no subquery is defined, all the elements from the subquery path subtrees are added to the result set.

``` rust
/// Subquery branch
pub struct SubqueryBranch {
    /// Subquery path
    pub subquery_path: Option<Path>,
    /// Subquery
    pub subquery: Option<Box<Query>>,
}
```

A **conditional subquery branch** is the same as default subquery branches, but takes an additional argument for query items, which again are keys or ranges. The subquery branch is only applied to the subtrees which meet the condition of matching the query items.

Finally, the last parameter of query is left_to_right, which is a boolean that defines the order of the result set. Left to right means lower to higher in terms of integers, or alphabetically in terms of strings.

See the [documentation](https://github.com/dashpay/grovedb/blob/master/README.md) for more details.

## Get

The previous tutorials used a function `get()` to retrieve items from storage. _Getting_ only allows for the retrieval of one item, the key must be specified, and cryptographic proofs of _gets_ aren’t supported. Queries, on the other hand, can return many values all at once, the keys don’t need to be provided, and query results _can_ be cryptographically proven.

``` rust
/// Get an element from the backing store
pub fn get<'p, P>(
    &self,
    path: P,
    key: &'p [u8],
    transaction: TransactionArg,
)
```

## Tutorial 4.1 - Simple Query

This tutorial populates a GroveDB instance with items within a subtree within a subtree within the root tree. It then constructs and executes a query to retrieve a subset of the items and prints the query result to the terminal.

In “grovedb-tutorials”, create a new Rust project using `cargo new simple-query`. Navigate to the new project, add GroveDB to the Cargo.toml file, and paste the following code into the main.rs file. Do `cargo run`.

``` rust
use grovedb::GroveDb;
use grovedb::Element;
use grovedb::{ Query, PathQuery};

const KEY1: &[u8] = b"key1";
const KEY2: &[u8] = b"key2";

fn main() {
   
    // Specify the path where the GroveDB instance exists.
    let path = String::from("../storage");
    
    // Open GroveDB at the path.
    let db = GroveDb::open(path).unwrap();

    // Populate GroveDB with values. This function is defined below.
    populate(&db);

    // Define the path to the subtree we want to query.
    let path = vec![KEY1.to_vec(), KEY2.to_vec()];

    // Instantiate a new query.
    let mut query = Query::new();

    // Insert a range of keys to the query that we would like returned.
    // In this case, we are asking for keys 30 through 34.
    query.insert_range(30_u8.to_be_bytes().to_vec()..35_u8.to_be_bytes().to_vec());

    // Put the query into a new unsized path query.
    let path_query = PathQuery::new_unsized(path, query.clone());

    // Execute the query and collect the result items in "elements".
    let (elements, _) = db
        .query_item_value(&path_query, true, None)
        .unwrap()
        .expect("expected successful get_path_query");

    // Print result items to terminal.
    println!("{:?}", elements);

}

fn populate(db: &GroveDb) {

    // Put an empty subtree into the root tree nodes at KEY1.
    // Call this SUBTREE1.
    db.insert([], KEY1, Element::empty_tree(), None, None)
        .unwrap()
        .expect("successful SUBTREE1 insert");

    // Put an empty subtree into subtree1 at KEY2.
    // Call this SUBTREE2.
    db.insert([KEY1], KEY2, Element::empty_tree(), None, None)
        .unwrap()
        .expect("successful SUBTREE2 insert");

    // Populate SUBTREE2 with values 0 through 99 under keys 0 through 99.
    for i in 0u8..100 {
        let i_vec = (i as u8).to_be_bytes().to_vec();
        db.insert([KEY1, KEY2], &i_vec, Element::new_item(i_vec.clone()), None, None)
            .unwrap()
            .expect("successfully inserted values");
    }
}
```

The terminal should output:

``` text
[[30], [31], [32], [33], [34]]
```

## Tutorial 4.2 - Complex Query

In “grovedb-tutorials”, create a new Rust project using `cargo new complex-query`. Navigate to the new project, add GroveDB to the Cargo.toml file, and paste the following code into the main.rs file. Do `cargo run`.

# Tutorial 5 - Proofs

GroveDB generally uses the `prove_query()` function to [generate](https://github.com/dashpay/grovedb/blob/009787b79538dae833ed6711253852046dfcc59d/grovedb/src/operations/proof/generate.rs) query proofs, and the `verify_query()` function to [verify](https://github.com/dashpay/grovedb/blob/009787b79538dae833ed6711253852046dfcc59d/grovedb/src/operations/proof/verify.rs) query proofs. `prove_query()` just takes a path query as an argument and `verify_query()` takes a proof as well as a path query.

This tutorial assumes the reader has already populated GroveDB with the key-values from Tutorial 4, but should be easy to follow anyways. Note that the first block of code in `main()` is essentially the same as Tutorial 4, but without the `populate()` function. We will generate a proof for the query from Tutorial 4, use it to calculate the root hash, and then compare that root hash to the GroveDB root hash.

In “grovedb-tutorials”, create a new Rust project using `cargo new proof`. Navigate to the new project, add GroveDB to the Cargo.toml file, and paste the following code into the main.rs file. Do `cargo run`.

``` rust
use grovedb::GroveDb;
use grovedb::{ Query, PathQuery };

const KEY1: &[u8] = b"key1";
const KEY2: &[u8] = b"key2";

fn main() {

    // Specify the path to the previously created GroveDB instance
    let path = String::from("../storage");
    // Open GroveDB as db
    let db = GroveDb::open(path).unwrap();
    // Define the path to the subtree we want to query.
    let path = vec![KEY1.to_vec(), KEY2.to_vec()];
    // Instantiate a new query.
    let mut query = Query::new();
    // Insert a range of keys to the query that we would like returned.
    query.insert_range(30_u8.to_be_bytes().to_vec()..35_u8.to_be_bytes().to_vec());
    // Put the query into a new unsized path query.
    let path_query = PathQuery::new_unsized(path, query.clone());
    // Execute the query and collect the result items in "elements".
    let (_elements, _) = db
        .query_item_value(&path_query, true, None)
        .unwrap()
        .expect("expected successful get_path_query");

    // Generate proof.
    let proof = db.prove_query(&path_query).unwrap().unwrap();

    // Get hash from query proof and print to terminal along with GroveDB root hash.
    let (hash, _result_set) = GroveDb::verify_query(&proof, &path_query).unwrap();

    // See if the query proof hash matches the GroveDB root hash
    println!("Does the hash generated from the query proof match the GroveDB root hash?");
    if hash == db.root_hash(None).unwrap().unwrap() {
        println!("Yes");
    } else { println!("No"); };
}
```

The terminal should output:

``` text
Does the hash generated from the query proof match the GroveDB root hash?
Yes
```

Now you know that your query results are complete, correct, and fresh.