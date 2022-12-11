# Tools
## tasks automatisation & business flows & connections between applications
* https://integromat.com, https://make.com
* 

## authentication
* Amazon Cognito
* okta.com
* auth0.com

## address
### US Zip codes
```sh
# Plano TX
curl https://public.opendatasoft.com/explore/dataset/us-zip-code-latitude-and-longitude/table/?q=plano

# coordinates
curl -X GET https://public.opendatasoft.com/api/records/1.0/search/?dataset=us-zip-code-latitude-and-longitude&q=Plano&facet=state&facet=timezone&facet=dst
```

## sms broadcasting
* www.twilio.com  
  [doc, examples](https://pypi.org/project/twilio/)  
  [my examples](python-utilities/twilio/twilio.md)  

##  e-mail broadcasting
* https://mailchimp.com/

## redirect
* Easyredir - https://www.easyredir.com/
* Redirection.io - https://redirection.io/
* SiteDetour - https://sitedetour.com/  

## screenshots
* site-shot.com
```sh
USER_KEY=YAAIEYK....
curl -L -X POST -H "Content-Type: application/x-www-form-urlencoded" -H "Accept: text/plain"  -H "userkey: $USER_KEY" -F "DEBUG=True" -F "url=google.com" https://api.site-shot.com 

curl -L -X POST -H "Accept: text/plain"  -H "userkey: $USER_KEY" -F "DEBUG=True" -F "url=google.com" https://site-shot.com?DEBUG=True

curl -X POST -H "userkey:$USER_KEY" https://api.site-shot.com/?url=google.com

curl -X POST -H "userkey:$USER_KEY" -F "url=http://www.emsylaw.com" -F "format=jpg" -o emsylaw.jpg https://api.site-shot.com 
```

## image compression
### shortpixel.com
* [https://shortpixel.com/api-tools](api tools, documenation)
* [https://shortpixel.com/api-docs](api docs, documenation)

```sh
url1="https://staging.s3.us-east-1.amazonaws.com/img/dir/chinese/bu-2739.jpeg"
body='{"key": "'$SHORTPIXEL_KEY'", "plugin_version": "dbrbr", "lossy": 2, "resize": 0, "resize_width": 0, "resize_height": 0, "cmyk2rgb": 1, "keep_exif": 0, "convertto": "", "refresh": 0, "urllist": ["'$url1'"], "wait": 35}'

curl -H "Content-Type: application/json" --data-binary $body -X POST https://api.shortpixel.com/v2/reducer.php
```

## feedback collector
### canny
```sh
export API_KEY=...
# boards list
curl https://canny.io/api/v1/boards/list -d apiKey=$API_KEY | jq .
curl https://canny.io/api/v1/boards/list?apiKey=$API_KEY | jq .

# board by id
export BOARD_ID=5f8cba47...
curl https://canny.io/api/v1/boards/retrieve -d apiKey=$API_KEY -d id=BOARD_ID | jq .
```

### yelp
* [API documentation](https://www.yelp.com/developers/documentation/v3/business)  
* [API documentation](https://www.yelp.com/developers/documentation/v3/business_reviews)
* [get started, postman](https://www.yelp.com/developers/documentation/v3/get_started)  
* [postman collection](https://app.getpostman.com/api/collections/6b506a43109229cb2798)  
* [python api, python code](https://github.com/gfairchild/yelpapi)  
* [authentication](https://www.yelp.com/developers/documentation/v3/authentication)  
```sh
yelp_id='law-office-of-spojmie-nasiri-pleasanton-5'

curl --location --request GET 'https://api.yelp.com/v3/businesses/law-office-of-camelia-mahmoudi-san-jose-3' \
--header "Authorization: Bearer $API_KEY"

curl --location --request GET 'https://api.yelp.com/v3/businesses/law-office-of-camelia-mahmoudi-san-jose-3/reviews' \
--header "Authorization: Bearer $API_KEY" | jq .
```

## user activities
* Matomo
  * [documentation](https://matomo.org/docs)
  * [documentation for tags, triggers, variables](https://matomo.org/docs/tag-manager/)
  * [import data from google analytics](https://matomo.org/docs/google-analytics-importer/)
  * [installation](https://matomo.org/docs/installation/)
  * [develop](https://developer.matomo.org/guides/tagmanager/introduction)
  * [develop](https://developer.matomo.org/guides/tracking-api-clients)
  * [develop](https://developer.matomo.org/guides/tagmanager/custom-tag)
  * [php integration](https://github.com/matomo-org/matomo-php-tracker)
  * [php integration](https://github.com/heiglandreas/piwik#readme)

## external data
### [global reestr of data](https://www.data.gov/)
### [Census API](https://www.census.gov/data/developers/updates/new-discovery-tool.html)
* [list of all datasets](https://api.census.gov/data.html)
* `wget api.census.gov/data.xml`
* [dataset by year](https://api.census.gov/data/2010.html)
* [dataset with UI](https://data.census.gov/cedsci/table?q=United%20States&t=535%20-%20German%20%28032-045%29%3APopulations%20and%20People&g=0100000US&tid=ACSDT5YSPT2015.B01001&hidePreview=false)
    ```sh
    wget https://api.census.gov/data/2014/pep/natstprc?get=STNAME,POP&DATE_=7&for=state:*
    wget https://api.census.gov/data/2014/pep/natstprc?get=STNAME,POP&DATE_=*&for=state:*
    wget https://api.census.gov/data/2013/pep/cty?get=STNAME,POP,NIM&for=county:*&in=state:01&DATE_=6
    ```
* user-guide
  * https://github.com/uscensusbureau/citysdk
  * https://www.census.gov/data/developers/guidance/api-user-guide.html
  * https://www.census.gov/content/dam/Census/data/developers/api-user-guide/api-guide.pdf
* [video tutorials](https://www.census.gov/data/academy/data-gems.html)
* For developers
  * https://www.census.gov/developers/
  * https://www.census.gov/data/developers/data-sets.html
  * https://project-open-data.cio.gov/schema/
  * https://project-open-data.cio.gov/metadata-resources/
