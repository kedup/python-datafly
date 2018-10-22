# Python Datafly

`datafly.py` is a Python implementation of the [Datafly algorithm](https://en.wikipedia.org/wiki/Datafly_algorithm).

Datafly is a greedy heuristic algorithm which is used to anonymize a table in order to satisfy [k-anonymity](https://en.wikipedia.org/wiki/K-anonymity).

Currently supports the CSV format.

## Usage

Use the `--help` command to show the help message:

```
usage: datafly.py [-h] --private_table PRIVATE_TABLE --quasi_identifier
                  QUASI_IDENTIFIER [QUASI_IDENTIFIER ...]
                  --domain_gen_hierarchies DOMAIN_GEN_HIERARCHIES
                  [DOMAIN_GEN_HIERARCHIES ...] -k K --output OUTPUT

Python implementation of the Datafly algorithm. Finds a k-anonymous
representation of a table.

optional arguments:
  -h, --help            show this help message and exit
  --private_table PRIVATE_TABLE, -pt PRIVATE_TABLE
                        Path to the CSV table to K-anonymize.
  --quasi_identifier QUASI_IDENTIFIER [QUASI_IDENTIFIER ...], -qi QUASI_IDENTIFIER [QUASI_IDENTIFIER ...]
                        Names of the attributes which are Quasi Identifiers.
  --domain_gen_hierarchies DOMAIN_GEN_HIERARCHIES [DOMAIN_GEN_HIERARCHIES ...], -dgh DOMAIN_GEN_HIERARCHIES [DOMAIN_GEN_HIERARCHIES ...]
                        Paths to the generalization files (must have same
                        order as the QI name list.
  -k K                  Value of K.
  --output OUTPUT, -o OUTPUT
                        Path to the output file.
```

#### Domain Generalization Hierarchy file format

For each Quasi Identifier attribute it must be specified a corresponding Domain Generalization Hierarchy, which is used to generalize the attribute values.

Each DGH is specified through a DGH file, which in each line specifies the hierarchy relationship of a value for that attribute. For example, for an attribute `age`, the file could be in this format:

```
...
42,30-45,30-60,1-60,1-120
43,30-45,30-60,1-60,1-120
44,30-45,30-60,1-60,1-120
45,30-45,30-60,1-60,1-120
46,45-60,30-60,1-60,1-120
...
```

As shown above each line specifies for a value not generalized (generalization level 0) its hierarchy relationship in the form `level 0,level 1,level 2,...,level n` (from not-generalized to most generic value).

#### Example of anonymization:

The [`./example`](https://github.com/alessiovierti/python-datafly/tree/master/example) folder contains a sample database (`db_100.csv`), and some Domain Generalization Hierarchy files (`age_generalization.csv`, `city_birth_generalization.csv`, `zip_code_generalization.csv`).

The following command will anonymize the table `db_100.csv` writing a new table `db_100_3_anon.csv` which is 3-anonymous (`k = 3`):

```
$ python datafly.py -pt "example/db_100.csv" -qi "age" "city_birth" "zip_code" -dgh "example/age_generalization.csv" "example/city_birth_generalization.csv" "example/zip_code_generalization.csv" -k 3 -o "example/db_100_3_anon.csv"
```

Note that the list of Quasi Identifier names and the corresponding DGH files paths must have the same order.

## Authors

* **Alessio Vierti** - *Initial work*

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details
