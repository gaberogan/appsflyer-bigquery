## Setup

Create a appsflyer-etl folder
Clone https://github.com/TG-17/tap-appsflyer
Clone https://github.com/singer-io/target-stitch
Change tap-appsflyer/setup.py to match target-stitch's singer-python version which is 5.8.0 as of today and singer-python's "backoff" package which is 1.8.0 as of today
Change target-stitch/setup.py cchardet to 2.1.7
Apply all the changes from https://github.com/singer-io/tap-appsflyer/pull/15/files
Apply all the changes from https://github.com/singer-io/tap-appsflyer/pull/17/files
Add organic_in_app_events
python3 -m venv env
source env/bin/activate
Run "brew unlink c-ares" if on mac (prevents https://github.com/saghul/pycares/issues/136)
pip install -e tap-appsflyer
pip install -e target-stitch
Create config.json with:
	client_id = 6 digit stitch data client id, see url
	token = long token for stitch import api
	app_id = AppsFlyer app id (ie com.rotoql.betql)
	api_token = AppsFlyer api token v1
	"small_batch_url": "https://api.stitchdata.com/v2/import/batch",
	"big_batch_url": "https://api.stitchdata.com/v2/import/batch",
	"batch_size_preferences": {}
tap-appsflyer -c config.json | target-stitch --config config.json