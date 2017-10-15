Myvi
======================
Myvi is a 3D visualization tool, just like a simple ang light Mayavi.

### A sigle ball
```python
# give position, r, and color
vts, fs, ns, cs = myvi.build_ball((100,100,100), 50, (1,0,0))

manager = myvi.Manager()
manager.add_obj('balls', vts, fs, ns, cs)
manager.show('Ball Demo')
```
![](http://myvi.imagepy.org/imgs/ball.jpg "ball")

### Random balls with random r and color
```python
os = np.random.rand(30).reshape((-1,3))
rs = np.random.rand(10)/5
cs = (np.random.rand(10)*255).astype(np.uint8)
cs = myvi.linear_color('jet')[cs]/255

vts, fs, ns, cs = myvi.build_balls(os, rs, cs)
manager = myvi.Manager()
manager.add_obj('balls', vts, fs, ns, cs)
manager.show('Random Balls Demo')
 ```
 ![](http://myvi.imagepy.org/imgs/balls.jpg "balls")
 
 ### Lines
```python
vts = np.array([(0,0,0),(1,1,0),(2,1,0),(1,0,0)], dtype=np.float32)
fs = np.array([(0,1,2),(1,2,3)], dtype=np.uint32)
ns = np.ones((4,3), dtype=np.float32)

n_mer, n_long = 6, 11
pi = np.pi
dphi = pi / 1000.0
phi = np.arange(0.0, 2 * pi + 0.5 * dphi, dphi)
mu = phi * n_mer
x = np.cos(mu) * (1 + np.cos(n_long * mu / n_mer) * 0.5)
y = np.sin(mu) * (1 + np.cos(n_long * mu / n_mer) * 0.5)
z = np.sin(n_long * mu / n_mer) * 0.5

vts, fs, ns, cs = myvi.build_line(x, y, z, (1, 0, 0))
cs[:] = myvi.auto_lookup(vts[:,2], myvi.linear_color('jet'))/255

manager = myvi.Manager()
obj = manager.add_obj('line', vts, fs, ns, cs)
obj.set_style(mode='grid')
manager.show('Line Rings')
 ```
 ![](http://myvi.imagepy.org/imgs/line.jpg "line")
 
 ### grid and mesh
```python
dphi, dtheta = np.pi/20.0, np.pi/20.0  
[phi,theta] = np.mgrid[0:np.pi+dphi*1.5:dphi,0:2*np.pi+dtheta*1.5:dtheta]  
m0 = 4; m1 = 3; m2 = 2; m3 = 3; m4 = 6; m5 = 2; m6 = 6; m7 = 4;  
r = np.sin(m0*phi)**m1 + np.cos(m2*phi)**m3 + np.sin(m4*theta)**m5 + np.cos(m6*theta)**m7  
x = r*np.sin(phi)*np.cos(theta)  
y = r*np.cos(phi)  
z = r*np.sin(phi)*np.sin(theta)  
vts, fs, ns, cs = myvi.build_mesh(x, y, z)
cs[:] = myvi.util.auto_lookup(vts[:,2], myvi.util.linear_color('jet'))/255

manager = myvi.Manager()
obj = manager.add_obj('mesh', vts, fs, ns, cs)
obj.set_style(mode='grid')
manager.show('Mesh Demo')
 ```
 ![](http://myvi.imagepy.org/imgs/grid.jpg "grid")
#### set the draw mode and color on the viewer interactivily.
 ![](http://myvi.imagepy.org/imgs/mesh.jpg "mesh")
 
### Digital Elevation Model
```python
img = imread('data/dem.png')
vts, fs, ns, cs = myvi.util.build_surf2d(img, ds=1, k=0.3, sigma=2)

manager = myvi.Manager()
manager.add_obj('dem', vts, fs, ns, cs)
manager.show('DEM Demo')
 ```
 ![](http://myvi.imagepy.org/imgs/dem.jpg "dem")
 
### Surface from image sequence
```python
fs = glob('data/vessel*.png')
imgs = np.array([imread(i, True) for i in fs])
imgs = ndimg.gaussian_filter(imgs, 1)
vts, fs, ns, vs = myvi.util.build_surf3d(imgs, 1, 128)

manager = myvi.Manager()
manager.add_obj('vessel', vts, fs, ns, (1,0,0))
manager.show('Vessel Demo')
 ```
 ![](http://myvi.imagepy.org/imgs/vessel.jpg "vessel")
 
 ### Embed in your ui program
 myvi.Viewer3D is a wxpanel, which contains a manager, implements render and interactive. you can just put the viewer in your program, then you can add object in.
 
 ```python
class YourFrame(wx.Frame):
	def __init__(self, parent, title='Frame3D', manager=None):
		wx.Frame.__init__(...)
		...
		self.viewer = Viewer3D(self)
        self.viewer.add_obj('name', vts, fs, ns, cs)
		...
 ```
 
 ## Documents

> **ske:** should be a nd skeleton image
> **return:** is a networkx Graph object
### graph detail:
> **graph.node[id]['pts'] :** Numpy(x, n), coordinates of nodes points
> **graph.node[id]['o']:** Numpy(n), centried of the node
> **graph.edge(id1, id2)['pts']:** Numpy(x, n), sequence of the edge point
> **graph.edge(id1, id2)['weight']:** float, length of this edge


### Build Graph:
build Graph by Skeleton, then plot as a vector Graph in matplotlib.

![](http://home.imagepy.org/sknw/buildgraph.png "??")
### Find Path
then you can use networkx do what you want
![](http://home.imagepy.org/sknw/findpath.png "??")
### 3D Skeleton
sknw can works on nd image, this is a 3d demo by mayavi
![](http://home.imagepy.org/sknw/3dgraph.png "??")