# Table Extension Specification

- **Title:** Table
- **Identifier:** <https://stac-extensions.github.io/table/v1.2.0/schema.json>
- **Field Name Prefix:** table
- **Scope:** Item, Collection
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @TomAugspurger

This document explains the table Extension to the [SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec) (STAC) specification.
It can be used with the [projection] extension to describe geospatial tabular data.

An Collection or Item can describe a *tabular data assets*, using a list of [Column objects](#column-object).
Additionally, Collections can describe many tabular datasets using [Table objects](#table-object).

- Examples:
  - [Item example](examples/item.json): Shows the basic usage of the extension in a STAC Item
  - [Collection example](examples/collection.json): Shows the basic usage of the extension in a STAC Collection
- [JSON Schema](json-schema/schema.json)
- [Changelog](./CHANGELOG.md)

## Item Properties and Collection Fields

|       Field Name       |                Type                 |                            Description                            |
| ---------------------- | ----------------------------------- | ----------------------------------------------------------------- |
| table:columns          | [ [Column Object](#column-object) ] | A list of (#column objects) describing each column. |
| table:primary_geometry | string                              | The primary geometry column name.                                 |
| table:row_count        | number                              | The number of rows in the dataset.                                |

**table:primary_geometry** Is the column name of the "primary" or "active" geometry. This is used by libraries like [geopandas] and [sf]
to control which geometry column is used. When a STAC item uses both the [projection] and `table` extensions, it's understood that the
values in `proj:espg`, `proj:bbox`, etc. refer to the `primary_geometry` column.

### Column Object

Column objects contain information about each colum in the table.

| Field Name  |  Type  |                                                        Description                                                         |
| ----------- | ------ | -------------------------------------------------------------------------------------------------------------------------- |
| name        | string | **REQUIRED**. The column name                                                                                              |
| description | string | Detailed multi-line description to explain the dimension. CommonMark 0.29 syntax MAY be used for rich text representation. |
| type        | string | Data type of the column. If using a file format with a type system (like Parquet), we recommend you use those types.       |

## *Asset Object* fields

The following fields can be used for assets (in the [`Asset Object`](https://github.com/radiantearth/stac-spec/blob/master/item-spec/item-spec.md#asset-object)).

|      Field Name       |       Type       |                 Description                  |
| --------------------- | ---------------- | -------------------------------------------- |
| table:storage_options | Map<string, any> | Additional keywords for opening the dataset. |

``table:storage_options`` can be used with [fsspec](https://filesystem-spec.readthedocs.io/en/latest/) to specify additional keywords
necessary to open the data. For example, an asset might use ``{"account_name": "ai4edataeuwest"}`` to indicate that the asset is
in the ``ai4edataeuwest`` storage account. Libraries like [adlfs](https://github.com/dask/adlfs) use this information to open the dataset.

## Collection Fields

The following fields apply only to
[Collections](https://github.com/radiantearth/stac-spec/blob/master/collection-spec/collection-spec.md).
They can be used to catalog a collection of tables, where each table is stored as an `Item`, without
having to include column-level metadata from each table on the Collection.

|  Field Name  |                    Type                    |               Description                |
| ------------ | ------------------------------------------ | ---------------------------------------- |
| table:tables | Map<string, [Table Object](#table-object)> | **REQUIRED** A mapping of table names to |

### Table Object

Table objects contain high-level summaries about a table.

| Field Name  |  Type  |                                                        Description                                                         |
| ----------- | ------ | -------------------------------------------------------------------------------------------------------------------------- |
| name        | string | **REQUIRED**. The table name                                                                                               |
| description | string | Detailed multi-line description to explain the dimension. CommonMark 0.29 syntax MAY be used for rich text representation. |

## Best Practices

STAC allows for some flexibility in how to catalog assets. In general, we recommend using the following hierarchy:

- Use `Collection` objects to catalog a dataset consisting of one or more tables.
- Use `Item` objects to catalog an individual table.

For a dataset consisting of a single table or many tables with the same schema (for example
[gbif](https://github.com/microsoft/AIforEarthDataSets/blob/main/data/gbif.md), which provides snapshots of the same database at
different points in time), you might include `table:columns` on the `Collection` itself, or both the `Collection` and `items`.

For datasets with many tables (for example, [USF Forest Inventory and Analysis](https://github.com/microsoft/AIforEarthDataSets/blob/main/data/forest-inventory-and-analysis.md)),
we recommend cataloging just the *tables* at the Collection level in `table:tables`, and cataloging the the columns at just the `Item` level in `table:columns`
on each Item.

## Contributing

All contributions are subject to the
[STAC Specification Code of Conduct](https://github.com/radiantearth/stac-spec/blob/master/CODE_OF_CONDUCT.md).
For contributions, please follow the
[STAC specification contributing guide](https://github.com/radiantearth/stac-spec/blob/master/CONTRIBUTING.md) Instructions
for running tests are copied here for convenience.

### Running tests

The same checks that run as checks on PR's are part of the repository and can be run locally to verify that changes are valid. 
To run tests locally, you'll need `npm`, which is a standard part of any [node.js installation](https://nodejs.org/en/download/).

First you'll need to install everything with npm once. Just navigate to the root of this repository and on 
your command line run:
```bash
npm install
```

Then to check markdown formatting and test the examples against the JSON schema, you can run:
```bash
npm test
```

This will spit out the same texts that you see online, and you can then go and fix your markdown or examples.

If the tests reveal formatting problems with the examples, you can fix them with:
```bash
npm run format-examples
```

[geopandas]: https://geopandas.org/
[sf]: https://r-spatial.github.io/sf/index.html
[projection]: https://github.com/stac-extensions/projection
