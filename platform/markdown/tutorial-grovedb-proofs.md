# Overview

GroveDB generally uses the `prove_query()` function to [generate](https://github.com/dashpay/grovedb/blob/009787b79538dae833ed6711253852046dfcc59d/grovedb/src/operations/proof/generate.rs) query proofs, and the `verify_query()` function to [verify](https://github.com/dashpay/grovedb/blob/009787b79538dae833ed6711253852046dfcc59d/grovedb/src/operations/proof/verify.rs) query proofs. `prove_query()` just takes a path query as an argument and `verify_query()` takes a proof as well as a path query.

## Prerequisites

This tutorial assumes the reader has already populated GroveDB with the key-values from Tutorial 4, but should be easy to follow anyways. Note that the first block of code in `main()` is essentially the same as Tutorial 4, but without the `populate()` function. We will generate a proof for the query from Tutorial 4, use it to calculate the root hash, and then compare that root hash to the GroveDB root hash.

# Code
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