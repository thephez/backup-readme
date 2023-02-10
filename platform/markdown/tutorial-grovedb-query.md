# Overview

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

# Code

## Simple Query

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

## Complex Query

In “grovedb-tutorials”, create a new Rust project using `cargo new complex-query`. Navigate to the new project, add GroveDB to the Cargo.toml file, and paste the following code into the main.rs file. Do `cargo run`.