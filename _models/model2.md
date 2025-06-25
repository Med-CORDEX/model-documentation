---
name: model2A
source_id: model2A
label: Model2 A
label_extended: Model 2 title
atmos:
  name: WRF v3.8.1
  resolution: 1 degree
  levels: 30
  TOA: 20 hPa
  physics:
    radiation:
      name: Myrad 7.1
      description: blah blah
      references:
        - citation: Lopez (2002)
          doi: http://doi.org/XXXX
    convection:
      name: Kain-Fritsch
      description: Mass flux scheme with ...
      reference: Kain et al. (2009)
      reference-url: http://doi.org/XXXX
    microphysics:
      name: WSM5

ocean:
  name: NEMO v12.4
  resolution: 0.5 degree
  levels: 40
land:
  name: CLM 4.8
  resolution: 0.25 degree
  levels: 20
---

## Atmosphere
{% include resolution-summary.html name=page.name component="atmosphere" %}

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

### Physics

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

{% include physics.html component="atmos" name=page.name %}

## Ocean

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

## Land

