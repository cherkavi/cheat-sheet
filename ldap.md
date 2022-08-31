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

## links
* [what is ldap](https://www.securew2.com/blog/ldap-explained)
* [what is ldap](https://jumpcloud.com/blog/what-is-ldap)
* [ldapsearch](https://www.junosnotes.com/linux/how-to-search-ldap-using-ldapsearch-examples/)
