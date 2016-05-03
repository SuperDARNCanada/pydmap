# pydmap
Python based library for reading and writing SuperDARN data in the dmap format


The only dependency this project has is Numpy


This project attempts to recreate the DMAP file encoding and decoding from RST. This version requires no dependencies on RST and can easily be imported to your Python scripts. This project is much more robust in error handling than the C version so it can be used to parse radar data from sockets as well.

To work with the package there are simple to use functions that abstract the DMAP complexity for the user. To read and decode the data from any DMAP formatted file, use:

parse_dmap_format_from_file(filepath,raw_dmap = False)

or if working on data from a buffer(such as data from a network socket) then use:

parse_dmap_format_from_stream(stream, raw_dmap = False)


Both of these functions return a list of dictionaries that hold the data for each parsed DMAP record, or if raw_dmap is set True then it returns the underlying Raw_Dmap object that holds data while it is being parsed.


The underlying algorithm for writing and encoding data to a DMAP file is slightly more complicated. When determining data types to be encoded, dmap writing will default to use the Python type, unless a user defined dictionary is supplied that overrides what type of data something is. This is neccessary to order to make files compatible with the C reading routines. For example gflg is an array of 0s and 1s which Python stores as ints. However, this needs to be written out as chars to stay compatible for the C version. There is a function that abstracts this complexity for .iqdat,.rawacf,.fitacf, and .map types:

dicts_to_file(data_dicts,file_path,file_type='')

This function takes a list of dictionaries to write, a file path to write to, and a file type to type override Python's default. Options are 'iqdat', 'rawacf', 'fitacf', and 'map'. 

To create a custom writing routine, a user can create a RawDmapWrite object and pass it your own user defined dictionary for types similar to above wrapper function. For type formats, refer to the struct module, or if using Numpy arrays then refer to the Numpy dtypes.

There are a set of basic unit tests if you make changes. 



