---
layout: default
---

# Regional Coupled Models

A brief summary table of all models and their basic components. This is built automatically from the model file front matter metadata.

<table>
  <thead>
    <tr>
      <th>Model</th>
      <th>source_id</th>
      <th>Extended label</th>
      <th>Atmosphere</th>
      <th>Aerosol</th>
      <th>Land</th>
      <th>Ocean</th>
      <th>coupler</th>
    </tr>
  </thead>
  <tbody>
    {% for model in site.models %}
    <tr>
      <td><a href="{{ site.baseurl }}{{ model.url }}">{{ model.label }}</a></td>
      <td>{{ model.source_id }}</td>
      <td>{{ model.label_extended }}</td>
      <td>{{ model.atmos.label }}</td>
      <td>{{ model.aerosol.label }}</td>
      <td>{{ model.land.label }}</td>
      <td>{{ model.ocean.label }}</td>
      <td>{{ model.coupler.label }}</td>
    </tr>
    {% endfor %}
  </tbody>
</table>

## Atmospheric components

Other tables can be built. E.g. for specific components, showing any info collected in the front matter.

<table>
  <thead>
    <tr>
      <th>Model</th>
      <th>Atmosphere</th>
      <th>Resolution</th>
      <th>Levels</th>
      <th>SW radiation</th>
      <th>LW radiation</th>
      <th>Convection</th>
      <th>Microphysics</th>
      <th>PLB</th>
      <th>...</th>
    </tr>
  </thead>
  <tbody>
    {% for model in site.models %}
    <tr>
      <td><a href="{{ site.baseurl }}{{ model.url }}">{{ model.label }}</a></td>
      <td>{{ model.atmos.label }}</td>
      <td>{{ model.atmos.resolution }}</td>
      <td>{{ model.atmos.levels }}</td>
      <td>{{ model.atmos.physics.radiation-sortwave.label }}</td>
      <td>{{ model.atmos.physics.radiation-longwave.label }}</td>
      <td>{{ model.atmos.physics.convection.label }}</td>
      <td>{{ model.atmos.physics.microphysics.label }}</td>
      <td>{{ model.atmos.physics.boundary-layer.label }}</td>
      <td>...</td>
    </tr>
    {% endfor %}
  </tbody>
</table>


