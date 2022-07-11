# GOES Extension Specification

- **Title:** NOAA Geostationary Operational Environmental Satellite (GOES)
- **Identifier:** <https://stac-extensions.github.io/goes/v1.0.0/schema.json>
- **Field Name Prefix:** goes
- **Scope:** Item, Collection
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @m-mohr @gadomski

This document explains the NOAA Geostationary Operational Environmental Satellite (GOES) extension
to the [SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec) (STAC) specification.

- [JSON Schema](json-schema/schema.json)
- [Changelog](CHANGELOG.md)
- Examples:
  - [Item example](examples/item.json): Shows the basic usage of the extension in a STAC Item
  - [Collection example](examples/collection.json): Shows the basic usage of the extension in a STAC Collection
- Implementations:
  - [stactools-goes](https://github.com/stactools-packages/goes)
  - [stactools-goes-glm](https://github.com/stactools-packages/goes-glm)

## Item Properties and Collection Fields

| Field Name                  | Type   | Description |
| --------------------------- | ------ | ----------- |
| goes:image-type             | number | One of: `FULL DISK`, `CONUS`, `MESOSCALE` (see below) |
| goes:mesoscale-image-number | number | One of: `1` (Region 1) or `2` (Region 2); Only applies if `goes:image-type` is set to `MESOSCALE` |
| goes:mode                   | number | One of: `3`, `4`, `6` (see below) |
| goes:orbital-slot           | string | One of: `West` or `East` |
| goes:system-environment     | string | One of: `OR`, `OT`, `IR`, `IT`, `IP`, `IS` (see below) |
| goes:processing-level       | string | **DEPRECATED.** Use [processing:level](https://github.com/stac-extensions/processing#suggested-processing-levels) instead. The values (e.g. `L2`) are the same. |

At least one field it required to be used. 
Not all fields apply to all products. Only add the fields that apply to your product.

### Additional Field Information

The following values are recommended to be set for [common metadata fields](https://github.com/radiantearth/stac-spec/blob/master/item-spec/common-metadata.md#instrument):

- `mission`: `GOES`
- `constellation`: `GOES`
- `platform`:
  - For GOES 16/R: `GOES-16`
  - For GOES 17/S: `GOES-17`
  - For GOES 18/T: `GOES-18`
- `instruments` (incomplete list of examples that can be added to the array):
  - `ABI`
  - for GLM: `FM1` (GOES 16) / `FM2` (GOES 17)

#### goes:system-environment

The following values are allowed:

- `OR`: operational system real-time data
- `OT`: operational system test data
- `IR`: test system real-time data
- `IT`: test system test data
- `IP`: test system playback data
- `IS`: test system simulated data

#### goes:image-type

The following values are allowed:

- `FULL DISK`: Near hemispheric earth region centered at the longitude of the sensing satellite.
- `CONUS`: Continental United States coverage region.
  An approximately 3000 km x 5000 km region intended to cover the continental United States
  within the constraints of viewing angle from the sensing satellite.
- `MESOSCALE`: Mesoscale coverage region.
  An approximately 1000 km x 1000 km dynamically centered region in the instrument’s field of regard.
  The particular coverage region associated with a mesoscale product is operator-selected to support
  high-rate temporal analysis of environmental conditions in regions of interest.

#### goes:mode

The following values are allowed:

- `3`: Mode 3 - Consists of one observation of the full disk scene of the earth, three observations
  of the continental United States (CONUS) scene, and thirty observations for each of two distinct
  mesoscale scenes every fifteen minutes, during nominal operations.
- `4`: Mode 4 - Consists of the observation of the full disk scene every five minutes.
- `5`: Mode 5 - Consists of one observation of the full disk scene of the earth, two observations
  of the continental United States (CONUS) scene, and twenty observations for each of two distinct
  mesoscale scenes every ten minutes, during nominal operations.

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
