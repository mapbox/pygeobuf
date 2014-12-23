## Geobuf

Geobuf is a compact binary geospatial format for lossless compression of GeoJSON and TopoJSON data.
Advantages over GeoJSON and TopoJSON:

- **Very compact**: typically makes GeoJSON 6-8 times smaller and TopoJSON 2-3 times smaller.
- Smaller even when comparing gzipped sizes: 2-2.5x compression for GeoJSON and 20-30% for TopoJSON.
- Easy **incremental parsing** &mdash; you can get features out as you read them,
without the need to build in-memory representation of the whole data.
- **Partial reads** &mdash; you can read only the parts you actually need, skipping the rest.
- Trivial **concatenation**: you can concatenate many Geobuf files together and they will form a valid combined Geobuf file.
- Encoding/decoding is potentially faster and takes less memory than native JSON encoding/decoding (i.e. in Web browsers).

#### Sample compression sizes

                    | normal    | gzipped
------------------- | --------- | --------
us-zips.json 	    | 101.85 MB | 26.67 MB
us-zips.pbf         | 12.24 MB  | 10.48 MB
us-zips.topo.json   | 15.02 MB  | 3.19 MB
us-zips.topo.pbf    | 4.85 MB   | 2.72 MB
idaho.json          | 10.92 MB  | 2.57 MB
idaho.pbf           | 1.37 MB   | 1.17 MB
idaho.topo.json     | 1.9 MB    | 612 KB
idaho.topo.pbf      | 567 KB    | 479 KB

### pygeobuf

This repository is the first encoding/decoding implementation
of the [new, significantly improved revision](https://github.com/mapbox/geobuf/issues/27) of Geobuf (in Python).
It serves as a prototyping playground for future (faster) implementations.

### Usage

Command line:

```bash
./encode.py sample.json # -> sample.pbf
./decode.py sample.pbf  # -> sample.pbf.json
```

As a module:

```python
import geobuf

pbf = geobuf.encode(my_json) # GeoJSON or TopoJSON -> Geobuf string
my_json = geobuf.decode(pbf) # Geobuf string -> GeoJSON or TopoJSON
```

The `encode` function accepts a dict-like object, for example the result of `json.loads(json_str)`.

Both `encode.py` and `geobuf.encode` accept two optional arguments:

- **precision** &mdash; max number of digits after the decimal point in coordinates, `6` by default.
- **dimensions** &mdash; number of dimensions in coordinates, `2` by default.

### Tests

```bash
python test.py -v
```

The tests run through all `.json` files in the `fixtures` directory,
comparing each original GeoJSON with an encoded/decoded one.
