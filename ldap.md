## implementation
* [openldap](https://www.openldap.org/)

## [browsers](https://ldapwiki.com/wiki/LDAP%20Browsers)
## [browsers](https://ldap.com/ldap-tools/)

## commands
whoami
```sh
ldapwhoami -x -v -D "CN=Vitalii Cherkashyn,OU=Users,OU=BMW,OU=Accounts,DC=vantage,DC=org" -H ldaps:///ubsinfesv0015.vantage.org:636 -W
```
find owner of account
```sh
ldapsearch -LLL -o ldif-wrap=no -h ubsinfesv0015.vantage.org -b "DC=vantage,DC=org" samaccountname=pen_import-s
```
## Architecture
![image](https://user-images.githubusercontent.com/8113355/187679898-4631dc98-f763-4184-872b-989f91c46208.png)
![image](https://user-images.githubusercontent.com/8113355/187679985-f920ee28-c0f0-4160-bf0c-6f51238e728f.png)


## links
* [what is ldap](https://www.securew2.com/blog/ldap-explained)
* [how ldap works](https://jumpcloud.com/blog/what-is-ldap#how-does-ldap-work)
* [ldapsearch](https://www.junosnotes.com/linux/how-to-search-ldap-using-ldapsearch-examples/)
