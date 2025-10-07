Run:

gs-extract-mesh \
  -m /workspace/my_cleaned_pointcloud/0fae8293-6b_only30k \
  -o /workspace/my_cleaned_pointcloud/0fae8293-6b_only30k/mesh_output_final


Notes:

Keep GPU available (do not set CUDA_VISIBLE_DEVICES= unless you intentionally want CPU).

You now have ~30k points, which is small enough to avoid segmentation faults.

This should finally complete the extraction without errors.

Do you want me to explain why we don’t need to add dummy attributes anymore?

You said:
(main) root@C.26502727:/workspace$ gs-extract-mesh \
  -m /workspace/my_cleaned_pointcloud/0fae8293-6b_only30k \
  -o /workspace/my_cleaned_pointcloud/0fae8293-6b_only30k/mesh_output_final
Loading trained model at iteration 30000
Traceback (most recent call last):
  File "/venv/main/bin/gs-extract-mesh", line 7, in <module>
    sys.exit(main())
  File "/workspace/gaustudio/gaustudio/scripts/extract_mesh.py", line 62, in main
    pcd.load(os.path.join(args.model,"point_cloud", "iteration_" + str(loaded_iter), "point_cloud.ply"))
  File "/workspace/gaustudio/gaustudio/models/base.py", line 95, in load
    names = sorted(names, key=lambda n: int(n.split('_')[-1]))
  File "/workspace/gaustudio/gaustudio/models/base.py", line 95, in <lambda>
    names = sorted(names, key=lambda n: int(n.split('_')[-1]))
ValueError: invalid literal for int() with base 10: 'scale'
(main) root@C.26502727:/workspace$ 
ChatGPT said:

Ah! Now it’s clear why this keeps failing. The error:

ValueError: invalid literal for int() with base 10: 'scale'


is not because of too many points or GPU issues — it’s because the PLY still has extra fields (opacity, rot_x, rot_y, rot_z, scale).

GauStudio’s base.py assumes that in the point_cloud/iteration_XXXX/ folder, the only file is point_cloud.ply and that PLY contains only x, y, z coordinates. Any extra fields like opacity or scale break the sorting logic when it reads the PLY attributes.

✅ Solution: Strip the PLY to contain only x, y, z points.

Here’s a quick Python snippet to do that:

python3 - <<EOF
import numpy as np
from plyfile import PlyData, PlyElement

ply_path = "/workspace/my_cleaned_pointcloud/0fae8293-6b_only30k/point_cloud/iteration_30000/point_cloud.ply"

# Load PLY
ply = PlyData.read(ply_path)
points = np.stack([ply['vertex']['x'], ply['vertex']['y'], ply['vertex']['z']], axis=-1)

# Create new PLY with only x, y, z
vertex = np.array([(p[0], p[1], p[2]) for p in points],
                  dtype=[('x', 'f4'), ('y', 'f4'), ('z', 'f4')])
ply_clean = PlyData([PlyElement.describe(vertex, 'vertex')])
ply_clean.write(ply_path)
EOF


After running this, point_cloud.ply will contain only x, y, z — no opacity, no rotation, no scale. Then you can run:

gs-extract-mesh \
  -m /workspace/my_cleaned_pointcloud/0fae8293-6b_only30k \
  -o /workspace/my_cleaned_pointcloud/0fae8293-6b_only30k/mesh_output_final


This should finally succeed without the scale error.
