# GOES Extension Specification

- **Title:** NOAA Geostationary Operational Environmental Satellite (GOES)
- **Identifier:** <https://stac-extensions.github.io/goes/v1.0.0/schema.json>
- **Field Name Prefix:** goes
- **Scope:** Item, Collection
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Pilot
- **Owner**: @m-mohr @gadomski

This document explains the NOAA Geostationary Operational Environmental Satellite (GOES) extension
to the [SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec) (STAC) specification.

- [JSON Schema](json-schema/schema.json)
- [Changelog](CHANGELOG.md)
- Examples:
  - [GLM Item example](examples/item-glm.json): An example for GLM
  - [ABI Item example](examples/item-abi.json): An shortened example for ABI
- Implementations:
  - [stactools-goes](https://github.com/stactools-packages/goes)
  - [stactools-goes-glm](https://github.com/stactools-packages/goes-glm)

## Item Properties and Collection Fields

At least one field it required to be used. 

### Common

These fields can be used both for ABI and GLM.

| Field Name                  | Type   | Description |
| --------------------------- | ------ | ----------- |
| goes:orbital_slot           | string | One of: `West`, `East` or `Test` |
| goes:system_environment     | string | One of: `OR`, `OT`, `IR`, `IT`, `IP`, `IS` (see below) |

### ABI only

These fields can be used if the `instruments` field contains `ABI`. 

| Field Name                  | Type    | Description |
| --------------------------- | ------- | ----------- |
| goes:image_type             | number  | One of: `FULL DISK`, `CONUS`, `MESOSCALE` (see below) |
| goes:mesoscale_image_number | integer | One of: `1` (Region 1) or `2` (Region 2); Only applies if `goes:image_type` is set to `MESOSCALE` |
| goes:mode                   | string | One of: `3`, `4`, `6` (see below) |

### GLM only

These fields can be used if the `instruments` field contains any of the GLM instruments (`FM1`, `FM2`, ...). 

| Field Name                           | Type    | Description |
| ------------------------------------ | ------- | ----------- |
| goes:group_time_threshold            | number  | Lightning group maximum time difference among lightning events in a group (in seconds) |
| goes:flash_time_threshold            | number  | Lightning flash maximum time difference among lightning events in a flash (in seconds) |
| goes:lightning_wavelength            | number  | central wavelength for lightning data (in nm) |
| goes:yaw_flip_flag                   | integer | Flag indicating spacecraft is operating in yaw flip configuration. 0 = upright, 1 = neither, 2 = inverted |
| goes:event_count                     | integer | The number of lightning events in the product |
| goes:group_count                     | integer | The number of lightning groups in the product |
| goes:flash_count                     | integer | The number of lightning flashes in the product |
| goes:nominal_satellite_subpoint_lat  | number  | Nominal satellite subpoint latitude (platform latitude in degrees north) |
| goes:nominal_satellite_subpoint_lon  | number  | Nominal satellite subpoint longitude (platform longitude in degrees east) |
| goes:nominal_satellite_height        | number  | Nominal satellite height above GRS 80 ellipsoid (platform altitude in kilometers) |
| goes:percent_navigated_L1b_events    | number  | After false event filtering, percent of lightning events navigated by instrument (0-1) |
| goes:percent_uncorrectable_L0_errors | number  | Percent data lost due to uncorrectable L0 errors (0-1) |

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

#### goes:system_environment

The following values are allowed:

- `OR`: operational system real-time data
- `OT`: operational system test data
- `IR`: test system real-time data
- `IT`: test system test data
- `IP`: test system playback data
- `IS`: test system simulated data

#### goes:image_type

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
- `6`: Mode 6 - Consists of one observation of the full disk scene of the earth, two observations
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
