# Anki 
## access DB
```sh
# db place
$USER/.local/share/Anki2/{anki_account_email_address}

sqlite3 collection.anki2
```

### fix cards in DB 
```sql
select * from decks;
select * from cards;

SELECT id, flds FROM notes WHERE flds LIKE '%gewissen%'; -- id of record 
-- 	Field data, joined by \x1f (unit separator) — e.g., "Front text\x1fBack text"
UPDATE notes SET flds = 'updated\x1ftext', mod = strftime('%s','now') WHERE id = 1743965509382;
```

## [access via REST](https://github.com/cherkavi/solutions/tree/master/anki-rest-api/)

## [anki solution: save images as cards](https://github.com/cherkavi/solutions/tree/master/anki-save-images)
## [anki solution: save google translator history](https://github.com/cherkavi/solutions/tree/master/anki-rest-api/)