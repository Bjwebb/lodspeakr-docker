
Introduction
------------

This is the code (will) run the http://resourceprojects.org website.

It is deployed using a Docker container, which is built on Docker hub at:
https://registry.hub.docker.com/u/bjwebb/resourceprojects.org-frontend/

Pre-requisites
--------------

To run lodspeakr you will need an instance of virtuoso running.

```
docker run -p 127.0.0.1:8890:8890 --name=virtuoso -v /usr/local/var/lib/virtuoso/db caprenter/automated-build-virtuoso 
```

Running from docker hub
-----------------------

```
# Pull the latest version
docker pull bjwebb/resourceprojects.org-frontend
# Run it
docker run --publish=127.0.0.2:80:80 --name=lodspeakr-container --rm --link virtuoso:virtuoso -e BASE_URL=http://127.0.0.2/ bjwebb/resourceprojects.org-frontend
```

Development
-----------

```
docker build -t lodspeakr .
docker run --publish=127.0.0.2:80:80 --name=lodspeakr-container --rm --link virtuoso:virtuoso -e BASE_URL=http://127.0.0.2/ lodspeakr
```

Then visit http://127.0.0.2/

If you wish to enable debug mode, also add `-e DEBUG=true` before `lodspeakr`. This turns on PHP debugging, and also allows you to see extra logs at http://127.0.0.2/logs

Running a docker build each time you change a file can be a bit too slow for development pursposes, so instead you can mount the components directory in the container from the host filesystem:

```
docker run --publish=127.0.0.2:80:80 --name=lodspeakr-container --rm --link virtuoso:virtuoso -e BASE_URL=http://127.0.0.2/  -v `pwd`/components:/var/www/html/lodspeakr/components lodspeakr
```

Uploading data into virtuoso
----------------------------

**Note:** The prefer method of loading data is using the ETL load container at https://github.com/NRGI/resource-projects-etl/#running-with-docker , however, if you you want to you can also upload data manually.

* Go to http://localhost:8890/conductor/
* Log in. Default username/password is dba/dba
* Click the linked data tab.

If you've previously uploaded resourceprojects data, delete the old data - go to Linked Data -> Graphs -> Graphs and hit the delete button on the resourceprojects.org row

To upload data go to Linked Data -> Quad Store Upload ( http://localhost:8890/conductor/rdf_import.vspx )

* Click the resource URL radio button.
* Put this in the resource URL text box: https://raw.githubusercontent.com/NRGI/resource-projects-etl/master/resource-projects.ttl
* Check the "Create graph explicitly" checkbox
* Put http://resourceprojects.org/ in the Named Graph IRI
* Hit the upload button

(If you've generated the data locally, you can use the "File" option instead).


License
-------

This license does not apply to the NRGI logo that is also included in this repository. Some included dependencies also have their own licenses, which are specified within those dependencies.

```
Copyright (c) 2015 Natural Resource Governance Institute

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.
```
