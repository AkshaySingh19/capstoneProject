AUTHORIZATION RECEIVED: A + B
=== PHASE 1 DATASET DOWNLOAD AUTHORIZATION (A) ===
Authorized: Download agreement submission for LRS2/LRS3, VoxCeleb2, FakeAVCeleb.
Open-access (no agreement): DeepfakeTIMIT.
Action required by user (manual, requires login/agreement submission):
1. Visit https://www.robots.ox.ac.uk/~vgg/data/lip_reading/lrs2.html → register/agreement → download
2. Visit https://www.robots.ox.ac.uk/~vgg/data/voxceleb/ → agreement → download VoxCeleb2
3. Visit https://github.com/hasanirtaza/FakeAVCeleb → request/access agreement → download
4. DeepfakeTIMIT: git clone / download from https://github.com/albahnasawi/DeepfakeTIMIT (open)
Status: Authorized; execution requires user to submit agreements and run downloads (cannot be automated fully due to agreement/auth requirements).

=== B (Repo Clone Manual Execution) ===
Authorized: Clone/setup open-source repos (no agreement needed).
Commands (manual/network-independent):
1. Wav2Lip:
   git clone https://github.com/wilderwj/Wav2Lip.git
   cd Wav2Lip
   # Download checkpoint: check repo for wav2lip_gan.pth link
   pip install -r requirements.txt
2. SadTalker:
   git clone https://github.com/OpenTalker/SadTalker.git
   cd SadTalker
   pip install -r requirements.txt
   # Download weights per repo README
Status: Commands documented; previous automated clone blocked by network/restriction. Manual execution available when user confirms or runs locally.
