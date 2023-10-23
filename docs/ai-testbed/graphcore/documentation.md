# Documentation links

 [Poplar SDK](https://docs.graphcore.ai/projects/sdk-overview/en/latest/overview.html?highlight=poptorch#introduction)<br>
 [PyTorch for the IPU: User Guide](https://docs.graphcore.ai/projects/poptorch-user-guide/en/latest/index.html)<br>
 [Targetting the IPU from Tensorflow 2](https://docs.graphcore.ai/projects/tensorflow-user-guide/en/latest/index.html) <br>
 [IPU programming guide](https://docs.graphcore.ai/projects/ipu-programmers-guide/en/latest/index.html) <br>
 [Examples](https://docs.graphcore.ai/projects/tutorials/en/latest/intro.html) <br>
 [Examples Github Repo](https://github.com/graphcore/examples) <br>
 [POD systems](https://docs.graphcore.ai/projects/ipu-pod-getting-started/en/latest/overview.html?highlight=PopTorch#pod-overview) <br> 
 [POD64 specs](https://www.graphcore.ai/products/bow-pod64#product-spec) <br>

[Common memory optimisations](https://docs.graphcore.ai/projects/memory-performance-optimisation/en/latest/common-memory-optimisations.html)<br>
Of the memory optimizations:<br>

* Using FP16 for the partials will often be helpful (accuracy may suffer)<br>
E.g., for a PyTorch model,
```python
gcopts = poptorch.Options()
#...
gcopts.Precision.setPartialsType(torch.half)
```
* Reducing the availableMemoryProportion, the amount of memory on device reserved for temporary data whilst the operation is executing, can be helpful (with no effect on accuracy). This is set for each IPU used by a single model instance (replica). (The default is 0.6 (60% of tile memory))
E.g., for a PyTorch model that uses 8 IPUs per replica, to reserve 15 percent of memory for temporary data, 
```python
gcopts = poptorch.Options()
#...
mem_prop = {f"IPU{i}": 0.15 for i in range(8)}
gcopts.setAvailableMemoryProportion(mem_prop)
```
