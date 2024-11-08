# skysurvey-skyfield
Skysurvey extension containing 3D density-velocity fields for accurate 3D-sky distribution

# Installation
using pip:
```bash
pip install skysurvey-skyfield
```


# Concept 
skysurvey-skyfield top level functions are expected to returns RA, Dec, zcosmo, vpec etc. that can be then be used in a skysurvey target model.

For instance:
```python
import skysurvey
import skysurvey_skyfield

model_3d = {"radecz": {"func": skysurvey_skyfield.get_random_3dsky,
                      #"kwargs": {},
                       "as":["ra", "dec", "z", "vpec"]}
          }
snia = skysurvey.SNIa.from_draw(5_000, model=model_3d)
```
