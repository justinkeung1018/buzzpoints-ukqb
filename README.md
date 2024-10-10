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
Clone [this repo](https://github.com/justinkeung1018/buzzpoints-ukqb/), which is a fork of [this excellent buzzpoint web app](https://github.com/JemCasey/buzzpoints/). The main differences between this fork and the original repo are:
- We pass in the filename of the database file via an environment variable
- The `data` directory contains all buzzpoint data from all tournaments (clear and unclear)

Here are the steps:
1. Take the `database.db` file generated from the previous step and rename it to something other than `database.db`, e.g. `20231209-arcadia.db`. Copy the file to the `data` directory.
2. Commit and push the new file to `main`.
3. Sign into Vercel [here](https://vercel.com/).
4. Create a new project and import the repo you just cloned. <img width="1917" alt="Screenshot 2024-10-10 at 23 29 18" src="https://github.com/user-attachments/assets/4aed4f6d-4699-4799-81f3-ece7cf850ec8">
5. Give the project a name. **Very important: set an environment variable called `DATABASE` with the name of the database file as value.** <img width="1916" alt="Screenshot 2024-10-10 at 23 36 19" src="https://github.com/user-attachments/assets/d91d3558-6acd-48f6-bd65-4c113c48f5fd">

Voila! You can change the domain name of the deployed app by clicking on the project > Settings > Domains.

### Troubleshooting
If you are trying to run the web app locally and are having issues with `npm install`, specifically, if `node-gyp` is throwing errors, try the following:
- Downgrade `npm` to `v18.5.0` using a version manager like `nvm`
- Make sure the whole path does not contain any spaces, e.g. if your path is `/Users/johndoe/code repos`, remove the space between `code` and `repos`.
