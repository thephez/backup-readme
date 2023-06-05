# Query Function

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

# Explanations

GroveDB queries can be very complex. This section gives explanations of each component of a path query. Deep understanding of each component isn’t necessary to follow and learn from the tutorials, so, if you’d rather learn by doing, feel free to [skip it](#simple-query-tutorial).

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

# Get

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

# Simple Query Tutorial

This tutorial populates GroveDB with values 0-99 in a subtree within a subtree within the root tree:

```
/// Root
///    SUBTREE1
///       SUBTREE2
///          Values 0-99
```

It then constructs and executes a query to retrieve a subset of the items and prints the query result to the terminal.

The following code can be run with ```cargo run --bin query-simple```.

``` rust
use grovedb::operations::insert::InsertOptions;
use grovedb::Element;
use grovedb::GroveDb;
use grovedb::{PathQuery, Query};

const KEY1: &[u8] = b"key1";
const KEY2: &[u8] = b"key2";

// Allow insertions to overwrite trees
// This is necessary so the tutorial can be rerun easily
const INSERT_OPTIONS: Option<InsertOptions> = Some(InsertOptions {
    validate_insertion_does_not_override: false,
    validate_insertion_does_not_override_tree: false,
    base_root_storage_is_free: true,
});

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
    db.insert([], KEY1, Element::empty_tree(), INSERT_OPTIONS, None)
        .unwrap()
        .expect("successful SUBTREE1 insert");

    // Put an empty subtree into subtree1 at KEY2.
    // Call this SUBTREE2.
    db.insert([KEY1], KEY2, Element::empty_tree(), INSERT_OPTIONS, None)
        .unwrap()
        .expect("successful SUBTREE2 insert");

    // Populate SUBTREE2 with values 0 through 99 under keys 0 through 99.
    for i in 0u8..100 {
        let i_vec = (i as u8).to_be_bytes().to_vec();
        db.insert(
            [KEY1, KEY2],
            &i_vec,
            Element::new_item(i_vec.clone()),
            INSERT_OPTIONS,
            None,
        )
        .unwrap()
        .expect("successfully inserted values");
    }
}
```

The terminal should output:

``` text
[[30], [31], [32], [33], [34]]
```

# Complex Query Tutorial

This tutorial populates GroveDB with the following tree structure:

```
/// Root
///   SUBTREE1
///      SUBTREE2
///         Values 0-49 except random number 1
///         SUBTREE3
///            Values 50-74 except random number 2
///            SUBTREE4
///               SUBTREE5
///                  Values 75-99
```

It then queries SUBTREE2 for keys 20-30. Call this QUERY1.

It applies a conditional subquery to QUERY1, which says if any keys 20-25 are subtrees, navigate the subquery path and execute the subquery. Call this SUBQUERY1. No path is specified for SUBQUERY1, so the subquery is applied directly to the subtrees. The subquery asks for keys 60 and 70 from those subtrees.

Say SUBTREE3 is located at key 22. The result set now looks like this:

```
[20,21,60,70,23,24,25,26,27,28,29,30]
```

It then applies a conditional subquery to SUBQUERY1, which says if 60 is a subtree, navigate the subquery path and execute the subquery. Call this SUBQUERY2. SUBQUERY2 specifies to navigate through SUBTREE4 and execute the subquery on SUBTREE5. The subquery asks for keys 90 through 94.

Say SUBTREE4 is located at key 60. The result set now looks like this:

```
[20,21,90,91,92,93,94,70,23,24,25,26,27,28,29,30]
```

QUERY1, SUBQUERY1, and SUBQUERY2 are then all put into a sized query, which sets a limit of 10 and an offset of 3. Limit of 10 means no more than 10 items can be included in the results. Offset of 3 means the first 3 pre-sized query results are omitted from the post-sized query results.

The final result set is:

```
[91,92,93,94,70,23,24,25,26,27]
```

The sized query is then passed to the path query and the path query is executed.

The following code can be run with ```cargo run --bin query-complex```.

``` rust
use grovedb::operations::insert::InsertOptions;
use grovedb::Element;
use grovedb::GroveDb;
use grovedb::{PathQuery, Query, QueryItem, SizedQuery};
use rand::Rng;

const KEY1: &[u8] = b"key1";
const KEY2: &[u8] = b"key2";
const KEY3: &[u8] = b"key3";

// Allow insertions to overwrite trees
// This is necessary so the tutorial can be rerun easily
const INSERT_OPTIONS: Option<InsertOptions> = Some(InsertOptions {
    validate_insertion_does_not_override: false,
    validate_insertion_does_not_override_tree: false,
    base_root_storage_is_free: true,
});

fn main() {
    // Specify the path where the GroveDB instance exists.
    let path = String::from("../storage");

    // Open GroveDB at the path.
    let db = GroveDb::open(path).unwrap();

    // Populate GroveDB with values. This function is defined below.
    populate(&db);

    // Define the path to the highest-level subtree we want to query.
    let path = vec![KEY1.to_vec(), KEY2.to_vec()];

    // Instantiate new queries.
    let mut query = Query::new();
    let mut subquery = Query::new();
    let mut subquery2 = Query::new();

    // Insert query items into the queries.
    // Query 20-30 at path.
    query.insert_range(20_u8.to_be_bytes().to_vec()..31_u8.to_be_bytes().to_vec());
    // If any 20-30 are subtrees and meet the subquery condition,
    // follow the path and query 60, 70 from there.
    subquery.insert_keys(vec![vec![60], vec![70]]);
    // If either 60, 70 are subtrees and meet the subquery condition,
    // follow the path and query 90-94 from there.
    subquery2.insert_range(90_u8.to_be_bytes().to_vec()..95_u8.to_be_bytes().to_vec());

    // Add subquery branches.
    // If 60 is a subtree, navigate through SUBTREE4 and run subquery2 on SUBTREE5.
    subquery.add_conditional_subquery(QueryItem::Key(vec![60]), Some(vec!(KEY3.to_vec())), Some(subquery2));
    // If anything up to and including 25 is a subtree, run subquery on it. No path.
    query.add_conditional_subquery(
        QueryItem::RangeToInclusive(std::ops::RangeToInclusive { end: vec![25] }),
        None,
        Some(subquery),
    );

    // Put the query into a sized query. Limit the result set to 10,
    // and impose an offset of 3.
    let sized_query = SizedQuery::new(query, Some(10), Some(3));

    // Put the sized query into a new path query.
    let path_query = PathQuery::new(path, sized_query.clone());

    // Execute the path query and collect the result items in "elements".
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
    db.insert([], KEY1, Element::empty_tree(), INSERT_OPTIONS, None)
        .unwrap()
        .expect("successful SUBTREE1 insert");

    // Put an empty subtree into subtree1 at KEY2.
    // Call this SUBTREE2.
    db.insert([KEY1], KEY2, Element::empty_tree(), INSERT_OPTIONS, None)
        .unwrap()
        .expect("successful SUBTREE2 insert");

    // Populate SUBTREE2 with values 0 through 49 under keys 0 through 49.
    for i in 0u8..50 {
        let i_vec = (i as u8).to_be_bytes().to_vec();
        db.insert(
            [KEY1, KEY2],
            &i_vec,
            Element::new_item(i_vec.clone()),
            INSERT_OPTIONS,
            None,
        )
        .unwrap()
        .expect("successfully inserted values in SUBTREE2");
    }

    // Set random_numbers
    let mut rng = rand::thread_rng();
    let rn1: &[u8] = &(rng.gen_range(15..26) as u8).to_be_bytes();
    let rn2: &[u8] = &(rng.gen_range(60..62) as u8).to_be_bytes();

    // Overwrite key rn1 with a subtree
    // Call this SUBTREE3
    db.insert(
        [KEY1, KEY2],
        &rn1,
        Element::empty_tree(),
        INSERT_OPTIONS,
        None,
    )
    .unwrap()
    .expect("successful SUBTREE3 insert");

    // Populate SUBTREE3 with values 50 through 74 under keys 50 through 74
    for i in 50u8..75 {
        let i_vec = (i as u8).to_be_bytes().to_vec();
        db.insert(
            [KEY1, KEY2, rn1],
            &i_vec,
            Element::new_item(i_vec.clone()),
            INSERT_OPTIONS,
            None,
        )
        .unwrap()
        .expect("successfully inserted values in SUBTREE3");
    }

    // Overwrite key rn2 with a subtree
    // Call this SUBTREE4
    db.insert(
        [KEY1, KEY2, rn1],
        &rn2,
        Element::empty_tree(),
        INSERT_OPTIONS,
        None,
    )
    .unwrap()
    .expect("successful SUBTREE4 insert");

    // Put an empty subtree into SUBTREE4 at KEY3.
    // Call this SUBTREE5.
    db.insert([KEY1, KEY2, rn1, rn2], KEY3, Element::empty_tree(), INSERT_OPTIONS, None)
        .unwrap()
        .expect("successful SUBTREE5 insert");

    // Populate SUBTREE5 with values 75 through 99 under keys 75 through 99
    for i in 75u8..99 {
        let i_vec = (i as u8).to_be_bytes().to_vec();
        db.insert(
            [KEY1, KEY2, rn1, rn2, KEY3],
            &i_vec,
            Element::new_item(i_vec.clone()),
            INSERT_OPTIONS,
            None,
        )
        .unwrap()
        .expect("successfully inserted values in SUBTREE5");
    }
}
```

The terminal output depends on which random numbers were generated. A few of the possibilities are shown below.

Random number 1 does not fall between 20-25:

```
[[22], [23], [24], [25], [26], [27], [28], [29]]
```

Random number 1 is 23, random number 2 is not 60:

```
[[60], [70], [24], [25], [26], [27], [28], [29]]
```

Random number 1 is 21, random number 2 is 60:

```
[[92], [93], [94], [70], [22], [23], [24], [25], [26], [27]]
```