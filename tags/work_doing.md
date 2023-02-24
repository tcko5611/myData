```dataview
TABLE priority AS "Priority" FROM #Status/Progress SORT priority DESC
```
```dataviewjs
let pages = dv.pages("#Status/Progress");
dv.header(1, 'Working Jiras')
for (let group of pages.groupBy(b => b.priority).sort(b =>b.key, "desc")) {
 dv.header(2, 'Priority ' + group.key)
 dv.list(group.rows.sort(k=> k.file.link, "asc").file.link)
}
```
```dataviewjs 
/*
const {createButton} = app.plugins.plugins["buttons"] 
const {update} = app.plugins.plugins['metaedit'].api 
const defer = async (file, key) => { 
const value = await app.plugins.plugins['templater-obsidian'].templater.functions_generator.internal_functions.modules_array[4].static_functions.get('prompt')("What Date") 
const date = app.plugins.plugins['nldates-obsidian'].parseDate(value).moment.format("YYYY-MM-DD") 
await update(key, date, file) 
} 
dv.table( 
["Name", "Date", "Bouton"], 
dv.pages("#task").map(t => [t.file.link, t['date'], 
createButton({app, el: this.container, args: {name: "Change date"}, clickOverride: {click: defer, params: [t.file.path, 'date']}})]) 
) 
*/
```