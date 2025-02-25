﻿# Docker py4web Scaffold Application

Stateless py4web application setup for Docker development. 

Summary Details:
- Table structure details and uploads are stored directly in the database (pre-setup with PostgreSQL). 
- The uploads stored in database are retrieved efficiently by using the upload table primary key (ID). 
- The client is always sent an encoded hashed version of this ID which can be decoded server side to directly retrieve the corresponding upload ID directly without adding additional indexes to the new generic "upload" table. 
- Uploads are stored in a separate table (distinct from the primary table requiring an upload field) to ensure they are only retrieved/queried when required to ensure their is no corresponding performance degradation for the primary tables.

## Installation
- Install [docker](https://docs.docker.com/get-docker/).

#### Clone this repo:
```
git clone https://github.com/macneiln/docker-py4web-stateless-example.git
```

#### Building Docker Image:
- Open repo in VS Code

#### Building Docker Image:
```
docker-compose -f .\docker-compose.development.yml build
OR
docker-compose -f .\docker-compose.development.yml build --no-cache
```

#### Running Docker Image:
```
docker-compose -f .\docker-compose.development.yml up
```

#### Open In Browser:
```
http://localhost:8000/index
OR
http://127.0.0.1:8000/index
```

#### Debug In VS Code:
- Go to "Run and Debug" tab in VS Code. 
- Click on the pre-setup configuration "Docker py4web: Remote Attach via 5678" to attach to the exposed docker port for debugging.
- Add breakpoints into your VS Code application specific files as required.

#### Implementation Details:
- Core functionality for automatically storing and retrieving uploads from the database (non-filesystem) is implemented within ```db_file_storage.py```.
- ```DBFileStorage``` object is created in ```common.py``` and replaces the default download function implementation.
- ```DBFileStorage``` object is imported in ```models.py``` to call two object functions, one to create the new generic upload table for usage, and one to iterate through and correct the existing tables that have defined standard pydal "upload" field types to ensure they are stored in database. 
- Auto-correction of all database table "upload" fields is accomplished via automatically setting all standard pydal.Field options "custom_retrieve", "custom_store", and "filter_out".
- Upload file retrieval is accomplished via an enhanced version of the bottlepy "static_file" functionality to accommodate database  file retrieval rather than from the file system. 
 
## Usage:
- Simply use the standard pydal.Field functionality for "uploads" such as validators, field definition, etc. and the upload fields will be auto-corrected for you without additional setup due to the usage of the ```DBFileStorage``` object (see example declaration/usage in ```models.py```, ```controllers.py```, and ```index.html```).

## License
[MIT](https://choosealicense.com/licenses/mit/)
