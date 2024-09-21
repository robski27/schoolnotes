#CS3 

```dataview
TABLE description,usage, file.cday as "creation date", tags
FROM #command 
where file.name != "command"
sort file.name

```