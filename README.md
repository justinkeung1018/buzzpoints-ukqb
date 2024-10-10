# How to load MODAQ buzzpoints and packets
The process is broken into three steps:
1. Convert the `.docx` packets into JSON
2. Generate a SQLite database file that contains both buzzpoint and packet data
3. Load the database file into the web app instance

## Step 1: convert the `.docx` packets into JSON
We use the [QBReader packet parser](https://github.com/qbreader/packet-parser). Clone the repo and follow the instructions in the README there.
### Notes
Pass in the `-b` flag when running the `packet-parser.py` Python file.

## Step 2: generate a SQLite database file that contains both buzzpoint and packet data
We use the tool [here](https://github.com/JemCasey/buzzpoint-migrator). Clone the repo and follow the instructions in the README there.
### Notes
To make sure categories render properly in the web app, when filling out the `index.json` in every subfolder of `question_sets`, i.e. in the example given,
```
.
|-- question_sets
|   |-- 2023_Chicago_Open
|   |   `-- editions
|   |       `-- 2023-08-05
|   |           `-- packet_files
|   |           `-- index.json
|   |   `-- index.json --- we are talking about this file
`-- tournaments
    |-- 2023_Chicago_Open
    |   `-- game_files
    |   `-- index.json
```
make sure `metadataStyle` is set to 5 (i.e. we tell the tool it should expect QBReader style categories) since the QBReader packet parser outputs categories parsed in the QBReader style: 
```
{
    "name": "2023 Chicago Open",
    "slug": "2023-chicago-open",
    "difficulty": "Open",
    "metadataStyle": 5 --- make sure this is 5
}
```

## Step 3: load the database file into the web app instance
TODO
