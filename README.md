# MLHub Extension Specification

- **Title:** MLHub
- **Identifier:** <https://raw.githubusercontent.com/radiantearth/mlhub-dataset/main/json-schema/schema.json>
- **Field Name Prefix:** mlhub
- **Scope:** Catalog
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @KennSmithDS

This document explains the MLHub Extension to the [SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec) (STAC) specification. An MLHub Extension is a STAC **Catalog**, in that it contains multiple children **Collection** and related **Item** STAC objects. However, the use-case is specific to when a datset is published onto the Radiant MLHub web page and API. There are additional metadata properties which are stored in the Catalog which are then used to construct a complete row for insertion into the `mlhub.dataset` table in our private PostgreSQL database on the MLHub back-end. These properties in the database are then parsed by the front-end and rendered in HTML. They are properties specific to Radiant MLHub's dataset publishing workflow only.

- Examples:
  - [Catalog example](examples/catalog.json): Shows the basic usage of the extension in a STAC Catalog
- [JSON Schema](json-schema/schema.json)
- [Changelog](./CHANGELOG.md)

## Catalog Fields

For Catalogs, the fields are placed on the top level of the Catalog.

| Field Name             | Type                                             | Description                                                       |
| ---------------------- | ------------------------------------------------ | ----------------------------------------------------------------- |
| mlhub:creator_contact  | [CreatorContact](#creatorcontact-object)         | **REQUIRED**. The primary creator and point of contact            |
| mlhub:tags             | \[string]                                        | **REQUIRED**. List of keywords to populate the webpage tag filter |
| mlhub:date_added       | string                                           | **REQUIRED**. Datetime for when the dataset was added to MLHub    |
| mlhub:date_modified    | string                                           | **REQUIRED**. Datetime for the last time dataset was modified     |
| mlhub:processing_level | number                                           | **REQUIRED**. Identifier indicating the level at which the data in the collection are processed |
| mlhub:collection_progress | string                                        | **REQUIRED**. Describes the production status of the dataset      |
| mlhub:science_keywords | \[[ScienceKeyword](#sciencekeyword-object)]      | **REQUIRED**. List of keywords chosen from a controlled keyword hierarchy maintained in the [Keyword Management System (KMS)](https://earthdata.nasa.gov/earth-observation-data/find-data/idn/gcmd-keywords)
| mlhub:publications     | \[[ExternalResource](#externalresource-object)]  | List of the publications associated with the dataset              |
| mlhub:tools_apps       | \[[ExternalResource](#externalresource-object)]  | List of the tools and applications used in dataset generation     |
| mlhub:tutorials        | \[[ExternalResource](#externalresource-object)]  | List of tutorials such as jupyter notebooks or github repos       |

### CreatorContact Object

This object provides reference to the primary point of contact for the dataset and their organizational affiliation, e.g. their email and name of research institution. Each value in both the `contact` and `creator` fields are strings, but both can also be single comma delineated strings to add more contact and creator details.

| Field Name  | Type   | Description                                                         |
| ----------- | ------ | ------------------------------------------------------------------- |
| contact     | string | **REQUIRED**. Email address(es) of the primary points of contact    |
| creator     | string | **REQUIRED**. Name and URL of affiliated institutions (in markdown) |

### Dataset Tags

Tags are a list of keywords added to the dataset's metadata to describe or categorize the catalog, combining one or more words, that improve the user experience by allowing them to filter all the datasets seen on Radiant MLHub catalog to a speicif subset of types, e.g. satellite constellation, machine learning use-case, or data provider.

### Dataset Added and Modified Dates

In addition to the `datetime` properties required by the STAC specification for Collections and Items, such as `datetime`, `start_datetime` and `end_datetime` as well as the `interval` values of `extent`, we need to capture the date that the dataset was added to Radiant MLHub and the most recent date that a dataset was updated. Therefore, this extension will contain two additional fields, `date_added` and `date_modified`. These two fields will follow the same formatting convention as other datetime objects in the STAC specification. All times in STAC metadata should be in [Coordinated Universal Time (UTC)](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) and be formatted according to [RFC 3339 section 5.6](https://tools.ietf.org/html/rfc3339#section-5.6).

### ScienceKeyword Object

[GCMD Keywords](https://wiki.earthdata.nasa.gov/display/CMR/GCMD+Keyword+Access) are organized using a multi-level hierarchical structure. This hierarchical structure provides a framework by which concepts can be classified and related.

| Field Name   | Type   | Description |
| ------------ | ------ | ----------- |
| category     | string | **REQUIRED**. Defines disciplines and high level concepts |
| topic        | string | **REQUIRED**. Defines disciplines and high level concepts |
| term         | string | **REQUIRED**. Define subject areas and parameters |
| variable_level_1 | string | Define subject areas and parameters |
| variable_level_2 | string | Define subject areas and parameters |
| variable_level_2 | string | Define subject areas and parameters |
| detailed_variable | string | Uncontrolled values that can be added by users to more specifically describe data |

### ExternalResource Object

This is a more abstract object describes an external resource that is somehow relevant to the dataset being published. This object is used by the `publications`, `tutorials`, and `tools and applications` fields, as they all share the same key/value pairs of nested attributes.

| Field Name   | Type   | Description |
| ------------ | ------ | ----------- |
| url          | string | URL for an external resource, e.g. a link to GitHub page, or PDF, etc. |
| title        | string | The title of the linked document or resource                           |
| author_url   | string | Webpage or social media account of the author(s)                       |
| author_name  | string | Name(s) of the author(s) who created the external resource referrenced |

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
