# Exporting data to NWB

## Description

The `export/nwb.py` module calls [DLC2NWB](https://github.com/DeepLabCut/DLC2NWB/) to
save output generated by Element DeepLabCut as NWB files. 
The main function, `dlc_session_to_nwb`, contains a flag to control calling a parallel 
function in [Element Session](https://github.com/datajoint/element-session/blob/main/element_session/export/nwb.py).

As DLC2NWB does not currently offer a separate function for generating `PoseEstimation`
objects (see [ndx-pose](https://github.com/rly/ndx-pose)), the current solution is to
allow DLC2NWB to write to disk, and optionally rewrite this file using metadata provided
by the export function in Element Session.

## Usage

Before using, please install [DLC2NWB](https://github.com/DeepLabCut/DLC2NWB/)

```bash
pip install dlc2nwb
```

Then, call the export function using keys from the `PoseEstimation` table.

```python
from element_deeplabcut import model
from element_session import session
from element_deeplabcut.export import dlc_session_to_nwb

session_key = (session.Session & CONDITION)
pose_key = (model.PoseEstimation & session_key).fetch1('KEY')
dlc_session_to_nwb(pose_key, use_element_session=True, session_kwargs=SESSION_KWARGS)
```


Here, `CONDITION` should uniquely identify a session and `SESSION_KWARGS` can be any of
the items described in the docstring of `element_session.export.nwb.session_to_nwb`
as a dictionary.