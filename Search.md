Trilium supports searching in notes. There are several ways to search notes:

* full text search - search in text and [[code note|code notes]] content. Since this is implemented as a database search, this works only for not protected notes (doesn't matter if you're in protected session or not)

* [[attribute|Attributes]] search - you can e.g. search for notes having certain label - see *Attribute query syntax* below.

* you can save search string (composed of either fulltext or attribute search) into so-called "saved search" note. If you expand this note, you will see the search results as child notes.
  * saved search can also reference [[script|scripts]] through relation - upon expanding, the script will provide a list of results

You can activate search by clicking on magnifier icon on the left or pressing `CTRL-S` keyboard [[shortcut|keyboard shortcuts]].

## Fulltext

Fulltext search is triggered whenever search string doesn't start with `@` (used in attribute search) or `=` (saved search with script). 

Fulltext searches on both title and content of undeleted, unprotected text and code notes which are not [[archived|archived notes]].

Input string is tokenized by whitespace separators and each individual token (word) must be present in the title or content. If you don't want this automatic tokenization, you can surround your search with double quotes, e.g. `"hello world"` will search for exact match.

## Attribute query syntax

### Standard attributes

Here you query by standard [[attributes]]:

* `@abc` - returns notes with label abc
* `@!abc` - returns notes which don't have label abc
* `@year=2019` - matches notes with label `year` having value `2019`
* `@year!=2019` - matches notes with label `year`, but not having value 2019 or not having label `year` at all.
  * if you want to express condition that note must have `year` label, but not value 2019, then you can do that with `@year @year!=2019`
* `@year<=2000` - return notes with label `year` having value 2000 or smaller. `>`, `<`, `>=` and `<=` are supported.
  * In this case parameter can be parsed as a number so number-wise comparison is done. But it's also possible to compare strings lexicographically in the same way. This is useful for comparing dates as seen below.
* `@rock @pop` - matches notes which have both `rock` and `pop` labels
  * `@rock and @pop` is an alternative syntax for the same thing
* `@rock or @pop` - only one of the labels must be present
  * `and` has a precedence over `or` in case both operators are used in a search string
* `@frontMan=*John` - matches notes where `frontMan` label starts with "John". There's also `!=*` for "doesn't start with", `*=` for "ends with" and `!*=` for "does not end with", `*=*` for "contains" and `!*=*` for "does not contain"

### Virtual attributes

It's also possible to query by so called "virtual attributes":

* `dateCreated` - local date of note's creation date in YYYY-MM-DD HH:mm:ss.sss+OOPP where OOPP are hours and minutes of offset compared to UTC (e.g. "+0100" for Central European Time)
* `dateModified` - local date of last note's modification in YYYY-MM-DD HH:mm:ss.sss+OOPP
* `utcDateCreated` - UTC date of note's creation date in YYYY-MM-DD HH:mm:ss.sssZ
* `utcDateModified` - UTC date of last note's modification date in YYYY-MM-DD HH:mm:ss.sssZ
* `noteId`
* `isProtected` - 1 if the note is protected, 0 otherwise
* `title` - useful if you want to search title separately
* `content`
* `type` - `text`, `code`, `image`, `file`, `search` or `relation-map`
* `mime` - e.g. `text/html` for text note
* `text` - fulltext attribute of both title and content together
* `isArchived` - filters only for [[archived notes]], `@!archived` then filters only for non-archived notes. Note that this filter does not work in OR relation, it is always AND.
* `in` - for example `@in=VuvLpfAPx2L9` will filter only notes which have note with noteId `VuvLpfAPx2L9` as one of the ancestors - i.e. note is in the subtree of `VuvLpfAPx2L9`. With negating operator - `@in!=VuvLpfAPx2L9` will filter notes which don't have note `VuvLpfAPx2L9` as their ancestor. Since Trilium 0.37

### Order and limit

It's possible to order the results by one or more attribute, e.g. `@orderBy=year,-genre` will order notes by `year` label in ascending order and then by `genre` label in descending order (because of `-` sign).

You can also limit the number of results using e.g. `@limit=1` will return only the first result. If there's no `limit`, then all notes are returned.

### Date comparisons

Attribute values are untyped strings which means that although you can save into it any kind of value like number or date, Trilium doesn't really know what kind of value it is and can't really treat it properly.

Fortunately if we stick to storing dates in YYYY-MM-DD format, we can still do quite a lot since such dates can be correctly compared as strings - e.g. `@dateCreated>=2015-01-01` will return notes which were created since 2015 even though Trilium is doing just string comparison.

What if you want to find notes created in 2015? You can use "starts with" operator (`=*`) like this: `@dateCreated=*2019` - this will match notes with label starting with string "2019" which all notes created in that year will satisfy.

### Smart filters

Trilium supports few special values which will might help you with some search queries:

* `NOW` means this instant to second precision, e.g. `2019-04-01 10:12:54`
  * `NOW-3600` means 1 hour ago
* `TODAY` means today with the day precision, e.g. `2019-04-01`
  * `TODAY-1` means yesterday, `TODAY+1` means tomorrow
* `MONTH` means this month, e.g. `2019-04`
  * `MONTH-1` means last month - i.e. `2019-03`
* `YEAR` means current year, e.g. `2019`
  * `YEAR-1` means last year - 2018

Using this you can create queries like:

* `@dateModified>=NOW-3600` - show me notes changed in the last hour
* `@dateCreated=*TODAY-1` - show me notes created during whole yesterday

## Saved search

Trilium provides a way to save common search as a note in the note tree. Search results will then appear as subnotes of this "saved search" note. You can see how this works in action:

[[gifs/saved-search.gif]]

### Saved search with script relation

If saved search string starts with `=`, then the following string is taken as a relation name and the target script is executed to get the list of note results.

So let's say the search string is `=handler`, then Trilium tries to find this saved note's relation with name "handler". Relation is expected to point to [[script|scripts]] which is then executed - script is expected to return a list of notes which should be presented as search results. This option exists for use cases where normal attribute/fulltext search doesn't cover all needs.

## Auto trigger search from URL

Opening Trilium like in the example below will open search pane and automatically trigger search for "abc":

```
http://localhost:8080/#search=abc
```