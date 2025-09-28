# Gradio Molecule3D

<a href="https://pypi.org/project/gradio_molecule3d/" target="_blank"><img alt="PyPI - Version" src="https://img.shields.io/pypi/v/gradio_molecule3d"></a>
<img alt="Python" src="https://img.shields.io/badge/python-3.8+-blue.svg">
<img alt="License" src="https://img.shields.io/badge/license-Apache%202.0-green.svg">

**A Gradio custom component for 3D molecular structure visualization with automatic prediction**

This component provides instant 3D visualization of molecular structures with **no "Predict" button required** - just upload your molecular file and see immediate 3D rendering using Jmol colors and optimized representations.

## ✨ Features

- 🚀 **Automatic Visualization** - No predict button needed, structures appear instantly on file upload
- 🎨 **Jmol Color Scheme** - Standard molecular colors with customizable scales
- 🔬 **Multiple Formats** - Supports PDB, CIF, SDF, MOL2, XYZ, and more
- ⚡ **Optimized Rendering** - Dual sphere + stick representation for clear visualization
- 🧪 **Crystal Support** - Automatic expansion of crystal structures for better viewing

## Installation

### From GitHub (Recommended)

```bash
pip install git+https://github.com/YOUR_USERNAME/gradio-molecule3d.git
```

### Local Development

```bash
git clone https://github.com/YOUR_USERNAME/gradio-molecule3d.git
cd gradio-molecule3d
pip install -e .
```

## Quick Start

### Automatic Visualization (No Predict Button)

```python
import gradio as gr
from gradio_molecule3d import Molecule3D

# Create demo with automatic visualization
with gr.Blocks() as demo:
    gr.Markdown("# 🧬 Molecular Structure Viewer")
    
    with gr.Row():
        with gr.Column():
            # File upload
            file_input = gr.File(
                label="Upload Molecular Structure",
                file_types=[".pdb", ".cif", ".sdf", ".mol2", ".xyz"]
            )
        
        with gr.Column():
            # 3D viewer with Jmol colors (default)
            molecule_viewer = Molecule3D(label="3D Structure")
    
    # 🎯 Auto-predict: Upload triggers immediate visualization
    file_input.upload(
        fn=lambda x: x,
        inputs=file_input,
        outputs=molecule_viewer
    )

if __name__ == "__main__":
    demo.launch()
```

### Custom Representations

```python
import gradio as gr
from gradio_molecule3d import Molecule3D

# Custom representation configuration
custom_reps = [
    {
        "model": 0,
        "style": "cartoon",
        "color": "hydrophobicity",
        "around": 0,
        "byres": False,
    },
    {
        "model": 0,
        "chain": "A",
        "resname": "HIS",
        "style": "stick",
        "color": "red"
    }
]

with gr.Blocks() as demo:
    molecule_viewer = Molecule3D(
        label="Custom Styled Molecule",
        reps=custom_reps
    )
    
demo.launch()
```

## `Molecule3D`

### Initialization

<table>
<thead>
<tr>
<th align="left">name</th>
<th align="left" style="width: 25%;">type</th>
<th align="left">default</th>
<th align="left">description</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left"><code>value</code></td>
<td align="left" style="width: 25%;">

```python
str | list[str] | Callable | None
```

</td>
<td align="left"><code>None</code></td>
<td align="left">Default file(s) to display, given as a str file path or URL, or a list of str file paths / URLs. If callable, the function will be called whenever the app loads to set the initial value of the component.</td>
</tr>

<tr>
<td align="left"><code>reps</code></td>
<td align="left" style="width: 25%;">

```python
Any | None
```

</td>
<td align="left"><code>[
    {
        "model": 0,
        "chain": "",
        "resname": "",
        "style": "sphere",
        "color": "Jmol",
        "around": 0,
        "byres": False,
        "scale": 0.3,
    },
    {
        "model": 0,
        "chain": "",
        "resname": "",
        "style": "stick",
        "color": "Jmol",
        "around": 0,
        "byres": False,
        "scale": 0.2,
    },
]</code></td>
<td align="left">None</td>
</tr>

<tr>
<td align="left"><code>config</code></td>
<td align="left" style="width: 25%;">

```python
Any | None
```

</td>
<td align="left"><code>{
    "backgroundColor": "white",
    "orthographic": False,
    "disableFog": False,
}</code></td>
<td align="left">dictionary of config options</td>
</tr>

<tr>
<td align="left"><code>confidenceLabel</code></td>
<td align="left" style="width: 25%;">

```python
str | None
```

</td>
<td align="left"><code>"pLDDT"</code></td>
<td align="left">label for confidence values stored in the bfactor column of a pdb file</td>
</tr>

<tr>
<td align="left"><code>file_count</code></td>
<td align="left" style="width: 25%;">

```python
Literal["single", "multiple", "directory"]
```

</td>
<td align="left"><code>"single"</code></td>
<td align="left">if single, allows user to upload one file. If "multiple", user uploads multiple files. If "directory", user uploads all files in selected directory. Return type will be list for each file in case of "multiple" or "directory".</td>
</tr>

<tr>
<td align="left"><code>file_types</code></td>
<td align="left" style="width: 25%;">

```python
list[str] | None
```

</td>
<td align="left"><code>None</code></td>
<td align="left">List of file extensions or types of files to be uploaded (e.g. ['image', '.json', '.mp4']). "file" allows any file to be uploaded, "image" allows only image files to be uploaded, "audio" allows only audio files to be uploaded, "video" allows only video files to be uploaded, "text" allows only text files to be uploaded.</td>
</tr>

<tr>
<td align="left"><code>type</code></td>
<td align="left" style="width: 25%;">

```python
Literal["filepath", "binary"]
```

</td>
<td align="left"><code>"filepath"</code></td>
<td align="left">Type of value to be returned by component. "file" returns a temporary file object with the same base name as the uploaded file, whose full path can be retrieved by file_obj.name, "binary" returns an bytes object.</td>
</tr>

<tr>
<td align="left"><code>label</code></td>
<td align="left" style="width: 25%;">

```python
str | None
```

</td>
<td align="left"><code>None</code></td>
<td align="left">The label for this component. Appears above the component and is also used as the header if there are a table of examples for this component. If None and used in a `gr.Interface`, the label will be the name of the parameter this component is assigned to.</td>
</tr>

<tr>
<td align="left"><code>every</code></td>
<td align="left" style="width: 25%;">

```python
Timer | float | None
```

</td>
<td align="left"><code>None</code></td>
<td align="left">Continously calls `value` to recalculate it if `value` is a function (has no effect otherwise). Can provide a Timer whose tick resets `value`, or a float that provides the regular interval for the reset Timer.</td>
</tr>

<tr>
<td align="left"><code>inputs</code></td>
<td align="left" style="width: 25%;">

```python
Component | Sequence[Component] | set[Component] | None
```

</td>
<td align="left"><code>None</code></td>
<td align="left">Components that are used as inputs to calculate `value` if `value` is a function (has no effect otherwise). `value` is recalculated any time the inputs change.</td>
</tr>

<tr>
<td align="left"><code>show_label</code></td>
<td align="left" style="width: 25%;">

```python
bool | None
```

</td>
<td align="left"><code>None</code></td>
<td align="left">if True, will display label.</td>
</tr>

<tr>
<td align="left"><code>container</code></td>
<td align="left" style="width: 25%;">

```python
bool
```

</td>
<td align="left"><code>True</code></td>
<td align="left">If True, will place the component in a container - providing some extra padding around the border.</td>
</tr>

<tr>
<td align="left"><code>scale</code></td>
<td align="left" style="width: 25%;">

```python
int | None
```

</td>
<td align="left"><code>None</code></td>
<td align="left">relative size compared to adjacent Components. For example if Components A and B are in a Row, and A has scale=2, and B has scale=1, A will be twice as wide as B. Should be an integer. scale applies in Rows, and to top-level Components in Blocks where fill_height=True.</td>
</tr>

<tr>
<td align="left"><code>min_width</code></td>
<td align="left" style="width: 25%;">

```python
int
```

</td>
<td align="left"><code>160</code></td>
<td align="left">minimum pixel width, will wrap if not sufficient screen space to satisfy this value. If a certain scale value results in this Component being narrower than min_width, the min_width parameter will be respected first.</td>
</tr>

<tr>
<td align="left"><code>height</code></td>
<td align="left" style="width: 25%;">

```python
int | float | None
```

</td>
<td align="left"><code>None</code></td>
<td align="left">The maximum height of the file component, specified in pixels if a number is passed, or in CSS units if a string is passed. If more files are uploaded than can fit in the height, a scrollbar will appear.</td>
</tr>

<tr>
<td align="left"><code>interactive</code></td>
<td align="left" style="width: 25%;">

```python
bool | None
```

</td>
<td align="left"><code>None</code></td>
<td align="left">if True, will allow users to upload a file; if False, can only be used to display files. If not provided, this is inferred based on whether the component is used as an input or output.</td>
</tr>

<tr>
<td align="left"><code>visible</code></td>
<td align="left" style="width: 25%;">

```python
bool
```

</td>
<td align="left"><code>True</code></td>
<td align="left">If False, component will be hidden.</td>
</tr>

<tr>
<td align="left"><code>elem_id</code></td>
<td align="left" style="width: 25%;">

```python
str | None
```

</td>
<td align="left"><code>None</code></td>
<td align="left">An optional string that is assigned as the id of this component in the HTML DOM. Can be used for targeting CSS styles.</td>
</tr>

<tr>
<td align="left"><code>elem_classes</code></td>
<td align="left" style="width: 25%;">

```python
list[str] | str | None
```

</td>
<td align="left"><code>None</code></td>
<td align="left">An optional list of strings that are assigned as the classes of this component in the HTML DOM. Can be used for targeting CSS styles.</td>
</tr>

<tr>
<td align="left"><code>render</code></td>
<td align="left" style="width: 25%;">

```python
bool
```

</td>
<td align="left"><code>True</code></td>
<td align="left">If False, component will not render be rendered in the Blocks context. Should be used if the intention is to assign event listeners now but render the component later.</td>
</tr>

<tr>
<td align="left"><code>key</code></td>
<td align="left" style="width: 25%;">

```python
int | str | None
```

</td>
<td align="left"><code>None</code></td>
<td align="left">if assigned, will be used to assume identity across a re-render. Components that have the same key across a re-render will have their value preserved.</td>
</tr>

<tr>
<td align="left"><code>showviewer</code></td>
<td align="left" style="width: 25%;">

```python
bool
```

</td>
<td align="left"><code>True</code></td>
<td align="left">If True, will display the 3Dmol.js viewer. If False, will not display the 3Dmol.js viewer.</td>
</tr>
</tbody></table>


### Events

| name | description |
|:-----|:------------|
| `change` | Triggered when the value of the Molecule3D changes either because of user input (e.g. a user types in a textbox) OR because of a function update (e.g. an image receives a value from the output of an event trigger). See `.input()` for a listener that is only triggered by user input. |
| `select` | Event listener for when the user selects or deselects the Molecule3D. Uses event data gradio.SelectData to carry `value` referring to the label of the Molecule3D, and `selected` to refer to state of the Molecule3D. See EventData documentation on how to use this event data |
| `clear` | This listener is triggered when the user clears the Molecule3D using the clear button for the component. |
| `upload` | This listener is triggered when the user uploads a file into the Molecule3D. |
| `delete` | This listener is triggered when the user deletes and item from the Molecule3D. Uses event data gradio.DeletedFileData to carry `value` referring to the file that was deleted as an instance of FileData. See EventData documentation on how to use this event data |



### User function

The impact on the users predict function varies depending on whether the component is used as an input or output for an event (or both).

- When used as an Input, the component only impacts the input signature of the user function.
- When used as an output, the component only impacts the return signature of the user function.

The code snippet below is accurate in cases where the component is used as both an input and an output.

- **As output:** Is passed, passes the file as a `str` or `bytes` object, or a list of `str` or list of `bytes` objects, depending on `type` and `file_count`.
- **As input:** Should return, expects a `str` filepath or URL, or a `list[str]` of filepaths/URLs.

 ```python
 def predict(
     value: bytes | str | list[bytes] | list[str] | None
 ) -> str | list[str] | None:
     return value
 ```
 
