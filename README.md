[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg?style=flat-square)](https://opensource.org/licenses/MIT)
[![CodeCov](https://github.com/cda-tum/mqt-qubomaker/actions/workflows/coverage.yml/badge.svg)](https://github.com/cda-tum/mqt-qubomaker/actions/workflows/coverage.yml)
[![Deploy to PyPI](https://github.com/cda-tum/mqt-qubomaker/actions/workflows/deploy.yml/badge.svg)](https://github.com/cda-tum/mqt-qubomaker/actions/workflows/deploy.yml)
[![codecov](https://codecov.io/gh/cda-tum/mqt-qubomaker/graph/badge.svg?token=WB16FSW7OZ)](https://codecov.io/gh/cda-tum/mqt-qubomaker)
[![Documentation](https://img.shields.io/readthedocs/mqt-qubomaker?logo=readthedocs&style=flat-square)](https://mqt-qubomaker.readthedocs.io/en/latest/)

<p align="center">
<picture>
  <a href="https://mqt.readthedocs.io/"><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/cda-tum/mqt-qubomaker/main/docs/_static/mqt_light.png" width="60%"></a>
  <a href="https://mqt.readthedocs.io/"><img src="https://raw.githubusercontent.com/cda-tum/mqt-qubomaker/main/docs/_static/mqt_dark.png" width="60%"></a>
</picture>
</p>

# MQT QUBOMaker: Automatic Generation of QUBO Formulations from Optimization Problem Specifications

MQT QUBOMaker is a framework that can be used to automatically generate QUBO formulations for various optimization problems based on a selection of constraints that define the problem.
It is developed by the [Chair for Design Automation](https://www.cda.cit.tum.de/) at the [Technical University of Munich](https://www.tum.de/) as part of the _[Munich Quantum Toolkit](https://mqt.readthedocs.io/) (MQT)_.

The tool allows users to create QUBO formulations, and, thus, interact with quantum algorithms, without requiring any background knowledge in the field of quantum computing. End-users can stay entirely within their domain of expertise while being shielded from the complex and error-prone mathematical tasks of QUBO reformulation.

Furthermore, MQT QUBOMaker supports a variety of different encodings. End users can easily switch between the encodings for evaluation purposes without any additional effort, a task that would otherwise require a large amount of tedious mathematical reformulation.

Currently, MQT QUBOMaker provides the following submodule:

- [_Pathfinder_](./src/mqt/qubomaker/pathfinder/README.md): This submodule provides a specialization of the QUBOMaker class for the solution of optimization problems involving the search for paths in a directed graph. It provides a large set of pathfinding-related constraints that are used to define individual problem instances.

The _Pathfinder_ submodule also has a supporting [GUI](https://cda-tum.github.io/mqt-qubomaker/) to further facilitate its use.

For more details, please refer to:

<p align="center">
  <a href="https://mqt-qubomaker.readthedocs.io/en/latest/">
  <img width=30% src="https://img.shields.io/badge/documentation-blue?style=for-the-badge&logo=read%20the%20docs" alt="Documentation" />
  </a>
</p>

If you have any questions, feel free to create a [discussion](https://github.com/cda-tum/mqt-qubomaker/discussions) or an [issue](https://github.com/cda-tum/mqt-qubomaker/issues) on [GitHub](https://github.com/cda-tum/mqt-qubomaker).

## Getting Started

`mqt-qubomaker` is available via [PyPI](https://pypi.org/project/mqt.qubomaker/).

```console
(venv) $ pip install mqt.qubomaker
```

The following code gives an example of the usage with the `pathfinder` submodule:

```python3
import mqt.qubomaker as qm
import mqt.qubomaker.pathfinder as pf

# define an example graph to investigate.
graph = qm.Graph.from_adjacency_matrix(
    [
        [0, 1, 3, 4],
        [2, 0, 4, 2],
        [1, 5, 0, 3],
        [3, 8, 1, 0],
    ]
)

# select the settings for the QUBO formulation.
settings = pf.PathFindingQUBOGeneratorSettings(
    encoding_type=pf.EncodingType.ONE_HOT, n_paths=1, max_path_length=4, loops=True
)

# define the generator to be used for the QUBO formulation.
generator = pf.PathFindingQUBOGenerator(
    objective_function=pf.MinimizePathLength(path_ids=[1]),
    graph=graph,
    settings=settings,
)

# add the constraints that define the problem instance.
generator.add_constraint(pf.PathIsValid(path_ids=[1]))
generator.add_constraint(
    pf.PathContainsVerticesExactlyOnce(vertex_ids=graph.all_vertices, path_ids=[1])
)

# generate and view the QUBO formulation as a QUBO matrix.
print(generator.construct_qubo_matrix())
```

**Detailed documentation and examples are available at [ReadTheDocs](https://mqt-qubomaker.readthedocs.io/en/latest/).**

## References

MQT QUBOMaker has been developed based on methods proposed in the following paper:

- D. Rovara, N. Quetschlich, and R. Wille "[A Framework to Formulate
  Pathfinding Problems for Quantum Computing](https://todo)", arXiv, 2024

## Acknowledgements

The Munich Quantum Toolkit has been supported by the European
Research Council (ERC) under the European Union's Horizon 2020 research and innovation program (grant agreement
No. 101001318), the Bavarian State Ministry for Science and Arts through the Distinguished Professorship Program, as well as the
Munich Quantum Valley, which is supported by the Bavarian state government with funds from the Hightech Agenda Bayern Plus.

<p align="center">
<picture>
<source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/cda-tum/mqt-qubomaker/main/docs/_static/tum_dark.svg" width="28%">
<img src="https://raw.githubusercontent.com/cda-tum/mqt-qubomaker/main/docs/_static/tum_light.svg" width="28%">
</picture>
<picture>
<img src="https://raw.githubusercontent.com/cda-tum/mqt-qubomaker/main/docs/_static/logo-bavaria.svg" width="16%">
</picture>
<picture>
<source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/cda-tum/mqt-qubomaker/main/docs/_static/erc_dark.svg" width="24%">
<img src="https://raw.githubusercontent.com/cda-tum/mqt-qubomaker/main/docs/_static/erc_light.svg" width="24%">
</picture>
<picture>
<img src="https://raw.githubusercontent.com/cda-tum/mqt-qubomaker/main/docs/_static/logo-mqv.svg" width="28%">
</picture>
</p>
