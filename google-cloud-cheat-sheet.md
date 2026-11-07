# Google cloud

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

## `gcloud`: The Infrastructure Manager
official CLI for the **Google Cloud Platform (GCP)**. It is the primary tool for creating and managing cloud infrastructure resources: Virtual machines (Compute Engine), databases (Cloud SQL), Kubernetes clusters (GKE), storage buckets, network configurations, and more.

### install gcloud 
```sh
sudo snap install google-cloud-cli --classic
gcloud --version

# autocomplete
source /snap/google-cloud-cli/current/completion.bash.inc

# init 
gcloud init

gcloud cheat-sheet
```

### gcloud authentication
```sh
gcloud auth list
gcloud auth application-default print-access-token
```

### glcoud usage with project
```sh 
GCP_EMAIL=$(gcloud config get-value account)
x-www-browser "https://console.cloud.google.com/home/dashboard?project=$GPROJECT_NAME&authuser=$GCP_EMAIL"

gcloud projects list

GPROJECT_NAME=gtask-manager-3841
# gmail.googleapis.com drive.googleapis.com docs.googleapis.com sheets.googleapis.com chat.googleapis.com people.googleapis.com
gcloud services enable calendar-json.googleapis.com tasks.googleapis.com --project=$GPROJECT_NAME
```

### gcloud extend project visibility 
```sh
gcloud services enable chat.googleapis.com  --project=$GPROJECT_NAME

# see `gws` tool below 
gws auth login --scopes "https://www.googleapis.com/auth/chat.spaces.readonly,https://www.googleapis.com/auth/chat.messages&authuser=$GCP_EMAIL"
```


## `gws`: The Workspace Data Controller
Google Workspace CLI - is a newer, open-source CLI specifically designed to interact with **Google Workspace APIs**.
directly manage Workspace user data ( Gmail, Drive, Calendar, Sheets, Docs, Chat, and Admin into shell commands )

### install gws 
```sh 
npm install -g @googleworkspace/cli

gws --version
```

### gws login
```sh
## insert/create credentials 
gws auth setup   # OAuth client: Client ID, Client Secret 
# Audience -> Test users -> Add users -> < enter your gmail account >

## for existing credentials map the scope 
gws auth login   # should start automatically after the `setup`
                 # "status": "success"
                 # mv ~/.config/gws/token_cache.json ~/.config/gws/token_cache.json.backup

gws auth status
```

### gws login token update, scope update, credentials update
```sh
## create credentials 
GCP_EMAIL=$(gcloud config get-value account)
x-www-browser "https://console.cloud.google.com/apis/credentials?project=$GPROJECT_NAME&authuser=$GCP_EMAIL"
              # OAuth client --> Desktop App : Client ID, Client Secret 
              # compare your downloaded json file with $HOME/.config/gws/client_secret.json
              # mv $(find ~/Downloads/client_secret_*googleusercontent.com.json) $HOME/.config/gws/client_secret.json
              # mv ~/.config/gws/token_cache.json ~/.config/gws/token_cache.json.backup

## for existing credentials map the scope 
gws auth login
              # ⚠️ check scopes !!!! otherwise repeat with deselecting all the elements !!!!
              # gws auth login --scopes "https://www.googleapis.com/auth/calendar,https://www.googleapis.com/auth/tasks,https://www.googleapis.com/auth/chat.spaces.readonly,https://www.googleapis.com/auth/chat.messages"

gws auth status

x-www-browser "https://console.cloud.google.com/auth/clients?project=$GPROJECT_NAME&authuser=$GCP_EMAIL"
```


### gws tasks 
```sh
# List your task lists
gws tasks tasklists list
gws tasks tasklists list --format table

TASKLIST_ID=$(gws tasks tasklists list | jq -r '.items[0].id')
TASKLIST_ID=$(gws tasks tasklists list | jq -r '.items[1].id')
# List tasks from a specific tasklist
gws tasks tasks list --params '{"tasklist":"'$TASKLIST_ID'"}'

# Create a task
gws tasks tasks insert --params '{"tasklist":"'$TASKLIST_ID'"}' --json '{"title":"Buy groceries","notes":"Milk, eggs, bread"}'

# Mark a task complete (PATCH status to "completed")
gws tasks tasks patch --params '{"task":"<TASK_ID>","tasklist":"<TASKLIST_ID>"}' --json '{"status":"completed"}'


# https://gist.github.com/chrisrimmer1/affca755213183e0a9cce40f51da28a8
```

### gws chat
```sh
## list spaces 
gws chat spaces list

## search message in space
SPACE_NAME="spaces/AAQAW"
gws chat spaces messages list --params "{\"parent\": \"$SPACE_NAME\"}"
gws chat spaces messages list --params "{\"parent\": \"$SPACE_NAME\", \"pageSize\": 1000}" --format json
gws chat spaces messages search --params "{\"parent\": \"spaces/-\", \"filter\": \"space.name=$SPACE_NAME AND d946-11f1\"}" --format json
gws chat spaces messages search --params "{\"parent\": \"spaces/-\", \"filter\": \"space.name=$SPACE_NAME and has_link() AND 6490-4854-b259\"}" --format json
```

For making possible to write messages in the chat: 
* go to dashboard of the project
* Enabled APIs & Services
* Google Chat API
* Configuration tab ( metrics, ... Credentials ... )
* Feel application info
  * App name - any 
  * Avatar url - like: https://cdn-icons-png.magnific.com/256/11747/11747124.png
  * Connection strings - HTTP Endpoint URL
    * App command: https://examples.com/chat
    * Addeed to space: https://examples.com/chat
    * Message: https://examples.com/chat
    * Removed from space: https://examples.com/chat
```sh
## write message in space
THREAD_NAME=$SPACE_NAME/threads/f8gaCH
gws chat spaces messages create --params '{"parent": "'$SPACE_NAME'", "messageReplyOption": "REPLY_MESSAGE_FALLBACK_TO_NEW_THREAD"}' --json '{"text": "new price: '$new_price'", "thread": {"name": "'$gchat_thread_id'"}}'
```

### gws discover 
```sh
gws schema gmail.users.messages.list
```