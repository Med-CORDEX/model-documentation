---
source_id: model2A
label: Model2 A
label_extended: Model 2 title
atmosphere:
  label: WRF v3.8.1
  resolution: 1 degree
  levels: 30
  TOA: 20 hPa
  physics:
    radiation:
      label: Myrad 7.1
      description: blah blah
      reference: Lopez (2002)
      reference-url: http://doi.org/XXXX
    convection:
      label: Kain-Fritsch
      description: Mass flux scheme with ...
      reference: Kain et al. (2009)
      reference-url: http://doi.org/XXXX
    microphysics:
      label: WSM5

ocean:
  label: NEMO v12.4
  resolution: 0.5 degree
  levels: 40
land:
  label: CLM 4.8
  resolution: 0.25 degree
  levels: 20
---

## Atmosphere
{% include resolution-summary.html source-id=page.source_id component="atmosphere" %}

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

### Physics

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

{% include physics-atm.html source-id=page.source_id %}

## Ocean

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

## Land

