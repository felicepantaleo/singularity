# Building

Build with:

```
sudo singularity build fireworks_el8.sif fireworks_el8_recipe
```

Make sure cvmfs is working and `cms.cern.ch` is mounted
Execute:
```
singularity shell  -B /run:/run -B /cvmfs -B /tmp/.X11-unix  fireworks_el8.sif
```

Then:
```
bash
source /cvmfs/cms.cern.ch/cmsset_default.sh
scram l
```
