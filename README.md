# FBX Parser

A Jupyter notebook for exploring skeletal and motion capture data in FBX files using the Autodesk FBX Python SDK and PyMo.

## Features

- Read skeleton hierarchies, joint relationships, and local offsets.
- Inspect scene metadata, node properties, and rotation settings.
- Extract translation and rotation animation curves into pandas DataFrames.
- Build a base pose and visualize skeletons with PyMo.

## Requirements

- Python and Jupyter Notebook
- Autodesk FBX Python SDK, including `fbx` and `FbxCommon`, compatible with your Python environment
- NumPy, pandas, Matplotlib, and scikit-learn
- PyMo and the mocolib setup used by the original notebook

The notebook records these dependency revisions:

| Dependency | Commit |
| --- | --- |
| PyMo | `fdb93e644f7df014fbed2bbffa6c88adbda4afcf` |
| mocolib | `c818e9743cdedc7ab9098dd83ff3c5ac8e686dc7` |

## Usage

1. Install the dependencies and make sure `fbx`, `FbxCommon`, and `pymo` can be imported in your notebook environment.
2. Open [`fbx_parser.ipynb`](fbx_parser.ipynb).
3. Replace the example `filepath` with the path to your own FBX file.
4. Run the imports and `FBX_Class` definition, then load the file:

```python
fbx_scene = FBX_Class("path/to/your_file.fbx")
fbx_scene.root_name = "pelvis"  # Use "Hips" for CMU-style skeletons.
data = [fbx_scene.parse()]

print_skel(data[0])
print(data[0].values.head())
```

Use the remaining cells to inspect the scene, convert rotations to joint positions, and draw 2D or 3D stick figures.

To extract animation curves directly:

```python
animation = fbx_scene.get_animation()
print(animation.head())
```

## Current limitations

- This is an experimental notebook with skeleton-specific adjustments for PXR and CMU examples.
- `parse()` currently assigns a single-frame base pose to `data.values`. To use animation instead, change `self.data.values = self.create_base_pose()` to `self.data.values = anim_data` inside `parse()`.
- Some base-pose joints, including clavicles, wings, and weapons, may have incorrect positions.
- Animation extraction returns the last processed stack/layer and constructs timestamps assuming uniformly spaced keys. Files without animation stacks may require changes.
- Example FBX files are not included; provide your own input files.
