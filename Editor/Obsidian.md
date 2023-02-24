# Open a new database
From "Open another vault" button, you can create a new vault.
# Themes and CSS
## Themes
Settings -> Appearance->Base themes : dark
Settings -> Appearance->Themes : select theme
## CSS

Setting -> Appearance -> CSS snippets -> Open snippets folder
list.css
```css
/* Bullet point line relationship */
.cm-hmd-list-indent .cm-tab, ul ul { position: relative; }
.cm-hmd-list-indent .cm-tab::before, ul ul::before {
  content:'';
  border-left: 3px solid rgba(0, 122, 255, 0.25);
  position: absolute;
  }
.cm-hmd-list-indent .cm-tab::before { 
  left: 0; 
  top: -5px; 
  bottom: -4px;
}
ul ul::before { left: -11px; top: 0; bottom: 0;}
```
grap.css
```css
.theme-dark .graph-view.color-fill-unresolved {color: #9e8aff;Opacity:1;}
```
remove  strikethrough for checkboxws
```css
/* use for view mode*/
.markdown-preview-view ul > li.task-list-item.is-checked {
	text-decoration:none;
  color:var(--text-normal);
}

.markdown-preview-view ol > li.task-list-item.is-checked {
  text-decoration:none;
  color:var(--text-normal);
}
/* use for edit mode */
.markdown-source-view.mod-cm6 .HyperMD-task-line[data-task]:not([data-task=" "]) {
    text-decoration:none;
    color: var(--text-normal);
}
```
# Unilities
## flowchart
using [[mermaid]] to create flowchart
```
---mermaid
flowchart TD  
    A[Start] --> B{Is it?}  
    B -- Yes --> C[OK]  
    C --> D[Rethink]  
    D --> B  
    B -- No ----> E[End]    
```