# ***enzyme***

## overview
***enzyme*** is a tool designed for code translation and lexical analysis, utilising a structure inspired by proteins in biology.

the system uses hierarchical structures to allow for clean translations and parameter management processes.

## structures

### `cofactor`
main translation unit analogous to a biological cofactor, serving as the "active site" for the transpiler:
```c
typedef struct cofactor
{
	text from; // the source pattern
	text to;   // the target pattern
} cofactor;
```

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

### `polypeptide`
aggregates multiple `amino_acid` structures, serving as the collection of parameters:
```c
typedef struct polypeptide
{
	amino_acid* chain; // array of peptides for each parameter
	long size;         // total number of parameters
} polypeptide;
```

### `protein`
functions like a biological protein, an important structure for the transformation of information:
```c
typedef struct protein
{
	cofactor macro;     // translation pattern
	polypeptide params; // parameters, if functional
} protein;
```

### `enzyme`
a collection of `protein` structures that work together to complete the transpiling process across an entire body of text.

the catalysis map is an ASCII look-up-table which groups enzymes which start with the same letter, to allow for accelerated search:
```c
typedef struct catalysis
{
	long* indices; // positions in list for enzymes
	long size;     // amount of similar enzymes
} catalysis;

typedef struct enzyme_struct
{
	protein* list;        // array of enzymes for translations
	long size;            // number of enzymes
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

1. **initialization:** set up the `enzymatic_pathway` with user-defined patterns and parameters.
2. **translation:** apply each `enzyme` to the input text for translation and parameter replacement.
3. **parameter processing:** utilize each `peptide` for assigning values within the translated text.
4. **final output:** produce the fully transpiled text as the end product.
