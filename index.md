---
layout: default
---

A brief summary table of all Med-CORDEX models and their basic components:
<p style="color:red; font-weight:bold;">This site is under construction. Model details cannot be trusted at all.</p>

<table>
  <thead>
    <tr>
      <th>Model</th>
      <th></th>
      <th>Atmosphere</th>
      <th>Aerosol</th>
      <th>Land</th>
      <th>River</th>
      <th>Ocean</th>
      <th>Ocn biogeochemistry</th>
      <th>coupler</th>
    </tr>
  </thead>
  <tbody>
    {% for model in site.models %}
    <tr>
      <td><a href="{{ site.baseurl }}{{ model.url }}">{{ model.name }}</a></td>
      <td>{{ model.label_extended }}</td>
      <td>{{ model.atmosphere.name }}</td>
      <td>{{ model.aerosol.name }}</td>
      <td>{{ model.land_surface.name }}</td>
      <td>{{ model.land_surface.physics.river_routing.name }}</td>
      <td>{{ model.ocean.name }}</td>
      <td>{{ model.ocean_biogeochemistry.name }}</td>
      <td>{{ model.coupler.name }}</td>
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
      <th>PBL</th>
      <th>...</th>
    </tr>
  </thead>
  <tbody>
    {% for model in site.models %}
    <tr>
      <td><a href="{{ site.baseurl }}{{ model.url }}">{{ model.name }}</a></td>
      <td>{{ model.atmosphere.name }}</td>
      <td>{{ model.atmosphere.native_horizontal_grid.resolution_x }}
          {{ model.atmosphere.native_horizontal_grid.horizontal_units }}</td>
      <td>{{ model.atmosphere.native_vertical_grid.n_z }}</td>
      <td>{{ model.atmosphere.physics.radiation_shortwave.name }}</td>
      <td>{{ model.atmosphere.physics.radiation_longwave.name }}</td>
      <td>{{ model.atmosphere.physics.convection.name }}</td>
      <td>{{ model.atmosphere.physics.microphysics.name }}</td>
      <td>{{ model.atmosphere.physics.boundary-layer.name }}</td>
      <td>...</td>
    </tr>
    {% endfor %}
  </tbody>
</table>

