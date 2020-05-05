# SmartMet Server Grib Snippets

Personal(ish) notes about using SmartMet Server with grib-support.

## Admin Plugin

Instead of `what` grib-version uses parameter `method`.
So `/admin?what=qengine` becomes `/grid-admin?method=...` and there are several 
method values for accomplishing different things.

### Find Available Data Producers

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
