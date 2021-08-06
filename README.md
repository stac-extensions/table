# Table Extension Specification

- **Title:** Parquet Table
- **Identifier:** <https://stac-extensions.github.io/parquet-table/v1.0.0/schema.json>
- **Field Name Prefix:** parquet
- **Scope:** Item, Collection
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @TomAugspurger

This document explains the table Extension to the [SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec) (STAC) specification.
This is the place to add a short introduction.

- Examples:
  - [Item example](examples/item.json): Shows the basic usage of the extension in a STAC Item
  - [Collection example](examples/collection.json): Shows the basic usage of the extension in a STAC Collection
- [JSON Schema](json-schema/schema.json)
- [Changelog](./CHANGELOG.md)

## Item Properties and Collection Fields

|        Field Name        |                          Type                           |                       Description                       |
| ------------------------ | ------------------------------------------------------- | ------------------------------------------------------- |
| table:columns            | [ [Column Object](#column-object) ]                     | **REQUIRED**. A list of objects describing each column. |
| table:geo_arrow_metadata | [Geo Arrow Metadata Object](#geo-arrow-metadata-object) | The geospatial metadata for this table.                 |

### Column Object

Column objects contain information about each colum in the table.

| Field Name  |       Type       |                                                        Description                                                         |
| ----------- | ---------------- | -------------------------------------------------------------------------------------------------------------------------- |
| name        | number           | **REQUIRED**. The column name                                                                                              |
| description | string           | Detailed multi-line description to explain the dimension. CommonMark 0.29 syntax MAY be used for rich text representation. |
| metadata    | Map<string, Any> | Additional metadata for the column                                                                                         |

### Geo Arrow Metadata Object

|   Field Name   |                         Type                         |                                Description                                |
| -------------- | ---------------------------------------------------- | ------------------------------------------------------------------------- |
| primary_column | string                                               | The primary geometry column name. Used by systems like geopandas.         |
| schema_version | string                                               | **REQUIRED**. The version of geo-arrow-spec the dataset was written with. |
| creator        | Map<string, [Creator Object](#creator-object)>       | Information describing the system used to write the dataset.              |
| columns        | Map<string, [Geo Column Object](#geo-column-object)> | The geometry information for each geometry column.                        |

### Creator Object

The Creator Object captures information about the library or system that was used to generate this dataset.

| Field Name |  Type  |                            Description                            |
| ---------- | ------ | ----------------------------------------------------------------- |
| library    | string | **REQUIRED**. Name of the system used to generate the dataset.    |
| version    | string | **REQUIRED**. Version of the system used to generate the dataset. |

### Geo Column Object

Additional geospatial metadata for geometry columns.

| Field Name |     Type     |                                           Description                                           |
| ---------- | ------------ | ----------------------------------------------------------------------------------------------- |
| crs        | string       | **REQUIRED**. The WKT representation of the column's coordinate reference system, if any.       |
| encoding   | string       | How the geometries are encoding. Currently, this is likely the string "WKB".                    |
| bbox       | \[ number \] | Bounding Box of the asset represented by this Item, formatted according to RFC 7946, section 5. |

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
