# ***enzyme***

## overview
***enzyme*** is a tool designed for code translation and lexical analysis, utilising a structure inspired by proteins in biology.

the system uses hierarchical structures to allow for clean translations and parameter management processes.

## structures
##
### `cofactor`
main translation unit analogous to a biological cofactor, serving as the "active site" for the transpiler:
```c
typedef struct cofactor
{
	text from; // the source pattern
	text to;   // the target pattern
} cofactor;
```
```c
cofactor example_cofactor =
{
	.from = "add_thing( a, b )",
	.to = "{ a = a + b; }"
}
```
##
### `amino_acid`
captures the positional information of parameters much like amino-acids in a chain:
```c
typedef struct amino_acid
{
	text param;    // the parameter name
	long* indices; // positions in enzyme's cofactor.to
	long size;     // number of occurrences
} amino_acid;
```
```c
amino_acid example_amino_1 =
{
	.text = "a",
	.indicies = { 2, 6 },
	.size = 2
}

amino_acid example_amino_2 =
{
	.text = "b",
	.indicies = { 10 },
	.size = 1
}
```
##
### `polypeptide`
aggregates multiple `amino_acid` structures, serving as the collection of parameters:
```c
typedef struct polypeptide
{
	cofactor macro;    // translation pattern
	amino_acid* params; // array of parameters
	long size;         // total number of parameters
} polypeptide;
```
```c
polypeptide example_polypeptide =
{
	.macro = example_cofactor,
	.params = { example_amino_1, example_amino_2 },
	.size = 2
}
```
##
### `enzyme`
a collection of `polypeptide` structures that work together to complete the transpiling process across an entire body of text.

the catalysis map is an ASCII look-up-table which groups enzymes which start with the same letter, to allow for accelerated search:
```c
typedef struct catalysis
{
	long* indices; // positions in list for polypeptides
	long size;     // amount of similar polypeptides
} catalysis;

typedef struct enzyme_struct
{
	polypeptide* list;    // array of polypeptides for translations
	long size;            // number of polypeptides
	catalysis map[ 256 ]; // ASCII-LUT
} enzyme_struct;
typedef enzyme_struct* enzyme;
```
##

the `cofactor` and `amino_acid` types rely on a `text` type, which is a dynamically expanding string of chars:
```c
typedef struct text_struct
{
	char* str;
	long str_size;
	long mem_size;
} text_struct;
typedef text_struct* text;
```

## processes

1. **initialization:** set up the `enzyme` with user-defined patterns and parameters.
2. **translation:** apply each `polypeptide` to the input text for translation and parameter replacement.
3. **parameter processing:** utilize each `cofactor` and `amino_acid` for assigning values within the translated text.
4. **final output:** produce the fully transpiled text as the end product.
