---
title: "Cheap and Fair demo Data Repository"
author: "Cheap and Fair team"
description: "Demo data repository"
date_created: "2024-09-12"
---

<script src="https://cdn.jsdelivr.net/pyodide/v0.26.1/full/pyodide.js"></script>
<a href="jup/">Test pyhealpy in JupyterLite</a>

<script type="text/javascript">
  async function main(){
    let pyodide = await loadPyodide();

await pyodide.loadPackage("micropip");
await pyodide.loadPackage("ssl");
const micropip = pyodide.pyimport("micropip");
await micropip.install('https://healpy.github.io/pyhealpy/dist/healpy-0.1.0-py3-none-any.whl');
await micropip.install('matplotlib');
const version = document.getElementById("healpyversion");
version.textContent = pyodide.runPython("import healpy as hp; hp.__version__");

pyodide.runPythonAsync(`
import matplotlib
matplotlib.use("module://matplotlib_pyodide.wasm_backend")
import healpy as hp
import numpy as np

import matplotlib.pyplot as plt
from pyodide.http import pyfetch
response = await pyfetch("https://g-1926f5.c2d0f8.bd7c.data.globus.org/myfolder5/dust/dust_023GHz.fits")

content = await response.bytes()
with open("a.fits", 'wb') as f:
    f.write(content)

await response.unpack_archive()
m = hp.read_map(
"a.fits"
)
hp.projview(m, coord=["G"], projection_type="mollweide")
plt.show()
`);
  }
  main();
</script>
