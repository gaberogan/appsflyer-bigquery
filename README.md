## Setup

- python3 -m venv env
- source env/bin/activate
- Run "brew unlink c-ares" if on mac (prevents https://github.com/saghul/pycares/issues/136)
- Run "apt install python3-pip" if on linux
- pip install -e tap-appsflyer
- pip install -e target-bigquery
- Create config.json with:
	- app_id = AppsFlyer app id (ie com.rotoql.betql)
	- api_token = AppsFlyer api token v1
	- "project_id": "your-gcp-project",
  - "dataset_id": "appsflyer",
  - "validate_records": true
- Create service-account.json file for your GCP credentials
- tap-appsflyer -c config.json | target-bigquery -c config.json

## Cron
Run "crontab -e"
Add this line:
`30 23 * * * source /python-binaries; cd /project-folder; GOOGLE_APPLICATION_CREDENTIALS=/service-account.json tap-appsflyer -c config.json | target-bigquery --c config.json`

## Changes
- Clone https://github.com/TG-17/tap-appsflyer fork
- Clone https://github.com/singer-io/target-stitch
- Change tap-appsflyer/setup.py to match singer-python v5.8.0 and match singer-python's "backoff" package, currently 1.8.0
- Apply all the changes from https://github.com/singer-io/tap-appsflyer/pull/15/files
- Apply all the changes from https://github.com/singer-io/tap-appsflyer/pull/17/files
- Add organic_in_app_events
