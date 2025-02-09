# categories

This contains tools for constructing the category tree for a 
given Wikipedia language.

## Usage
```shell
python3 categories en --dest ./public/categories
```

The above command serializes the category tree for the 
English Wikipedia, and stores it in the `./public/categories`
directory. See `python3 categories --help` for available 
options.

## Format

The category tree is split into bins, in which `.category` 
files are placed. Directories are named as the remainder of 
a given category's id divided by 2000. This balancing was 
chosen by observation of the English Wikipedia, which at the 
time of writing has about 2 million unique categories.

`.category` files are named by their page ids. The file type is 
an easily parseable, compact binary format for storing a 
category name, its neighbors, and its articles. 

The structure is described as follows:

* (4 bytes) `name_bytes_len`

* utf-8 string of the category title, of byte length 
  `name_bytes_len`

* (4 bytes) `predecessors_bytes_len`

* Unsigned int32 list of predecessor ids, of byte length
  `predecessors_bytes_len`
* (4 bytes) `successors_bytes_len`

* Unsigned int32 list of successor ids, of byte length 
  `successors_bytes_len`

* Unsigned int32 list of article ids, to EOF

The script generates a `.index` file in the root directory and 
each of its bins - this should be interpreted as an array 
of unsigned 32-bit integers. This array described the names of 
the bins (in the case of the root directory), and the ids of 
the categories contained within the bin (in the case of a 
category bin).

The motivation of separating the large number of category 
files into bins serves two major purposes - to allow the user 
to find a random category without downloading an entire 
category id index and to ease reading the directory 
contents during development.
