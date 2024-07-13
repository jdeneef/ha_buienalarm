# HomeAssistant precipitation sensors and graph

Created this for my own use, because I wasn't happy with the available options.

Note that data is used from [buienalarm](https://www.buienalarm.nl), I have no connection whatever with buienalarm, just using the open api as made available by buienalarm. This setup basically mimicks the buienalarm app, which is exactly the intended idea ;-) It can also easily be extended to eg [buienradar](https://www.buienradar.nl), so the setup is not limited to buienalarm.

**This is working on my homeassistant environment, that's it. Shared for tesing and usage at your own risk!**

**Installation requires copying files to your homeassistant configuration and updating home assistant configuration, so some homeassistant knowledge is required**

What does it do:

- Create sensors for precipitation forecast
- add a true/false sensor triggering to true when rain is expected coming 5-30 mins approx, used for eg opening sun screens etc.
- add a graph showing the expected rain for the next 2 hours, sort of following buienalarm app

# Installation
This setup adds a [RESTful integration](https://www.home-assistant.io/integrations/rest/) to your homeassistant, polling the api every 5mins. To make the data visible I added a plotly graph showing sort of the same chart as the buienalarm app.

## Setup of Sensors 
Copy `buienalarm.yaml` and ensure the file is read from your `configuration.yaml`. Eg, drop the file in `/config/packages/` and use the `config` lines below to include the `packages` directory:

```yaml
homeassistant:
  packages: !include_dir_named packages
```

Followed by a reboot your homeassistant instance. 

The integration will automatically use your Home zone `zone.home` location to pull in `api` data.

Adding the file in your config creates sensors:

Sensor|Attribute|Unit|Comment
---|---|---|---
`sensor.buienalarm_precipitation_intensity`|n.a.|mm/h|the sensor itself holds the precipitation intensity, attributes hold the api data
`sensor.buienalarm_precipitation_intensity`|`precip`|n.a.|table with precipitation values per period `delta`
`sensor.buienalarm_precipitation_intensity`|`start`|timestamp|start time of `precip` table
`sensor.buienalarm_precipitation_intensity`|`delta`|seconds|measurement period of `precip` table
`sensor.buienalarm_precipitation_expected`|n.a.|Bool|returns True when rain expected next 4 to 6 `delta` periods

## Setup of Graph
The chart is plotted using [plotlygrap](https://github.com/dbuezas/lovelace-plotly-graph-card). Install this plugin using [HACS](https://hacs.xyz) if not already available.

Next copy text from this `plotlygraph.yaml` and drop this in a manual card on any of your dashboards and you are done. The graph is adjustable, eg if you want to adjust the horizontal lines for light and medium rain, just adjust the `y0` and `y1` in the `shapes` section:

```yaml
  shapes:
    - type: line
      xref: paper
      y0: 0.1
      y1: 0.1
```

## Sample precipitation graph and history

![Example raining](rain.png "rain ;-)")
![Example rain expected sensor](rain_expected.png "Rain expected sensor")


