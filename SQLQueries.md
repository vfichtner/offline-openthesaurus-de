# SQL Abfragen #

Hier folgen die zwei SQL Abfragen die in der Anwendung verwendet werden.
Die Abfragen wurden eins zu eins aus dem Quellcode kopiert, bei der
Variable "txt" handelt es sich um das Suchwort.

Alle Optimierungsvorschläge sind willkommen.


**Query 1:** Synonym Suche

```
			final String query ="SELECT DISTINCT t2._id, t2.word, tl.short_level_name, "
			+ " c.category_name FROM term t"
			+ " LEFT JOIN synset s ON t.synset_id =s._id"
			+ " LEFT JOIN term t2 ON t2.synset_id = s._id"
			+ " LEFT JOIN category_link cl ON t2.synset_id = cl.synset_id"
			+ " LEFT JOIN category c ON c._id = cl.category_id"
			+ " LEFT JOIN term_level tl ON t2.level_id = tl.id"
			+ " WHERE t.word like ?"
			+ " OR t.normalized_word like ?"
			+ " GROUP BY t2.word";
			
			retCursor = sqliteDatabase.rawQuery(query, new String[]{txt,txt});	
```

**Query 2:** Autocomplete Anfrage

```
			String query = "SELECT DISTINCT word,_id FROM term"+ 
			" WHERE word like \""+txt+"%\""+
			" GROUP BY word LIMIT 15;";
```