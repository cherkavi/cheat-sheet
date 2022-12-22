# Tools
## tasks automatisation & business flows & connections between applications
* https://integromat.com, https://make.com
* 

## authentication
### Amazon Cognito
### okta.com
### auth0.com
### authy (twilio)
* https://authy.com/
* https://authy.com/blog/understanding-2fa-the-authy-app-and-sms/
* https://www.twilio.com/authy
* [python-flask application](https://www.twilio.com/docs/authy/quickstart/two-factor-authentication-python-flask)
* [python client 2FA api](https://github.com/twilio/authy-python/tree/master/authy)
* https://www.twilio.com/docs/authy/api/users
* [Account](https://www.twilio.com/console/authy/getting-started)
* [console](https://www.twilio.com/console)
* [console-application](https://www.twilio.com/console/authy/applications)
* [dashboard](https://dashboard.authy.com/applications/<app_id>)
* [dashboard-twilio](https://www.twilio.com/console/authy/applications/292858)
### Yahoo authentication
* [manual steps](https://developer.yahoo.com/oauth2/guide/openid_connect/getting_started.html)
* https://developer.yahoo.com/oauth2/guide/
* https://developer.yahoo.com/oauth2/guide/flows_authcode/
* https://developer.yahoo.com/oauth2/guide/openid_connect/getting_started.html
* https://developer.yahoo.com/oauth2/guide/openid_connect/troubleshooting.html
* [errors](https://developer.yahoo.com/oauth2/guide/errors/)
* https://auth0.com/docs/connections/social/yahoo
#### [yahoo project](https://developer.yahoo.com/apps/JueQG777/)
!!! important: OpenID Connect Permissions: !!! 
```properties
App ID
${APP_ID}
Client ID (Consumer Key)
${CLIENT_ID}
Client Secret (Consumer Secret)
${CLIENT_SECRET}
```
#### request authentication
```sh
# step 1
https://api.login.yahoo.com/oauth2/request_auth?client_id=${CLIENT_ID}&response_type=code&redirect_uri=https://ec2-52-29-176-00.eu-central-1.compute.amazonaws.com&scope=profile,email&nonce=6b526ab2-c0eb

# step 2 
# RESPONSE "Yahoo code"
# code=${CLIENT_CODE}
```
#### request Token 
```bash
curl -X POST https://api.login.yahoo.com/oauth2/get_token --data "code=${CLIENT_CODE}&grant_type=authorization_code&client_id=${CLIENT_ID}&client_secret=${CLIENT_SECRET}&redirect_uri=https://ec2-52-29-176-00.eu-central-1.compute.amazonaws.com&response_type=code"
```
#### [refresh_token usage](https://developer.yahoo.com/oauth2/guide/openid_connect/decode_id_token.html#decode-id-token)
#### [Get User Info API](https://developer.yahoo.com/oauth2/guide/get-user-inf/Get-User-Info-API.html)
```bash
curl --verbose -X POST --data "access_token=${ACCESS_TOKEN}" https://api.login.yahoo.com/openid/v1/userinfo
```
#### encode string for header request
```python
import base64
encodedBytes = base64.b64encode(f"{client_id}:{client_secret}".encode("utf-8"))
encodedStr = str(encodedBytes, "utf-8")
```

## captcha
### google captcha
[reCaptcha client code](https://developers.google.com/recaptcha/docs/v3)
[reCaptcha server code](https://developers.google.com/recaptcha/docs/verify)
[my own project with re-captcha](https://console.cloud.google.com/security/recaptcha/sign-up?project=test-web-project-280419)

#### index file 
```html
<html>
        <head>
                <script src="https://www.google.com/recaptcha/api.js?render=6LcJIwYaAAAAAIpJLnWRF44kG22udQV"></script>
          
<script>
      function onClick(e) {
        //e.preventDefault();     
              console.log("start captcha");
        grecaptcha.ready(function() {
          grecaptcha.execute('6LcJIwYaAAAAAIpJLnWRF44kG22udQV', {action: 'submit'}).then(function(token) {
              console.log(token);                                     
          });
        });
      }
  </script>

        </head>
        <body>
                <button title="captcha" onclick="onClick()" >captcha </button>
        </body>
</html>
```
#### start apache locally in that file 
```sh
docker run --rm --name apache -v $(pwd):/app -p 9090:8080 bitnami/apache:latest
x-www-browser http://localhost:9090/index.html
```

#### example of checking response
```sh
# google key
GOOGLE_SITE_KEY="6Ldo3dsZAAAAAIV6i6..."
GOOGLE_SECRET="6Ldo3dsZAAAAACHkEM..."

CAPTCHA="03AGdBq26Fl_hBnLn7lNf5s53xTRN23yt1OeS4Y7vV6ARSEehMuE_0uKL..."
echo $CAPTCHA
curl -X POST -F "secret=$GOOGLE_SECRET" -F "response=$CAPTCHA" https://www.google.com/recaptcha/api/siteverify
```


## address
### [country codes](https://countrycode.org/)  
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
### https://mailchimp.com/
### [sendgrid](https://sendgrid.com/docs/api-reference/)
* [python example](https://github.com/sendgrid/sendgrid-python/blob/main/examples/helpers/mail_example.py)  
* [additional fields for sending email](https://sendgrid.com/docs/for-developers/sending-email/single-sends-2020-update/#single-sends-api-fields)  
* [template variables](https://sendgrid.com/docs/ui/sending-email/how-to-send-an-email-with-dynamic-transactional-templates/)  
#### sendgrid api 
```sh
# get all templates
curl -X "GET" "https://api.sendgrid.com/v3/templates" \
-H "Authorization: Bearer $API_KEY" \
-H "Content-Type: application/json"

# get template
curl --request GET \
--url https://api.sendgrid.com/v3/templates/d-5015e600bedd47b49e09d7e5091bf513 \
--header "authorization: Bearer $API_KEY" \
--header 'content-type: application/json'
```

```sh
# send 
curl --request POST \
--url https://api.sendgrid.com/v3/mail/send \
--header "authorization: Bearer $API_KEY" \
--header 'content-type: application/json' \
--data @email-data.json
```
email-data.json
```json
{
        "personalizations":[
                {
                        "to":[
                                {"email":"vitalii.cherkashyn@gmail.com","name":"Vitalii"}
                        ],
                        "subject":"test sendgrid"
                }
        ],
        "content": [{"type": "text/plain", "value": "Heya!"}],
        "from":{"email":"vitalii.cherkashyn@gmail.com","name":"Vitalii"},
        "reply_to":{"email":"vitalii.cherkashyn@gmail.com","name":"Vitalii"}
}
```


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

## social networks
### linkedin 
* [all my applications](https://www.linkedin.com/developers/apps)
* [create new application](https://www.linkedin.com/developer/apps/new)
* [your user permissions](https://www.linkedin.com/psettings/permitted-services)
* [appl by id](https://www.linkedin.com/developers/apps/${APP_ID}/auth)
#### linkedin api
##### login with linkedin
```html
<html><body>
<a href="https://www.linkedin.com/oauth/v2/authorization?response_type=code&client_id=78j2mw9cg7da1x&redirect_uri=http%3A%2F%2Fec2-52-29-176-43.eu-central-1.compute.amazonaws.com&state=my_unique_value_generated_for_current_user&scope=r_liteprofile%20r_emailaddress"> login with LinkedIn </a>
</body></html>
```
example of response from LinkedIn API:
```sh
http://ec2-52.eu-central-1.compute.amazonaws.com/?code=<linkedin code>&state=<my_unique_value_generated_for_current_user>
```
##### linkedin profile api
```sh
curl -X GET -H "Authorization: Bearer $TOKEN" https://api.linkedin.com/v2/me
```
answer example:  
```json
{"localizedLastName":"Cherkashyn","profilePicture":{"displayImage":"urn:li:digitalmediaAsset:C5103AQ..."},"firstName":{"localized":{"en_US":"Vitalii"},"preferredLocale":{"country":"US","language":"en"}},"lastName":{"localized":{"en_US":"Cherkashyn"},"preferredLocale":{"country":"US","language":"en"}},"id":"9yP....","localizedFirstName":"Vitalii"}
```
##### collaboration with Contact API
application permissions: r_emailaddress  
[documentation](https://developer.linkedin.com/docs/v1/people/email-lookup-api)  
[documentation](https://docs.microsoft.com/en-us/linkedin/shared/integrations/people/primary-contact-api)  

