# Fritzbox 
* [support for developers](https://fritz.com/pages/schnittstellen)
  * [XML SOAP collaboration](https://fritz.support/resources/TR-064_First_Steps.pdf)
* [generating support information for the FritzBox with specific version](https://fritz.com/en/apps/knowledge-base/FRITZ-Box-7590/1426_Generating-support-information-for-the-FRITZ-Box?t_id=7241791)

## Common part
```sh
FRITZ_IP="fritz.box"
FRITZ_PORT="49000"
FRITZ_URL="http://${FRITZ_IP}:${FRITZ_PORT}"

FRITZBOX_USER=''  # !!! set your value !!!
FRITZBOX_PASS=''  # !!! set your value !!!

# get device info
curl -s http://$FRITZ_IP:$FRITZ_PORT/tr64desc.xml
curl -s http://$FRITZ_IP:$FRITZ_PORT/tr64desc.xml | grep HostFilter
curl -s http://$FRITZ_IP:$FRITZ_PORT/x_hostfilterSCPD.xml 
curl -s http://$FRITZ_IP:$FRITZ_PORT/igddesc.xml
```

## Get Security Port
```sh
curl -s "${FRITZ_URL}/upnp/control/deviceinfo" -H 'Content-Type: text/xml; charset="utf-8"' \
-H 'SOAPAction: "urn:dslforum-org:service:DeviceInfo:1#GetSecurityPort"' \
-d '<?xml version="1.0"?>
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/" s:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/">
    <s:Body>
        <u:GetSecurityPort xmlns:u="urn:dslforumorg:service:DeviceInfo:1"></u:GetSecurityPort>
    </s:Body>
</s:Envelope>' | xmllint --format -
```

## Get Host number of Entries
```sh
curl -s -X POST "${FRITZ_URL}/upnp/control/hosts" \
     -H "Content-Type: text/xml; charset="utf-8"" \
     -H "SoapAction: urn:dslforum-org:service:Hosts:1#GetHostNumberOfEntries" \
     -d '<?xml version="1.0"?>
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/" s:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/">
    <s:Body>
        <u:GetHostNumberOfEntries xmlns:u="urn:dslforum-org:service:Hosts:1"></u:GetHostNumberOfEntries>
    </s:Body>
</s:Envelope>' | xmllint --format -
```

## General existing/allowed variables, Request with Authentication 
```sh
curl --user "$FRITZBOX_USER:$FRITZBOX_PASS" --silent --anyauth \
    -X POST "${FRITZ_URL}/upnp/control/wanpppconn1" \
     -H "Content-Type: text/xml; charset="utf-8"" \
     -H "SoapAction: urn:dslforum-org:service:WANPPPConnection:1#GetInfo" \
     -d '<?xml version="1.0" encoding="utf-8"?>
         <s:Envelope s:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/" 
                     xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
           <s:Body>
             <u:GetInfo xmlns:u="urn:dslforum-org:service:WANPPPConnection:1">
             </u:GetInfo>
           </s:Body>
         </s:Envelope>' | xmllint --format -
```         

## list of commands: hostfilter 
```sh
x-www-browser http://fritz.box:49000/x_hostfilterSCPD.xml
```

## command hostfilter: Get profiles 
```sh
RESPONSE=$(curl --user "$FRITZBOX_USER:$FRITZBOX_PASS" --anyauth -s -X POST "${FRITZ_URL}/upnp/control/x_hostfilter" \
-H 'Content-Type: text/xml; charset="utf-8"' \
-H 'SOAPAction: "urn:dslforum-org:service:X_AVM-DE_HostFilter:1#GetFilterProfiles"' \
-d '<?xml version="1.0" encoding="utf-8"?>
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/" s:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/">
    <s:Body>
        <u:GetFilterProfiles xmlns:u="urn:dslforum-org:service:X_AVM-DE_HostFilter:1">
        </u:GetFilterProfiles>
    </s:Body>
</s:Envelope>' | xmllint --format - )

echo "$RESPONSE" | xmllint --xpath "string(//*[local-name()='NewFilterProfileList'])" - | xmllint --format -
```

## command hostfilter: Set profile to device
```sh
FILTER=filtprof2876   # silent mode
FILTER=filtprof2866   # block-after-2200
FILTER=filtprof1      # FILTER_STANDARD
DEVICE_IP_ADDRESS=192.168.178.36

curl --user "$FRITZBOX_USER:$FRITZBOX_PASS" --anyauth -X POST "${FRITZ_URL}/upnp/control/x_hostfilter" \
-H 'Content-Type: text/xml; charset="utf-8"' \
-H 'SOAPAction: "urn:dslforum-org:service:X_AVM-DE_HostFilter:1#AddHostEntryToFilterProfile"' \
-d '<?xml version="1.0" encoding="utf-8"?> 
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/" s:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"> 
    <s:Body> 
        <u:AddHostEntryToFilterProfile xmlns:u="urn:dslforum-org:service:X_AVM-DE_HostFilter:1"> 
            <NewIPv4Address>'$DEVICE_IP_ADDRESS'</NewIPv4Address> 
            <NewFilterProfileID>'$FILTER'</NewFilterProfileID> 
        </u:AddHostEntryToFilterProfile> 
    </s:Body> 
</s:Envelope>'
```

## Request with simple Authentication 
```sh
# 10.7.1 Initial Client Request
curl -k --user "$FRITZBOX_USER:$FRITZBOX_PASS" --anyauth \
     -X POST "${FRITZ_URL}/upnp/control/hosts" \
     -H "Content-Type: text/xml; charset=utf-8" \
     -H "SoapAction: urn:dslforum-org:service:Hosts:1#GetHostNumberOfEntries" \
     -d '<?xml version="1.0" encoding="utf-8"?>
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/" s:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/">
 <s:Header>
    <h:InitChallenge xmlns:h="http://soap-authentication.org/digest/2001/10/" s:mustUnderstand="1">
        <UserID>admin</UserID>
    </h:InitChallenge >
 </s:Header>
 <s:Body>
    <u:GetHostNumberOfEntries xmlns:u="urn:dslforum-org:service:Hosts:1"></u:GetHostNumberOfEntries>
 </s:Body>
</s:Envelope>'
```

===
## 2Steps authentication with obtaining necessary credentials
```sh
# Target Service Details (for GetHostNumberOfEntries)
SERVICE_URL="${FRITZ_URL}/upnp/control/hosts"
SERVICE_TYPE="urn:dslforum-org:service:Hosts:1"
ACTION="GetHostNumberOfEntries"

# --- STEP 1: Get the Nonce and Realm ---
# We send an InitChallenge to provoke the 401 response containing the Nonce
INIT_XML="<?xml version='1.0' encoding='utf-8'?>
<s:Envelope xmlns:s='http://schemas.xmlsoap.org/soap/envelope/' s:encodingStyle='http://schemas.xmlsoap.org/soap/encoding/'>
  <s:Header>
    <h:InitChallenge xmlns:h='http://soap-authentication.org/digest/2001/10/' s:mustUnderstand='1'>
      <UserID>$FRITZBOX_USER</UserID>
    </h:InitChallenge>
  </s:Header>
  <s:Body>
    <u:$ACTION xmlns:u='$SERVICE_TYPE'></u:$ACTION>
  </s:Body>
</s:Envelope>"

curl -s -X POST "$SERVICE_URL" -H "SoapAction: $SERVICE_TYPE#$ACTION" -H "Content-Type: text/xml; charset=utf-8" -d "$INIT_XML"

RESPONSE=$(curl -s -X POST "$SERVICE_URL" -H "SoapAction: $SERVICE_TYPE#$ACTION" -H "Content-Type: text/xml; charset=utf-8" -d "$INIT_XML")
echo $RESPONSE


# Extract Nonce and Realm from the response
NONCE=$(echo "$RESPONSE" | grep -oPm1 "(?<=<Nonce>)[^<]+")
REALM=$(echo "$RESPONSE" | grep -oPm1 "(?<=<Realm>)[^<]+")

if [ -z "$NONCE" ]; then
    echo "Error: Could not retrieve Nonce from Fritz!Box."
    exit 1
fi

# --- STEP 2: Calculate the MD5 Auth Hash ---
# Formula: MD5(MD5(user:realm:password):nonce)
SECRET_H1=$(echo -n "$FRITZBOX_USER:$REALM:$FRITZBOX_PASS" | openssl dgst -md5 -hex | cut -d' ' -f2)
AUTH_HASH=$(echo -n "$SECRET_H1:$NONCE" | openssl dgst -md5 -hex | cut -d' ' -f2)

# --- STEP 3: Send the Final Authenticated Request ---
AUTH_XML="<?xml version='1.0' encoding='utf-8'?>
<s:Envelope xmlns:s='http://schemas.xmlsoap.org/soap/envelope/' s:encodingStyle='http://schemas.xmlsoap.org/soap/encoding/'>
  <s:Header>
    <h:ClientAuth xmlns:h='http://soap-authentication.org/digest/2001/10/' s:mustUnderstand='1'>
      <Nonce>$NONCE</Nonce>
      <Auth>$AUTH_HASH</Auth>
      <UserID>$FRITZBOX_USER</UserID>
      <Realm>$REALM</Realm>
    </h:ClientAuth>
  </s:Header>
  <s:Body>
    <u:$ACTION xmlns:u='$SERVICE_TYPE'></u:$ACTION>
  </s:Body>
</s:Envelope>"

FINAL_RESPONSE=$(curl -s -X POST "$SERVICE_URL" \
    -H "SoapAction: $SERVICE_TYPE#$ACTION" \
    -H "Content-Type: text/xml; charset=utf-8" \
    -d "$AUTH_XML")

echo "Result:"
echo "$FINAL_RESPONSE" | xmllint --format - 2>/dev/null || echo "$FINAL_RESPONSE"
```

## Request with strong (2Steps) Authentication 
```sh
<?xml version="1.0" encoding="utf-8"?>
<s:Envelope s:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/" xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <s:Header>
    <h:ClientAuth xmlns:h="http://soap-authentication.org/digest/2001/10/" s:mustUnderstand="1">
      <Nonce>F758BE72FB999CEA</Nonce>
      <Auth>b4f67585f22b0af7c4615db5a18faa14</Auth>
      <UserID>admin</UserID>
    </h:ClientAuth>
  </s:Header>
  <s:Body>
    <u:GetGenericHostEntry xmlns:u="urn:dslforum-org:service:Hosts:1">
      <NewIndex>0</NewIndex>
    </u:GetGenericHostEntry>
  </s:Body>
</s:Envelope>
```
