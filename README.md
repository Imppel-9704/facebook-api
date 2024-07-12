[![Python 3.6](https://img.shields.io/badge/python-3.12-blue.svg)](https://www.python.org/downloads/release/python-312/)

# Facebook Ads Insights API
Example code of Facebook Ads Insights API for Python to get campaign performance from Facebook Ads manager.

## Before Start
You need to create your own application on `https://developers.facebook.com/`.
After created application then go to your app and take a look at left panel `Add Product`. Choose `Marketing API`.
Then go to `Tools` and select Token Permissions (Choose all of it) and click `Get Token`.

## Installing
```
pip install facebook_business
pip install pandas
```

## Usage
After installing necessary libaries. Let's import it.
```
import time
import pandas as pd
from facebook_business.api import FacebookAdsApi
from facebook_business.adobjects.adaccount import AdAccount
from facebook_business.adobjects.business import Business
from facebook_business.adobjects.adsinsights import AdsInsights
from facebook_business.adobjects.adreportrun import AdReportRun
```

Fill your business id
```
business_id = Business('your_business_id')
```

If you created application already. Get your App ID from the top of the page. My App Secret from App Setting and Access Token from the previous step.
```
my_app_id = 'your_app_id'
my_app_secret = 'your_app_secret'
my_access_token = 'your_access_token'
FacebookAdsApi.init(my_app_id, my_app_secret, my_access_token)
```

This `get_owned_ad_accounts()` will return all your managed accounts
```
accounts = business_id.get_owned_ad_accounts()
```

Create variables `fields` with the fields you want to query and parameters as `params` to choose date preset and level of your data.
```
fields = [
        AdsInsights.Field.account_id,
        AdsInsights.Field.account_name,
        AdsInsights.Field.campaign_id,
        AdsInsights.Field.campaign_name,
        AdsInsights.Field.objective,
        AdsInsights.Field.creative_media_type,
        AdsInsights.Field.impressions,
        AdsInsights.Field.spend,
        AdsInsights.Field.inline_link_clicks
        ]

params = {
        'date_preset': 'today',
        'level': 'campaign'
        }
```

Then you can use `for loop` if you have many accounts to get your data. It will return as json.
```
for account in accounts:
  acc = AdAccount(account['id'])
  result = acc.get_insights(fields=fields, params=params)
```

In case you want to export as csv file. You can handle data with `Pandas`
```
df = pd.json_normalize(result)
df.to_csv('facebook_data.csv', index=False, encoding='utf-8-sig')
```
