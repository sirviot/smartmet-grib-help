# SmartMet Server Grib Snippets

Personal(ish) notes about using SmartMet Server with grib-support.

## Find Parameter Name For Timeseries Requests

You need grid-gui plugin for this. Visit `/grid-gui`

The available selections depend on source data, and all settings are not available for every data producer.

From dropdowns select:

* Producer
* Parameter
* Leve type
* Level (available if Level type is Pressure or Hybrid)
* Forecast type
* Forecast numer
* Geometry

Values for Forecast type:

* 1: Deterministic forecast
* 2: Analysis
* 3: Ensemble forecast, perturbation
* 4: Ensemble forecast, control forecast
* 5: Statistical post processing

Values for Forecast number depict the ensamble member numbe (or something like that) when forecast type is an Ensamble.

When all is selected you can find the appropriate parameter name from text field named **FMI Key**. It is a colon separated id that can be used with timeseries requests.

For example `T-K:ECG:1008:1:0:1`.

* `T-K` is the parameter temperature in kelvins
* `ECG` is the data producer
* `1008` is the geometry and
* `1` is the level type
* `:0:1` at the end don't really mean anything when level type is `1`, but are mandatory.

Example 2: `T-K:ECG:1007:2:25000:1`

* Same parameter, producer and geometry as previously
* `2` as Level Type, meaning Pressure
* `25000` as Level, required by Level Type. Pascals I think
* `:1` in the end, mandatory but has no real meaning

Example 3: `T-K:ECGEPS:1068:3:30:3:6`

* `T-K` Parameter name
* `ECGEPS` Producer name
* `1068` Geometry
* `3` Level Type (Hybrid)
* `30` Level number (unit is a mystery)
* `3` Forecast Type (Ensemble forecast, perurbation)
* `6` Number of the ensemble memeber

This example has all the key items that are user defineable. Very exiting.

## Find Available Data Producers

Instead of `what` grib-version uses parameter `method`.
So `/admin?what=qengine` becomes `/grid-admin?method=...` and there are several 
method values for accomplishing different things.

### HTML Table

There is a `page` querystring field that can have a value of `producers`.

`/grid-admin?page=producers`

This will return available producers as a HTML table.

### CSV

Returns the available producer identifiers 
and descriptions.

URL: `/grid-admin?method=getProducerInfoList&sessionId=0`

Response will be something like:

```text
result=0
producerInfoHeader=producerId;name;title;description;flags;producerId
producerInfo=1;ASDFBLEND;ASDFBLEND;My blend of models bias fields;0;100
...
```

First row. I don't know what it should be used for.
The `producerId` is listed twice on the header row, and in the data it has a 
value of both `1` and `100`.

If you already know the value of `producerId` you can get the rest of the
information:

URL: `/grid-admin?method=getProducerInfoById&producerId=1&sessionId=0`

The response will be similar to previous, but with only one producerInfo row.

```text
result=0
producerInfoHeader=producerId;name;title;description;flags;producerId
producerInfo=1;ASDFBLEND;ASDFBLEND;My blend of models bias fields;0;100
```
