# Tutorial

GroveDB uses the `open()` function to [open](https://github.com/dashpay/grovedb/blob/master/grovedb/src/lib.rs) an existing instance of GroveDB at the given path in your filesystem, unless there is no existing instance, in which case a new one will be created with an empty root tree. The only argument for `open()` is the filesystem path.

The following code can be run with ```cargo run --bin open```.

``` rust
use grovedb::GroveDb;

fn main() {

   // Specify the path where you want to set up the GroveDB instance
   let path = String::from("../storage");
  
   // Open a new GroveDB at the path
   GroveDb::open(path).unwrap();

   // Print to terminal
   println!("Opened {:?}", path);

}
```

A folder called “storage” should have appeared in your grovedb-tutorials directory, populated with a bunch of files. GroveDB is set up and ready for us to insert some data.