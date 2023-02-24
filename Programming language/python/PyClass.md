# Class

```python
from threading import Thread
from multiprocessing import Process
import matplotlib.pyplot as plt
import matplotlib as mpl
import matplotlib.image as mpimg
# from PIL import Image
import sys 
class TaskProxy(Thread):
  def __init__(self, fileName):
    super(TaskProxy, self).__init__()
    self.fileName = fileName
  def run(self):
    p = Process(target=self.plot, args=())
    p.start()
    p.join()
  def plot(self):
    mpl.rcParams['toolbar'] = 'None' 
    img = mpimg.imread(self.fileName)
    fig = plt.figure(figsize=(2,1.5))
    plt.tick_params( axis='both', which='both', bottom='off', top='off', labelbottom='off', left='off', right='off', labelleft='off') 
    imgplot = plt.imshow(img)
    title = self.fileName[self.fileName.rfind('/')+ 1: self.fileName.rfind('.v.png')]
    fig.canvas.set_window_title(title)
    plt.show()          

if __name__ == '__main__':
  fileName = sys.argv[1]
  p = TaskProxy(fileName)
  p.plot()
```