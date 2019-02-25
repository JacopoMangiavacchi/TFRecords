# TFRecords (.tfrecord) file format Builder and Reader

[![Build Status](https://dev.azure.com/edgeWonders/TFRecords/_apis/build/status/JacopoMangiavacchi.TFRecords)](https://dev.azure.com/edgeWonders/TFRecords/_build/results?buildId=latest)


The TFRecords format is briefly documented
[here](https://www.tensorflow.org/api_guides/python/python_io#tfrecords_format_details),
and described as the recommended format for feeding data into TenosorFlow
[here](https://www.tensorflow.org/api_guides/python/reading_data#standard_tensorflow_format)
and
[here](https://www.tensorflow.org/api_guides/python/io_ops#example_protocol_buffer).

This library facilitates producing data in the TFRecord format directly in
node.js. The library is not "official" - it is not part of TensorFlow, and it is
not maintained by the TensorFlow team.

An existing TFRecord Library and NPM Package (https://www.npmjs.com/package/tfrecord) already provide TFRecord API support for the Node.js platform.  This Library provide a TFrecords Javascript API solution that support both Browser and Node.js runtime.

## Usage - Build a TFRecords Buffer

The example below covers recommended API usage for generating a TFRecords buffer.

```javascript
// Generate TFRecord
const builder = new TFRecordsBuilder();

builder.addFeature("image/width", FeatureType.Int64, 1024);
builder.addFeature("image/height", FeatureType.Int64, 800);
builder.addFeature("image/filename", FeatureType.String, "name");
builder.addFeature("image/encoded", FeatureType.Binary, imageBuffer);
builder.addFeature("image/format", FeatureType.String, "jpeg");
builder.addArrayFeature("image/object/bbox/xmin", FeatureType.Float, 0.0);
builder.addArrayFeature("image/object/bbox/ymin", FeatureType.Float, 0.0);
builder.addArrayFeature("image/object/bbox/xmax", FeatureType.Float, 1.0);
builder.addArrayFeature("image/object/bbox/ymax", FeatureType.Float, 1.0);
builder.addArrayFeature("image/object/class/text", FeatureType.String, "tag1");
builder.addArrayFeature("image/object/class/label", FeatureType.Int64, 0);

// Build single TFRecord
const tfrecord = builder.build()

// Get TFRecords buffer
const tfRecords = TFRecordsBuilder.buildTFRecords([tfrecord]);
```

## Usage - Read a TFRecords Buffer

The example below covers recommended API usage for read a TFRecords buffer.

```javascript
const reader = new TFRecordsReader(tfrecords);

const width = reader.getFeature(0, "image/width", FeatureType.Int64) as number;
const height = reader.getFeature(0, "image/height", FeatureType.Int64) as number;
const xminArray = reader.getArrayFeature(0, "image/object/bbox/xmin", FeatureType.Float) as number[];
const yminArray = reader.getArrayFeature(0, "image/object/bbox/ymin", FeatureType.Float) as number[];
const xmaxArray = reader.getArrayFeature(0, "image/object/bbox/xmax", FeatureType.Float) as number[];
const ymaxArray = reader.getArrayFeature(0, "image/object/bbox/ymax", FeatureType.Float) as number[];
const textArray = reader.getArrayFeature(0, "image/object/class/text", FeatureType.String) as string[];

```

