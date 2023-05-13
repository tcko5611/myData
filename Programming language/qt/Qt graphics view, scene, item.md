---
date : 2023-04-28 08:53
priority : 1
---
# Metadata
Status ::
Type ::
# Question
How to handle the architecture?
# Answer
# Note
## QGraphicsView
* **setScene(scene)**
* *setRenderHint(QPainter::RenderHints, false)*
* *setOptimizationFlags(QGraphicsView::OptimizationFlags)*
* *setViewportUpdateMode(QGraphicsView::ViewportUpdateMode)*
* *setTransformationAnchor(QGraphicsView::ViewportAnchor)*
* **setDragMode(mode)**
	* *QGraphicsView::NoDrag* : Nothing
	* *QGraphicsView::ScrollHandDrag* : Drag with scrollbars
	* *QGraphicsView::RubberBandDrag* : Dragging items with dubber band
* **setInteractive(b)**
	* *true* : allow scene interaction
	* *false* : view is read-only
* **wheelEvent(e)** : override, protected
	* using *e->modifiers() & Qt::ControlModifier*, *e->delta()* and *e->accept()* or *QGraphicsView::wheelEvent(e)*
* **setMatrix(m)**
	* *m* : is *QMatrix*:
		* matrix.scale(f2, f2) : maybe 1/32 or 32 is good
		* matrix.rotate(degree) 
* *ensureVisible(rect, xmargin = 50, ymargin = 50)*
* *render(paiter)*
	* renders source rect.
## QGraphicsItem
* **setPos(point)**
	* position in parent coordinates
* **boundingRect()** : override
	* painting must inside the rect
* **shape()**: overide
	* collision detection... can be like *path.addEllipse(boundingRect())*
* **paint(painter, item, widget)** : override
	* to paint the shape
*  *mousePressEvent(event)* : override
* *mouseMoveEvent(event)* : override
* *mouseReleaseEvent(event)* : override
## QGraphicsScene
* *addItem(item)*
	* add QGraphicsItem