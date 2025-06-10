# JIRA

## JIRA REST API applications
* [all my open issues](https://github.com/cherkavi/python-utilities/blob/master/jira/jira-open-issues.py)
* [read comments with my name](https://github.com/cherkavi/python-utilities/blob/master/jira/jira-comments-with-my-name.py)
### Jira Query Language, JQL
[jql search](https://www.atlassian.com/blog/jira-software/jql-the-most-flexible-way-to-search-jira-14)
```
reporter = currentUser() and status = open ORDER BY createdDate DESC
reporter = currentUser() and status in ('open', 'new') ORDER BY createdDate DESC
reporter = currentUser() and status = 'in progress' ORDER BY createdDate DESC
text ~ "is_abstract" and project = "Brand Configuration Management"
```
## JIRA REST API
[REST API](https://docs.getxray.app/display/XRAY/Import+Execution+Results+-+REST#ImportExecutionResultsREST-JUnitXMLresults)
[how to create token](https://www.resolution.de/post/how-to-create-api-tokens-for-jira-server-s-rest-api/)

### jira REST API requests
```sh
curl -X POST -H "Content-Type: multipart/form-data" -u ${JIRA_USER}:${JIRA_PASSWORD} -F "file=@cypress/results/testresult.xml" "https://atc.ubsgroup.net/jira/rest/raven/1.0/import/execution/junit?projectKey=EXTRACT&testPlanKey=EXTRACT-219&testEnvironments=${CYPRESS_BASEURL}"

curl -H "Authorization: Bearer ${JIRA_TOKEN}" -X GET ${JIRA_URL}/rest/api/2/myself
curl -H "Authorization: Bearer ${JIRA_TOKEN}" -X GET ${JIRA_URL}/rest/agile/1.0/board/66453 | jq .

curl -H "Authorization: Bearer $JIRA_TOKEN" $JIRA_URL/rest/api/latest/issue/SSBBCC-2050?expand=renderedFields | jq .
curl -H "Authorization: Bearer $JIRA_TOKEN" $JIRA_URL/rest/api/latest/issue/SSBBCC-2050?fields=status | jq .fields.status.name
curl -H "Authorization: Bearer $JIRA_TOKEN" $JIRA_URL/rest/api/latest/issue/SSBBCC-2050?fields=summary
```

### jira REST API requests
print all your open issues in format: `jira 171455 # In Progress  # [PROD] Kafka: Workflow - Boardliteratur`
```sh
    curl -H "Authorization: Bearer $JIRA_TOKEN" \
         -H "Content-Type: application/json" \
         --silent \
         --data '{"jql":"assignee = currentUser() AND resolution = Unresolved AND status != Pending"}' \
     "$JIRA_URL/rest/api/2/search" | jq -r '.issues[] | "jira \(.key | split("-")[1]) # \(.fields.status.name)  # \(.fields.summary)"'
```
more complex request with additional fields
```
AND status not in (Open, Pending)
AND issueFunction in aggregateExpression("Storypoints gesamt", "Storypoints.sum()")
ORDER BY cf[10293] DESC, status DESC, priority DESC
```

### create sub-task
```sh
function create-sub-task(){
curl -X POST "$JIRA_URL/rest/api/2/issue" \
  -H "Authorization: Bearer $JIRA_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "fields": {
      "project": { "key": "IOO" },
      "parent": { "key": "'"$PARENT_JIRA"'" },   
      "summary": "'"$SUMMARY"'",
      "issuetype": { "name": "Sub-task" }
    }
  }'
}
export PARENT_JIRA="IOO-308"
export SUMMARY="sub task description"; create-sub-task
```
