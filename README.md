# pyXDSM

A python library for generating publication quality PDF XDSM diagrams.
This library is a thin wrapper that uses the TIKZ library and LaTeX to build the PDFs.

![XDSM of MDF](https://github.com/mdolab/pyXDSM/blob/master/images_for_readme/mdf.png)

# Installation
Clone this repo or download the zip and unzip it.

    cd pyxdsm
    pip install .




## What is XDSM?
The eXtended Design Struture Matrix (XDSM) is a graphical language for describing the movement of data and the execution sequence for a  multidisciplinary optimization  problem.
You can read the [paper by Lamb and Martins](http://mdolab.engin.umich.edu/content/extensions-design-structure-matrix) for all the details.
If you  would like a citation for XDSM, here is the bibtex for that paper:

    @article {Lambe2012,
    title = {Extensions to the Design Structure Matrix for the Description of Multidisciplinary Design, Analysis, and Optimization Processes},
    journal = {Structural and Multidisciplinary Optimization},
    volume = {46},
    year = {2012},
    pages = {273-284},
    doi = {10.1007/s00158-012-0763-y},
    author = {Andrew B. Lambe and Joaquim R. R. A. Martins}
    }


## TIKZ and LaTeX?
You need to install these libraries for pyXDSM to work. See the [install guide](https://www.latex-project.org/get/) for your platform

## How do I use it?
Here is a simple example. There are some other more advanced things you can do as well. Check out the [examples folder](https://github.com/mdolab/pyXDSM/blob/master/examples)
```python
from pyxdsm.XDSM import XDSM

opt = 'Optimization'
solver = 'MDA'
proc = 'ProcessTip'
comp = 'Analysis'
group = 'Metamodel'
func = 'Function'

x = XDSM()

x.add_system('opt', opt, 'Optimizer')
x.add_system('solver', solver, 'Newton')
x.add_system('D1', comp, r'$D_1$')
x.add_system('D2', comp, r'$D_2$')
x.add_system('F', func, r'$F$')
x.add_system('G', func, r'$G$')


x.connect('opt', 'D1', r'$x, z$')
x.connect('opt', 'D2', r'$z$')
x.connect('opt', 'F', r'$x, z$')
x.connect('solver', 'D1', r'$y_2$')
x.connect('solver', 'D2', r'$y_1$')
x.connect('D1', 'solver', r'$\mathcal{R}(y_1)$')
x.connect('solver', 'F', r'$y_1, y_2$')
x.connect('D2', 'solver', r'$\mathcal{R}(y_2)$')
x.connect('solver', 'G', r'$y_1, y_2$')


x.connect('F', 'opt', r'$f$')
x.connect('G', 'opt', r'$g$')

x.add_output('opt', r'$x^*, z^*$', side='left')
x.add_output('D1', r'$y_1^*$', side='left')
x.add_output('D2', r'$y_2^*$', side='left')
x.add_output('F', r'$f^*$', side='left')
x.add_output('G', r'$g^*$', side='left')
x.write('mdf')
```
![XDSM of MDF](https://github.com/mdolab/pyXDSM/blob/master/images_for_readme/mdf.png)
