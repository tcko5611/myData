# JavaScript
## Query
### dv.current()
Get currentpage information
### dv.pages(source)
```
dv.pages() => all pages in your vault 
dv.pages("#books") => all pages with tag 'books'
dv.pages('"folder"') => all pages from folder "folder" 
dv.pages("#yes or -#no") => all pages with tag #yes, or which DON'T have tag #no 
dv.pages('"folder" or #tag') => all pages with tag #tag, or from folder "folder"
```
### dv.pagePaths(source)
Get pages path (data array)
### dv.page(path)
```
dv.page("Index") => The page object for /Index 
dv.page("books/The Raisin.md") => The page object for /books/The Raisin.md
```
## Render
```
dv.el(element, text)
dv.header(level, text)
dv.paragraph(text)
dv.span(text)
dv.execute(source)
dv.executeJs("dv.list([1, 2, 3])");
```
## Dataviews
### dv.list(elements)
```
dv.list([1, 2, 3]) => list of 1, 2, 3
dv.list(dv.pages().file.name) => list of all file names
dv.list(dv.pages().file.link) => list of all file links
dv.list(dv.pages("#book").where(p => p.rating > 7)) => list of all books with rating greater than 7
```
### dv.taskList(tasks, groupByFile)
```
// List all tasks from pages marked '#project'
dv.taskList(dv.pages("#project").file.tasks)

// List all *uncompleted* tasks from pages marked #project
dv.taskList(dv.pages("#project").file.tasks
    .where(t => !t.completed))

// List all tasks tagged with '#tag' from pages marked #project
dv.taskList(dv.pages("#project").file.tasks
    .where(t => t.text.includes("#tag")))
```
### dv.table(headers, elements)
```
// Render a simple table of book info sorted by rating.
dv.table(["File", "Genre", "Time Read", "Rating"], dv.pages("#book")
    .sort(b => b.rating)
    .map(b => [b.file.link, b.genre, b["time-read"], b.rating])
```
## Examples
### Grouped Books
```
for (let group of dv.pages("#book").groupBy(p => p.genre)) {
    dv.header(3, group.key);
    dv.table(["Name", "Time Read", "Rating"],
        group.rows
            .sort(k => k.rating, 'desc')
            .map(k => [k.file.link, k["time-read"], k.rating]))
}
```