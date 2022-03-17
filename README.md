# Template Extension Specification

- **Title:** MLHub Dataset
- **Identifier:** <https://stac-extensions.github.io/mlhub-dataset/v1.0.0/schema.json>
- **Field Name Prefix:** mlhds
- **Scope:** Collection
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @KennSmithDS

This document explains the MLHub Dataset Extension to the [SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec) (STAC) specification. An MLHub Dataset is a STAC **Catalog**, in that it contains multiple children **Collection** and related **Item** STAC objects. However, the use-case is specific to when a datset is published onto the Radiant MLHub web page and API. There are additional metadata properties which are stored in the Catalog which are then used to construct a complete row for insertion into the `mlhub.dataset` table in our private PostgreSQL database on the MLHub back-end. These properties in the database are then parsed by the front-end and rendered in HTML. They are properties specific to Radiant MLHub's dataset publishing workflow only.

- Examples:
  - [Collection example](examples/collection.json): Shows the basic usage of the extension in a STAC Collection
- [JSON Schema](json-schema/schema.json)
- [Changelog](./CHANGELOG.md)

## Collection Fields

| Field Name             | Type                                       | Description                                                       |
| ---------------------- | ------------------------------------------ | ----------------------------------------------------------------- |
| mlhds:creator_contact  | [CreatorContact](#CreatorContact%20Object)         | **REQUIRED**. The primary creator and point of contact            |
| mlhds:publications     | \[[ExternalResource](#ExternalResource%20Object)]  | List of the publications associated with the dataset              |
| mlhds:tools_apps       | \[[ExternalResource](#ExternalResource%20Object)]  | List of the tools and applications used in dataset generation     |
| mlhds:tutorials        | \[[ExternalResource](#ExternalResource%20Object)]  | List of tutorials such as jupyter notebooks or github repos       |
| mlhds:tags             | \[string]                                  | **REQUIRED**. List of keywords to populate the webpage tag filter |
| mlhds:long_description | string                                     | **REQUIRED**. Multiparagraph descript that appears on the webpage |

### CreatorContact Object

This object provides reference to the primary point of contact for the dataset and their organizational affiliation, e.g. their email and name of research institution. Each value in both the `contact` and `creator` fields are strings, but both can also be single comma delineated strings to add more contact and creator details.

| Field Name  | Type   | Description                                                         |
| ----------- | ------ | ------------------------------------------------------------------- |
| contact     | string | **REQUIRED**. Email address(es) of the primary points of contact    |
| creator     | string | **REQUIRED**. Name and URL of affiliated institutions (in markdown) |

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
