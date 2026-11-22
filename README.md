# AliceVision Sensor Database

A database of camera sensor widths, used by [AliceVision](https://github.com/alicevision/AliceVision)
and [Meshroom](https://github.com/alicevision/Meshroom). It lets AliceVision turn the focal length in
millimetres from the image metadata (EXIF) into a focal length in pixels, which is the starting point
for camera calibration (see the `CameraInit` node).

This database used to live in the AliceVision source tree (`src/aliceVision/sensorDB/cameraSensors.db`).
Its full history was kept when it moved to this repository.

## File format

`cameraSensors.db` is a plain text file with one camera per line:

```
brand;model;sensorWidth;sources
```

| Field         | Description                                                                        |
|---------------|------------------------------------------------------------------------------------|
| `brand`       | Camera make, as written in the EXIF `Make` tag.                                    |
| `model`       | Camera model, as written in the EXIF `Model` tag.                                  |
| `sensorWidth` | Sensor width in millimetres, using `.` as the decimal separator.                   |
| `sources`     | Where the value comes from, comma-separated (e.g. `dpreview,digicamdb`).           |

Example:

```
Canon;Canon EOS 5D Mark IV;36.0;digicamdb,dpreview,dxomark,imaging-resource
```

Empty lines and lines starting with `#` are ignored.

## Using the database with AliceVision

AliceVision reads the file given by the `ALICEVISION_SENSOR_DB` environment variable:

```bash
export ALICEVISION_SENSOR_DB=/path/to/sensordb/cameraSensors.db
```

## Contributing

To add or fix a camera:

1. Edit `cameraSensors.db`. Copy the brand and model exactly as they appear in the image metadata
   (for example with `exiftool -Make -Model image.jpg`).
2. Give the source of the sensor width in the last field. If you measured or looked up the value
   yourself, use `usercontribution`.
3. Keep the file sorted:

   ```bash
   python sensorDatabaseSort.py -i cameraSensors.db -o cameraSensors.db
   ```

4. Open a pull request.

## License

This database is distributed under the [Mozilla Public License 2.0](LICENSE), like AliceVision.
