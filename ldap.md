## implementation
* [openldap](https://www.openldap.org/)

## [browsers](https://ldapwiki.com/wiki/LDAP%20Browsers)
## [browsers](https://ldap.com/ldap-tools/)

## commands
whoami
```sh
ldapwhoami -x -v -D "CN=Vitalii Cherkashyn,OU=Users,OU=UBS,OU=Accounts,DC=vantage,DC=org" -H ldaps://ubsinfesv0015.vantage.org:636 -W
# CN - Common Name
# OU - Organizational Unit
# DC - Domain Component
```

find owner of account
```sh
LDAP_HOST=ubsinfesv0015.vantage.org
ldapsearch -LLL -o ldif-wrap=no -h $LDAP_HOST -b "DC=vantage,DC=org" samaccountname=pen_import-s
ldapsearch -LLL -o ldif-wrap=no -h $LDAP_HOST -b "OU=Accounts,DC=vantage,DC=org" samaccountname=cherkavi
ldapsearch -LLL -o ldif-wrap=no -h $LDAP_HOST -b "OU=Accounts,DC=vantage,DC=org" -s sub "displayName=Vitalii Cherkashyn"
ldapsearch -LLL -o ldif-wrap=no -h $LDAP_HOST -b "OU=Accounts,DC=vantage,DC=org" -s sub "Mail=vitalii.cherkashyn@ubs.de"
ldapsearch -LLL -o ldif-wrap=no -h $LDAP_HOST -b "OU=Accounts,DC=vantage,DC=org" -s sub "Mail=vitalii.cherkashyn@ubs.de" -D "CN=Vitalii Cherkashyn,OU=Users,OU=UBS,OU=Accounts,DC=vantage,DC=org" -Q -W
```
```sh
# in case of error message: No Kerberos credentials available
kinit pen_import-s
```
find all accounts in LDAP
```sh
# list of the accounts
ldapsearch -LLL -o ldif-wrap=no -E pr=1000/noprompt -h $LDAP_HOST -b "DC=vantage,DC=org" samaccountname=r-d-ubs-developer member 
# account name and e-mail 
ldapsearch -LLL -o ldif-wrap=no -E pr=1000/noprompt -h $LDAP_HOST -b "DC=vantage,DC=org" cn="Vitalii Cherkashyn" samaccountname
ldapsearch -LLL -o ldif-wrap=no -E pr=1000/noprompt -h $LDAP_HOST -b "DC=vantage,DC=org" cn="Vitalii Cherkashyn" samaccountname mail
```

## Architecture
![image](https://user-images.githubusercontent.com/8113355/187679898-4631dc98-f763-4184-872b-989f91c46208.png)
![image](https://user-images.githubusercontent.com/8113355/187679985-f920ee28-c0f0-4160-bf0c-6f51238e728f.png)


## links
* [what is ldap](https://www.securew2.com/blog/ldap-explained)
* [how ldap works](https://jumpcloud.com/blog/what-is-ldap#how-does-ldap-work)
* [ldapsearch](https://www.junosnotes.com/linux/how-to-search-ldap-using-ldapsearch-examples/)
