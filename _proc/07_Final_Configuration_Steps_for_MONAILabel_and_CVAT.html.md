---
output-file: final_configuration_steps_for_monailabel_and_cvat.html
title: Copying Datasets, Adding Labels, and Configuring MONAILabel with CVAT

---



<!-- WARNING: THIS FILE WAS AUTOGENERATED! DO NOT EDIT! -->

### Step 1: Copying the Dataset

We assume that your dataset is organized into a main folder, with images stored in the `images/` subfolder. In this step, you will copy your dataset into the `datasets/` folder. For example, if your dataset is called `SurgVU`, you should copy it like this:

1. **Copy your dataset to the datasets folder**:
   ```bash
   cp -r /path/to/SurgVU datasets/images/
   ```

Make sure the `datasets/images/` folder contains all your dataset images.

### Step 2: Adding the `labels.csv` File

You need to create a `labels.csv` file that MONAILabel can read when creating a CVAT datasource. This file should contain columns for the label `name`, `id`, and `color`.

1. **Create the `labels.csv` file** in the `apps/endoscopy/` folder, and fill it with the following content:

```csv
name,id,color
Tumor_Cancer,1,#FF0000
Tumor_Sessile_Adenoma,2,#00FF00
Tumor_Polypoidal_Adenoma,3,#0000FF
Tumor_Pedunculated_Adenoma,4,#FFFF00
Sucker,5,#FF00FF
Diathermy,6,#00FFFF
Retractor,7,#800000
Needle_Holder,8,#008000
Needle,9,#000080
Suture,10,#808000
Clip,11,#800080
Scissors,12,#008080
Rectoscope,13,#FFA500
Rectal_Wall,14,#A52A2A
Rectal_Wall_Defect,15,#5F9EA0
Rectal_Fold,16,#D2691E
Rectal_Lumen,17,#FF7F50
Mucosa,18,#6495ED
Submucosa,19,#DC143C
Circular_Muscle,20,#00008B
Longitudinal_Muscle,21,#B8860B
M_Fat,22,#006400
Blood_Vessel,23,#8B0000
Fluid,24,#E9967A
Bowel,25,#8FBC8F
Blood,26,#483D8B
Anal_Rectal_Junction,27,#2F4F4F
Anal_Cavity,28,#FFD700
Reflection,29,#ADFF2F
```

This file will be referenced during the MONAILabel configuration to use your custom labels.

### Step 4: Modifying `apps/endoscopy/lib/infers/tooltracking.py` to Include Custom Labels

To make sure your custom labels are used by the tooltracking model, you need to update the labels and their corresponding colors in the `tooltracking.py` file. Here's how you can override the labels:

```python
# Override Labels
self.labels = {
    "Tumor_Cancer": 1,
    "Tumor_Sessile_Adenoma": 2,
    "Tumor_Polypoidal_Adenoma": 3,
    "Tumor_Pedunculated_Adenoma": 4,
    "Sucker": 5,
    "Diathermy": 6,
    "Retractor": 7,
    "Needle_Holder": 8,
    "Needle": 9,
    "Suture": 10,
    "Clip": 11,
    "Scissors": 12,
    "Rectoscope": 13,
    "Rectal_Wall": 14,
    "Rectal_Wall_Defect": 15,
    "Rectal_Fold": 16,
    "Rectal_Lumen": 17,
    "Mucosa": 18,
    "Submucosa": 19,
    "Circular_Muscle": 20,
    "Longitudinal_Muscle": 21,
    "M_Fat": 22,
    "Blood_Vessel": 23,
    "Fluid": 24,
    "Bowel": 25,
    "Blood": 26,
    "Anal_Rectal_Junction": 27,
    "Anal_Cavity": 28,
    "Reflection": 29
}

self.label_colors = {
    "Tumor_Cancer": (255, 0, 0),
    "Tumor_Sessile_Adenoma": (0, 255, 0),
    "Tumor_Polypoidal_Adenoma": (0, 0, 255),
    "Tumor_Pedunculated_Adenoma": (255, 255, 0),
    "Sucker": (255, 0, 255),
    "Diathermy": (0, 255, 255),
    "Retractor": (128, 0, 0),
    "Needle_Holder": (0, 128, 0),
    "Needle": (0, 0, 128),
    "Suture": (128, 128, 0),
    "Clip": (128, 0, 128),
    "Scissors": (0, 128, 128),
    "Rectoscope": (255, 165, 0),
    "Rectal_Wall": (165, 42, 42),
    "Rectal_Wall_Defect": (95, 158, 160),
    "Rectal_Fold": (210, 105, 30),
    "Rectal_Lumen": (255, 127, 80),
    "Mucosa": (100, 149, 237),
    "Submucosa": (220, 20, 60),
    "Circular_Muscle": (0, 0, 139),
    "Longitudinal_Muscle": (184, 134, 11),
    "M_Fat": (0, 100, 0),
    "Blood_Vessel": (139, 0, 0),
    "Fluid": (233, 150, 122),
    "Bowel": (143, 188, 143),
    "Blood": (72, 61, 139),
    "Anal_Rectal_Junction": (47, 79, 79),
    "Anal_Cavity": (255, 215, 0),
    "Reflection": (173, 255, 47)
}
```

### Step 5: Creating a Helper Script to Run MONAILabel with Custom Labels

You can create a simple shell script to run MONAILabel with your custom labels and CVAT integration in one command. Save the following script as `run_monailabel.sh`:

```bash
#!/bin/bash

# Copy config file with custom labels
cp apps/endoscopy/cvat_plugin/tooltracking.yaml /usr/local/monailabel/plugins/cvat/endoscopy/tooltracking.yaml
 
# Set environment variables
export MONAI_LABEL_DATASTORE=cvat
export MONAI_LABEL_DATASTORE_URL=http://127.0.0.1:8080
export MONAI_LABEL_DATASTORE_USERNAME=<username>
export MONAI_LABEL_DATASTORE_PASSWORD=<password>

# Start MONAILabel server with the specified configurations
monailabel start_server \
  --app apps/endoscopy \
  --studies datasets/images \
  --conf models tooltracking \
  --conf epistemic_enabled true \
  --conf epistemic_top_k 100 \
  --conf cvat_segment_size 100 \
  --conf cvat_labels_file apps/endoscopy/surgvu_labels.csv
```

Make sure to give execute permission to the script:
```bash
chmod +x run_monailabel.sh
```

Now you can run MONAILabel with all configurations using this single command:
```bash
./run_monailabel.sh
```

