# Tools
## tasks automation & business flows & connections between applications
* https://integromat.com
* https://make.com

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
### Google authentication
#### links
* [playground](https://developers.google.com/oauthplayground/)
* [identity signin](https://developers.google.com/identity/sign-in/web)
* [backend server](https://developers.google.com/identity/sign-in/web/backend-auth)
* [verify ID token](https://developers.google.com/identity/sign-in/web/backend-auth#verify-the-integrity-of-the-id-token)
* https://developers.google.com/identity/protocols/oauth2/openid-connect#python
* https://developers.google.com/identity/choose-auth
* https://developers.google.com/identity/sign-in/web
* https://developers.google.com/identity/sign-in/web/backend-auth
* https://developers.google.com/identity/protocols/oauth2
* https://developers.google.com/identity/protocols/oauth2/openid-connect
* [Tutorial: Set redirect URI](https://developers.google.com/identity/protocols/oauth2/openid-connect#setredirecturi)
* https://support.google.com/a/answer/6149686?hl=en&ref_topic=4487770

#### google project
* [create new web project](https://cloud.google.com/console/project)
* [project console](https://console.cloud.google.com/cloud-resource-manager)
  1. Go to the Credentials page.
  2. Click Create credentials > OAuth client ID.
  3. Select the Web application application type.
  4. Name your OAuth 2.0 client and click Create
* [credentials](https://console.developers.google.com/apis/credentials) 
* [project settings](https://console.cloud.google.com/iam-admin/settings?project=test-web-project-280419)
* [dashboard of project](https://console.cloud.google.com/home/dashboard?project=test-web-project-280419)
* [dashboard of project:: API and services](https://console.cloud.google.com/apis/dashboard?project=test-web-project-280419&show=all)
* [OAuth consent screen ](https://console.developers.google.com/apis/credentials/consent?project=test-web-project-280419)
  > Any User in google Account
* [OAuth credentials (set up project for accessing to user's accounts)](https://console.developers.google.com/apis/credentials?project=test-web-project-280419)
  > select OAuth Client IDs

#### [Tutorial "how to add button"](https://developers.google.com/identity/sign-in/web/sign-in)
> it is Working only from remote ( AWS EC2 ) host
```html
<html lang="en">
  <head>
    <meta name="google-signin-scope" content="profile email">
    <meta name="google-signin-client_id" content="273067202806-6cc49luinddclo4t6.apps.googleusercontent.com">
    <script src="https://apis.google.com/js/platform.js" async defer></script>
  </head>
  <body>
    <div class="g-signin2" data-onsuccess="onSignIn" data-theme="dark">google button</div>
    <script>
      function onSignIn(googleUser) {
        // Useful data for your client-side scripts:
        var profile = googleUser.getBasicProfile();
        console.log("ID: " + profile.getId()); // Don't send this directly to your server!
        console.log('Full Name: ' + profile.getName());
        console.log('Given Name: ' + profile.getGivenName());
        console.log('Family Name: ' + profile.getFamilyName());
        console.log("Image URL: " + profile.getImageUrl());
        console.log("Email: " + profile.getEmail());

        // The ID token you need to pass to your backend:
        var id_token = googleUser.getAuthResponse().id_token;
        console.log("ID Token for backend: " + id_token);
      }
    </script>


    <a href="#" onclick="signOut();">Sign out</a>
    <script>
      function signOut() {
        var auth2 = gapi.auth2.getAuthInstance();
        auth2.signOut().then(function () {
          console.log('User signed out.');
        });
      }
    </script>
    
  </body>
</html>
```

## google rest api services, collaboration with google rest api service 
[example of using via rest client postman](https://blog.postman.com/how-to-access-google-apis-using-oauth-in-postman/)
### Links 
* [project main dashboard](https://console.cloud.google.com/home/dashboard)
* [credentials ](https://console.cloud.google.com/apis/credentials)
* [how to activate google drive access](https://console.cloud.google.com/apis/api/drive.googleapis.com)
  > check "disable api" active button in the second line of the screen - your Drive API is active
* [check google api access](https://console.cloud.google.com/apis/library/drive.googleapis.com)
* [how to use  google drive api](https://developers.google.com/drive/api/reference/rest/v3)
  * [list files](https://developers.google.com/drive/api/reference/rest/v3/files/list)
* [OAuth authentication for curl request](https://developers.google.com/identity/protocols/oauth2/javascript-implicit-flow)

```sh
GDRIVE_URL=https://www.googleapis.com

## obtain token for REST API collaboration 
# register REDIRECT_URL via "OAuth 2.0 Client IDs" "Authorised redirect URIs": https://console.cloud.google.com/apis/credentials
REDIRECT_URL=https://google.com
CLIENT_ID=5344876.....-scmq9ph1tbrvva7p353......apps.googleusercontent.com
SCOPE=https://www.googleapis.com/auth/drive.metadata.readonly
# open in browser
x-www-browser https://accounts.google.com/o/oauth2/v2/auth?scope=${SCOPE}&include_granted_scopes=true&response_type=token&state=state_parameter_passthrough_value&redirect_uri=${REDIRECT_URL}&client_id=${CLIENT_ID}
# copy from redirect url "access_token" field
# curl -X GET -v  https://accounts.google.com/o/oauth2/token

TOKEN="ya29.a0AfB_byBl7oToNlM..."
curl -H "Authorization: Bearer $TOKEN" ${GDRIVE_URL}/drive/v3/about
curl -H "Authorization: Bearer $TOKEN" ${GDRIVE_URL}/drive/v3/files
```
token update
```sh
json_body='{"grant_type":"authorization_code","code":"****my_token","client_id":"******.googleusercontent.com","client_secret":"*******client_secret","redirect_uri":"http://localhost:3000"}'
curl -X POST https://oauth2.googleapis.com/token --data ${json_body}
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
### Google Maps
#### google maps api links
[Google maps platform, document root](https://developers.google.com/places/web-service/search)
[Request API Key](https://developers.google.com/my-business/content/prereqs###request-access)
[How to use api keys](https://cloud.google.com/docs/authentication/api-keys)
[Example of showing credentials](https://console.developers.google.com/apis/credentials)
[cloud billing console](https://console.cloud.google.com/billing)

#### project links
[my google project dashboard](https://console.cloud.google.com/google/maps-apis/overview)
[create new web project](https://cloud.google.com/console/project)
[create new web project](https://console.cloud.google.com/cloud-resource-manager)
[project settings](https://console.cloud.google.com/iam-admin/settings)
[dashboard of project, API and services](https://console.cloud.google.com/home/dashboard)
[OAuth consent screen:-> Any User in google Account](https://console.developers.google.com/apis/credentials/consent)
[OAuth credentials:-> select OAuth Client IDs->ClientId](https://console.developers.google.com/apis/credentials)

#### Google maps REST API
[place search](https://developers.google.com/places/web-service/search)
[place details](https://developers.google.com/places/web-service/details)
[Review, for specific location with additional authentication by Google](https://developers.google.com/my-business/content/review-data###list_all_reviews)
```sh
# activate api key
 YOUR_API_KEY="AIzaSyDTE..."
echo $YOUR_API_KEY

# attempt to find place by name 
# x-www-browser https://developers.google.com/places/web-service/search
SEARCH_STRING="Fallahi%20Zaher%20Attorney"
curl -X GET "https://maps.googleapis.com/maps/api/place/findplacefromtext/json?input=${SEARCH_STRING}&inputtype=textquery&fields=place_id,photos,formatted_address,name,rating,geometry&key=${YOUR_API_KEY}"
# place_id="ChIJl73rFVTf3IARQFQg3ZSOaKo"

# detail about place (using place_id) including user's reviews
# x-www-browser https://developers.google.com/places/web-service/details
curl -X GET "https://maps.googleapis.com/maps/api/place/details/json?place_id=${place_id}&fields=name,rating,review,formatted_phone_number&key=$YOUR_API_KEY"
# unique field here - time
```

#### more that 5 review API:
* [google issue request](https://issuetracker.google.com/issues/35825957)
* [Outscraper 3rd API](https://outscraper.com/)
* [Outscraper 3rd API git](https://github.com/outscraper/google-services-api-pyhton/blob/master/outscraper/api_client.py)


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

## image upload
### imgbb
```sh
# https://api.imgbb.com/
API_KEY_IMGBB=9f......
IMAGE_FILE=handdrawing-01.jpg
IMAGE_NAME="${IMAGE_FILE%.*}"
echo $IMAGE_FILE"  "$IMAGE_NAME

curl --location --request POST "https://api.imgbb.com/1/upload?&key=$API_KEY_IMGBB&name=${IMAGE_NAME}" \
 -F "image=@${IMAGE_FILE}" -H "accept: application/json" | jq . 
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

### facebook
#### links
* [Main manual](https://developers.facebook.com/docs/facebook-login/manually-build-a-login-flow/)
* [facebook SDK](https://facebook-sdk.readthedocs.io/en/latest/install.html)
* [business-sdk python](https://github.com/facebook/facebook-python-business-sdk) 
* Developer guide
  * [business-sdk](https://developers.facebook.com/docs/business-sdk)
  * [graph API Explorer](https://developers.facebook.com/tools/explorer/)
  * [facebook login web](https://developers.facebook.com/docs/facebook-login/web)
  * [scope, permissions](https://developers.facebook.com/docs/facebook-login/permissions/)
  * [create development account](https://developers.facebook.com/apps/)
  * [all possible products](https://developers.facebook.com/apps/${APP_ID}/add/)
  * [activate FB login](https://developers.facebook.com/apps/${APP_ID}/fb-login/quickstart/)
  * [settings of FB login](https://developers.facebook.com/apps/${APP_ID}/fb-login/settings/)
  * [app registration](https://developers.facebook.com/docs/apps#register)
  * [Security](https://developers.facebook.com/docs/facebook-login/security/)
  * [recommendations](https://developers.facebook.com/docs/graph-api/reference/recommendation/)
  * [ratings](https://developers.facebook.com/docs/graph-api/reference/page/ratings)
  * [guide](https://developers.facebook.com/docs/pages/guides)
  * [realtime](https://developers.facebook.com/docs/pages/realtime)
  * [development rules](https://developers.facebook.com/docs/graph-api/overview)
  * [permission and features](https://developers.facebook.com/apps/${APP_ID}/app-review/permissions/)
  * [test app add product](https://developers.facebook.com/apps/$APP_ID/dashboard/#addProduct)
* [pulling review](https://stackoverflow.com/questions/23380533/facebook-api-for-pulling-reviews)
* [create test app  ( upper left corner )](https://www.facebook.com/pg/$PROFILE_NAME-$PROFILE_ID/reviews/?ref=page_internal)

#### get profile_id from facebook page
```sh
curl -X GET https://www.facebook.com/$PROFILE_NAME | grep profile_owner
```
#### get access token
```sh
# https://www.facebook.com/v7.0/dialog/oauth?client_id=${APP_ID}&redirect_uri=${REDIRECT_URL}&state=state123abc
# REDIRECT_URI !!! DON'T REMOVE trailing slash !!! 
curl -X GET https://graph.facebook.com/v7.0/oauth/access_token?client_id=${APP_ID}&redirect_uri=${REDIRECT_URI}&client_secret=${APP_SECRET}&code=${CODE_PARAMETER}
# b'{"access_token":"$ACCESS_TOKEN","token_type":"bearer","expires_in":5181746}'
```
#### get profile info
```sh
curl -X GET https://graph.facebook.com/v7.0/me?fields=id,last_name,name&access_token=${ACCESS_TOKEN}"
```
#### get ratings
```sh
# $PROFILE_ID/ratings?fields=reviewer,review_text,rating,has_review
curl -i -X GET \
 "https://graph.facebook.com/v8.0/$PROFILE_ID/ratings?fields=reviewer%2Creview_text%2Crating%2Chas_review&access_token=$ACCESS_TOKEN"

# https://developers.facebook.com/tools/explorer/?method=GET&path=$PROFILE_ID%2Fratings%3Ffields%3Drating%2Creview_text%2Creviewer&version=v8.0
# https://developers.facebook.com/tools/explorer/${APP_ID}/?method=GET&path=${PROFILE_NAME}%3Ffields%3Dreview_text&version=v8.0
```
possible fields:
* name
* created_time
* rating
* review_text
* recommendation_type
* reviewer
* has_review
```json
 {
  "data": [
    {
      "created_time": "2020-08-17T20:40:26+0000",
      "recommendation_type": "positive",
      "review_text": "let's have a fun! it is a great company for that"
    }
  ]
}
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

## user guide user tutorials
* https://shepherdjs.dev/
* https://introjs.com/
* https://www.npmjs.com/package/guidechimp

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

## e-commerce
### shopify
```sh
x-www-browser https://$SHOPIFY_SHOP_NAME.myshopify.com &
x-www-browser https://$SHOPIFY_SHOP_NAME.myshopify.com/admin &	
# shopify product count
curl --location --header "X-Shopify-Access-Token: ${SHOPIFY_AUTH_TOKEN}" -X GET "https://${SHOPIFY_SHOP_NAME}.myshopify.com/admin/api/2021-04/products/count.json" 
# shopify product by id
curl --location -X GET --header "X-Shopify-Access-Token: ${SHOPIFY_AUTH_TOKEN}" "https://${SHOPIFY_SHOP_NAME}.myshopify.com/admin/api/2021-04/products/${1}.json" | jq .
# shopify get all products
curl --location --header "X-Shopify-Access-Token: ${SHOPIFY_AUTH_TOKEN}"  -X GET "https://${SHOPIFY_SHOP_NAME}.myshopify.com/admin/api/2021-04/products.json?fields=id" | jq .	
# get variants count
curl --location -X GET "https://${SHOPIFY_API_KEY}:${SHOPIFY_API_PASSWORD}@${SHOPIFY_SHOP_NAME}.myshopify.com/admin/api/2021-04/products/${PRODUCT_ID}/variants/count.json" | jq .
# shopify product variant by id
curl --location -X GET --header "X-Shopify-Access-Token: ${SHOPIFY_AUTH_TOKEN}" "https://${SHOPIFY_SHOP_NAME}.myshopify.com/admin/api/2021-07/variants/${1}.json" | jq . 
# get metadata for product by id
curl --location -X GET "https://${SHOPIFY_API_KEY}:${SHOPIFY_API_PASSWORD}@${SHOPIFY_SHOP_NAME}.myshopify.com/admin/api/2021-04/products/${PRODUCT_ID}/metafields.json" | jq .

# shopify create product
curl -v -X POST -H "X-Shopify-Access-Token:${SHOPIFY_AUTH_TOKEN}" --data "{'product': {'title': 'test-product-remove-me', 'product_type': 'Framed Prints', 'published': True, 'vendor': 'Classy Art', 'tags': 'Furniture,Accessories,Wall Art,Category:Accessories,Category_Accessories,Category:Wall Art,Category_Wall Art,Color:Brown,Color_Brown,Product Type:Framed Prints,Product Type_Framed Prints,Brand:Classy Art,Brand_Classy Art', 'taxable': True, 'options': None, 'variants': [{'option1': None, 'option2': None, 'option3': None, 'price': 139.99, 'compare_at_price': None, 'sku': '1055', 'weight_unit': 'lb', 'weight': 0, 'inventory_management': 'shopify', 'inventory_policy': 'continue'}], 'status': 'draft'}}"  https://${SHOPIFY_SHOP_NAME}.myshopify.com/admin/api/2021-04/products.json
# shopify update product
curl --header "X-Shopify-Access-Token: ${SHOPIFY_AUTH_TOKEN}" --request PUT "https://${SHOPIFY_SHOP_NAME}.myshopify.com/admin/api/2021-04/products/${PRODUCT_ID}.json" \
--header 'Content-Type: application/json' \
--data-binary '@product.json'
# delete product by id
curl --location --header "X-Shopify-Access-Token: ${SHOPIFY_AUTH_TOKEN}"  -X DELETE "https://${SHOPIFY_SHOP_NAME}.myshopify.com/admin/api/2021-04/products/$1.json"
# remove variant, delete variant in "standard product" (has only one variant )  leads to remove product
curl --location -w "response-code: %{http_code}\n" -X DELETE "https://${SHOPIFY_API_KEY}:${SHOPIFY_API_PASSWORD}@${SHOPIFY_SHOP_NAME}.myshopify.com/admin/api/2021-04/products/${PRODUCT_ID}/variants/${VARIANT_ID}.json"

# add metadata to image 
curl --location -X PUT "https://${SHOPIFY_API_KEY}:${SHOPIFY_API_PASSWORD}@${SHOPIFY_SHOP_NAME}.myshopify.com/admin/api/2021-04/products/${PRODUCT_ID}/images/${IMAGE_ID}.json"  \
--header 'Content-Type: application/json' \
--data-raw '{"image": {"id": 28125015310400,"metafields": [{"key": "test2","value": "test3","value_type": "string","namespace": "tags", "imageid":28125015310400}]}}'
# get image count by product
curl --location -X GET "https://${SHOPIFY_API_KEY}:${SHOPIFY_API_PASSWORD}@${SHOPIFY_SHOP_NAME}.myshopify.com/admin/api/2021-04/products/${PRODUCT_ID}/images/count.json" | jq .
# get images by product
curl --location -X GET "https://${SHOPIFY_API_KEY}:${SHOPIFY_API_PASSWORD}@${SHOPIFY_SHOP_NAME}.myshopify.com/admin/api/2021-04/products/${PRODUCT_ID}/images.json" | jq .
# get image metadata from !!! global account storage !!!
curl --location -X GET https://${SHOPIFY_API_KEY}:${SHOPIFY_API_PASSWORD}@${SHOPIFY_SHOP_NAME}.myshopify.com/admin/api/2021-04/metafields.json?metafield[imageid]=${IMAGE_ID}

# get all collections
curl --location -X GET "https://${SHOPIFY_API_KEY}:${SHOPIFY_API_PASSWORD}@${SHOPIFY_SHOP_NAME}.myshopify.com/admin/api/2021-04/smart_collections.json" | jq .
# get collection by id 
COLLECTION_ID=270289993909
curl --location -X GET "https://${SHOPIFY_API_KEY}:${SHOPIFY_API_PASSWORD}@${SHOPIFY_SHOP_NAME}.myshopify.com/admin/api/2021-04/smart_collections/{$COLLECTION_ID}.json" | jq .
# delete collection by id 
curl --location -X DELETE "https://${SHOPIFY_API_KEY}:${SHOPIFY_API_PASSWORD}@${SHOPIFY_SHOP_NAME}.myshopify.com/admin/api/2021-04/smart_collections/{$COLLECTION_ID}.json" | jq .

# get policies
curl --location --request GET "https://${SHOPIFY_API_KEY}:${SHOPIFY_API_PASSWORD}@${SHOPIFY_SHOP_NAME}.myshopify.com/admin/api/2021-04/policies.json"
# get policies with token
curl --location --header "X-Shopify-Access-Token: ${SHOPIFY_AUTH_TOKEN}" --request GET "https://${SHOPIFY_SHOP_NAME}.myshopify.com/admin/api/2021-04/policies.json"
# get access scopes
curl --location --request GET "https://${SHOPIFY_API_KEY}:${SHOPIFY_API_PASSWORD}@${SHOPIFY_SHOP_NAME}.myshopify.com/admin/oauth/access_scopes.json" | jq .
# get shop description 
curl --location --request GET "https://${SHOPIFY_API_KEY}:${SHOPIFY_API_PASSWORD}@${SHOPIFY_SHOP_NAME}.myshopify.com/admin/api/2021-04/shop.json" --silent | jq .

```
