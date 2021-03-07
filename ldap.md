find owner of account
```sh
ldapsearch -LLL -o ldif-wrap=no -h ubsinfesv0015.vantage.org -b "DC=vantage,DC=org" samaccountname=pen_import-s
```
