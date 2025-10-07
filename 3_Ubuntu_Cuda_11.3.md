GAUSTUDIO ---3 

nvidia  4090 

-----------------------------------
gitbash
#create a SSH key for the account
ssh-keygen -t rsa -b 4096 -C naveenmcv.temp@gmail.com

# to get the ssh key from my local path 
cat ~/.ssh/id_rsa.pub

# vast.ai > Keys > add > & paste it here.

# then rent a instance
# then connect it,

------------------------------------------------------------------


# Create a new conda environment
conda create -n gaustudio python=3.8
# Activate the conda environment
conda activate gaustudio


----------------------------------

# Example command to install PyTorch version 1.12.1+cu113
conda install pytorch=1.12.1 torchvision=0.13.1 cudatoolkit=11.3 -c pytorch

# Example command to install PyTorch version 2.0.1+cu118
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu118
-----------------------------------------

# 1️⃣ Clone the GauStudio repo from GitHub
git clone https://github.com/GAP-LAB-CUHK-SZ/gaustudio.git

# 2️⃣ Change directory into the cloned repo
cd gaustudio


-------------
ls
>>
(gaustudio) root@C.26531104:/workspace/gaustudio$ ls
LICENSE  README.md  assets  docs  gaustudio  pyproject.toml  requirements.txt  setup.py  submodules
(gaustudio) root@C.26531104:/workspace/gaustudio$ 

-----------
pip install -r requirements.txt

-----------
cd submodules/gaustudio-diff-gaussian-rasterization
python setup.py install
cd ../../
python setup.py develop

-----------
pip install gdown
gdown https://drive.google.com/uc?id=1MCNKAkZpuyPnDX4EgFRiA4QowtxUfJQC

-------
unzip 0fae8293-6b_only30k.zip -d /workspace/my_project

-----------
pip install opencv-python
--
pip install tqdm trimesh omegaconf plyfile open3d scipy scikit-image

-----

gs-extract-mesh -m /workspace/my_project/0fae8293-6b_only30k -o /workspace/my_project/0fae8293-6b_only30k/mesh_output
















