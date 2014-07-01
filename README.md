TMP102 Hardwarepins Library
===========================

This library provides an interface to the TMP102 I2C temperature sensor.

* [Product information and data sheet](http://www.ti.com/product/TMP102/description)

Getting Started
---------------

This library can be imported into Kinoma Studio and added to an application's build path settings. You can then use the TMP102 module to create an object that polls the sensor's temperature value and returns the result to the application.

The following objects can be created using the TMP102 module in this library:

* [TMP102](#tmp102)

Example
-------

The following example demonstrates how to instantiate a new TMP102 object and poll the temperature value from the sensor.

```xml
<program xmlns="http://www.kinoma.com/kpr/1">
    <require id="TMP102" path="TMP102" />
    <script>
        <![CDATA[
            // create a new TMP102 instance and specify the 
            // I2C data and clock pins on the device
            var tmp102 = new TMP102.TMP102( 27, 29 );

            // initialize the sensor and start polling
            tmp102.init();
            tmp102.poll( 100, "milliseconds", "/tmp102/value" );
        ]]>
    </script>
    <handler path="/tmp102/value">
        <behavior>
            <method id="onInvoke" params="handler, message">
                <![CDATA[
                    var data = message.requestObject;

                    if( data != null )
                        trace( temp.celsius.toFixed(1) + "°C" + " / " + temp.fahrenheit.toFixed(1) + "°F" + "\n" );
                ]]>
            </method>
        </behavior>
    </handler>
</program>
```

API Reference
-------------

The following reference describes the object interfaces defined in the TMP102 module.

TMP102
------

The TMP102 object interface is used to get temperature values from the TMP102 sensor.

Creating a new instance of a TMP102 object:

```javascript
var tmp102 = new TMP102.TMP102( sda, scl, addr, config );
```

Initializing the TMP102 object instance:

```javascript
tmp102.init();
```

To request the current temperature from the sensor, you must specify a callback to receive the result. The callback is the name of a handler that is implemented in your program or module:

```javascript
tmp102.get( callback );
```

To continuously poll the temperature from the sensor, you must specify the polling interval and time units, as well as the callback handler to receive the temperature value:

```javascript
tmp102.poll( time, units, callback, skipFirst );
```

>The units parameter must be one of the following string literal values:
>
>* milliseconds
>* seconds
>* minutes
>* microseconds
>* nanoseconds
>* hours
>* days

When the TMP102 object's callback handler is invoked, the result will be passed in the message requestObject property. The temperature will be passed in a data structure as a raw sensor value, and also converted to celsius and fahrenheit. The structure of the result object will be as follows:

```javascript
{ raw:342, celsius:21.375, fahrenheit:70.475 }
```
