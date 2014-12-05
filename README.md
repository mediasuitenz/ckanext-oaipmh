# CKAN Harvester for OAI-PMH

## Instructions

### Installation

Use `pip` to install this plugin. This example installs it in `/vagrant`

```bash
source /home/www-data/pyenv/bin/activate
pip install -e git+https://github.com/openresearchdata/ckanext-oaipmh.git#egg=ckanext-oaipmh --src /vagrant
cd /vagrant/ckanext-oaipmh
pip install -r requirements.txt
python setup.py develop
```

Make sure the ckanext-harvest extension is installed as well.

### Setup the Harvester

- add `oaipmh_harvester` to `ckan.plugins` in `development.ini` (or `production.ini`)
- restart your webserver
- with the web browser go to `<your ckan url>/harvest/new`
- as URL fill in the base URL of an OAI-PMH conforming repository, e.g. http://boris.unibe.ch/cgi/oai2
for more see http://www.openarchives.org/Register/BrowseSites
- select **Source type** `OAI-PMH`
- if your OAI-PMH needs credentials, add the following to the "Configuration" section: `{'username': 'foo', 'password': 'bar' } `
- Save
- on the harvest admin click **Reharvest**

### Run the Harvester

On the command line do this:

- activate the python environment
- `cd` to the ckan directory, e.g. `/usr/lib/ckan/default/src/ckan`
- start the consumers:

```
paster --plugin=ckanext-oaipmh harvester gather_consumer &
paster --plugin=ckanext-oaipmh harvester fetch_consumer &
```

- run the job:

    `paster --plugin=ckanext-oaipmh harvester run`

The harvester should now start and import the OAI-PMH metadata.

## Developing without running jobs manually

To make it easier to develop, tests are setup that allow to do that:

    . ~/default/bin/activate
    cd /vagrant/ckanext-oaipmh

    nosetests --logging-filter=ckanext.oaipmh.harvester --ckan --with-pylons=test.ini ckanext/oaipmh/tests

In this example the logging filter is used to only show messages of the harvester.
