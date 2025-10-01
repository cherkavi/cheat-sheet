# Google cloud snipset

## For using any service:
1. create project
2. create budget
3. assign budget to project
4. assign Service API to project
5. enable Service API for project
6. check Credentials->API restriction

## Google Maps 
* [documentation rest api swagger](https://places.googleapis.com/$discovery/rest?version=v1)
* [documentation rest api](https://developers.google.com/maps/documentation/places/web-service/reference/rest)
* [documentation place details](https://developers.google.com/maps/documentation/places/web-service/place-details)  
* 
```sh
# documentation rest api
# "id": "GoogleMapsPlacesV1Place"
curl -X POST -H 'Content-Type: application/json' -H "X-Goog-Api-Key: $GOOGLE_API_TOKEN" \
-H 'X-Goog-FieldMask: places.id,places.name,places.displayName,places.formattedAddress,places.reviews' \
-d '{
  "textQuery" : "Bismarckstraße 23, 87700 Memmingen, Germany"
}' 'https://places.googleapis.com/v1/places:searchText'
```
```sh
PLACE_ID=ChIJqULAg_Tym0cRfg03ZWPHoAg 
curl -X GET -H 'Content-Type: application/json' \
-H 'X-Goog-FieldMask: id,name,displayName,formattedAddress,reviews' \
-H "X-Goog-Api-Key: $GOOGLE_API_TOKEN" https://places.googleapis.com/v1/places/$PLACE_ID
```
