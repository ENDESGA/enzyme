# ***enzyme***

## overview
***enzyme*** is a tool designed for code translation and lexical analysis, utilising a structure inspired by proteins in biology.

the system uses hierarchical structures to allow for clean translations and parameter management processes.

## structures

### `amino_acid`
a basic translation unit analogous to a biological amino acid, serving as the building block for the transpiler:
```c
typedef struct amino_acid
{
	text from; // the source pattern
	text to;   // the target pattern
} amino_acid;
```

### `peptide`
captures the positional information of parameters much like peptides form active segments of proteins:
```c
typedef struct peptide
{
	text param;      // the parameter name
	long* indices;   // positions in enzyme's cofactor.to
	long size;       // number of occurrences
} peptide;
```

### `polypeptide`
aggregates multiple `peptide` structures, each representing a parameter within a translation unit:
```c
typedef struct polypeptide
{
	peptide* chain; // array of peptides for each parameter
	long size;      // total number of parameters
} polypeptide;
```

### `enzyme`
functions like a biological enzyme, catalyzing a translation operation with an associated pattern and parameters:
```c
typedef struct enzyme
{
	amino_acid cofactor;  // translation pattern
	polypeptide peptides; // parameters, if functional
} enzyme;
```

### `enzymatic_pathway`
a collection of `enzyme` structures that work together to complete the transpiling process across an entire body of text.

the catalysis map is an ASCII look-up-table which groups enzymes which start with the same letter, to allow for accelerated search:
```c
typedef struct catalysis
{
	long* indices; // positions in list for enzymes
	long size;     // amount of similar enzymes
} catalysis;

typedef struct enzymatic_struct {
	enzyme* list;         // array of enzymes for translations
	long size;            // number of enzymes
	catalysis map[ 256 ]; // ASCII-LUT
} enzymatic_struct;
typedef enzymatic_struct* enzymatic_pathway;
```
##

the `amino_acid` and `peptide` types rely on a `text` type, which is a dynamically expanding string of chars:
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
