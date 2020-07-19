Merging JSON files
===================
Multiple signal, parameter, pin, etc JSON files from different sources can be merged into one JSON file
for further processing. This allows including ready general purpose signal JSONs, signal maps from
other devices, etc, without copy/pasting those in. The script is used as:

::

   python3 merge_json.py /tmp/infile1.json /tmp/infile2.json -o /tmp/outfile.json

This script is in:

::

   /coderoot/eosal/scripts/merge_json.py

Files to merge are typically listed in merge.json. The merge.json files are used by generate_c_code.py script
to control merging.

200718, updated 18.7.2020/pekka