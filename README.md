# gaustudio2

vast ai

new setup
nvidia template (cuda installed ) + jupyter terminal

___________________________________________
Gaustudio nvidia template

Step-by-Step Setup Guide for GauStudio
✅ 1. System & Environment Check
On your Vast.ai terminal, confirm GPU, CUDA, and Python:
```
nvidia-smi
nvcc --version
python3 --version
```
Expected:
CUDA 11.3–12.x ✅
Python ≥ 3.8 ✅
GPU visible ✅


2. Create a Conda Environment

(Highly recommended — isolates dependencies)

conda create -n gaustudio python=3.8 -y
conda activate gaustudio


🔥 3. Install PyTorch (CUDA Compatible)

Since your GPU supports CUDA 12.9, install PyTorch built for CUDA 12.x:

pip install torch torchvision --index-url https://download.pytorch.org/whl/cu121

(If CUDA 12.9 build is unavailable, the CUDA 12.1 wheel works fine — it’s backward-compatible.)


4. Clone the Repository
git clone https://github.com/GAP-LAB-CUHK-SZ/gaustudio.git
cd gaustudio

____________________________________________________

📜 5. Install Dependencies
pip install -r requirements.txt


You’ll see packages like:

cv2
tqdm
vdbfusion
trimesh
omegaconf
plyfile
open3d
scipy
skimage
ninja


packages = [
    "cv2",
    "tqdm",
    "vdbfusion",
    "trimesh",
    "omegaconf",
    "plyfile",
    "open3d",
    "scipy",
    "skimage",
    "ninja"
]

for pkg in packages:
    try:
        __import__(pkg)
        print(f"[✅] {pkg} is installed")
    except ImportError:
        print(f"[❌] {pkg} is NOT installed")
----


python - <<'EOF'
packages = ["cv2","tqdm","vdbfusion","trimesh","omegaconf","plyfile","open3d","scipy","skimage","ninja"]
for p in packages:
    try:
        __import__(p)
        print(f"[✅] {p} OK")
    except ImportError:
        print(f"[❌] {p} missing")
EOF


-----
if anything missing : then,

pip install vdbfusion scikit-image ninja



______________________________________________________

6. Install the Custom Rasterizer & Gaustudio
cd submodules/gaustudio-diff-gaussian-rasterization
python setup.py install
cd ../../
python setup.py develop


This step builds their C++/CUDA rasterizer — you must see something like:

running install
building 'diff_gaussian_rasterization' extension


✅ If this succeeds, you’re ready.
-----------------------------


need to upload folder files .
zip it

and upload to google drive. share link. ( with access to all)

install this for google drive:
pip install gdown


this doesnt work it will download 2.7kb files not MB files.
this link doesnt work.. needd a '?' format
gdown https://drive.google.com/file/d/1glnolbFVhVvdnCSChB6wyrABCEIiDJ79/view?usp=drive_link

use this link to generate direct download link
https://sites.google.com/site/gdocs2direct/?utm_source=chatgpt.com

need like this one,
gdown https://drive.google.com/uc?export=download&id=1MCNKAkZpuyPnDX4EgFRiA4QowtxUfJQC
--


this worked>>

cd /workspace
gdown --fuzzy "https://drive.google.com/file/d/1MCNKAkZpuyPnDX4EgFRiA4QowtxUfJQC/view?usp=drive_link" -O my_cleaned_pointcloud.zip




----


then unzip it

unzip 0fae8293-6b_only30k.zip -d myfolder





------------------------
7. Prepare Input Data

You need a directory like this:

my_data/
 ├── cameras.json
 └── point_cloud/
      └── iteration_30000/
          └── point_cloud.ply


If you only have point_cloud.ply, do this quick mock structure:

mkdir -p my_data/point_cloud/iteration_30000
mv your_file.ply my_data/point_cloud/iteration_30000/point_cloud.ply
touch my_data/cameras.json


You can use a dummy cameras.json file if you just want to test mesh extraction, but for real outputs, use one from 3DGS output folders.


8. Run Mesh Extraction
gs-extract-mesh -m ./my_data -o ./output/my_mesh


After this runs successfully, your output folder will look like:

output/my_mesh/
 ├── fused_mesh.ply
 ├── textured_mesh/
 └── logs/


----
if you getting issue that. camera.json is not in the correct path, then copy and paste it to required path.

cp /workspace/my_cleaned_pointcloud/0fae8293-6b_only30k/cameras.json /workspace/my_cleaned_pointcloud/








--------------------------------------------------------------------------
🎨 9. (Optional) Bind Texture Using MVS-Texturing

If you want to texture the mesh:

Compile mvs-texturing

Add build/bin to your $PATH

Then:

cd output/my_mesh
texrecon ./images ./fused_mesh.ply ./textured_mesh --outlier_removal=gauss_clamping --data_term=area --no_intermediate_results

✅ 10. Confirm Everything Works

Try:

python -c "import torch; import open3d; print('Setup OK!')"


You should see no errors and your mesh will be saved in .ply or .obj format.





