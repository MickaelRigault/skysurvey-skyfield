# skysurvey-skyfield
Skysurvey extension containing 3D density-velocity fields for accurate 3D-sky distribution

# Installation
using pip:
```bash
pip install skysurvey-skyfield
```

# Concept 
skysurvey-skyfield top level functions are expected to returns RA, Dec, zcosmo, vpec etc. that can be then be used in a skysurvey target model.

```python
import skysurvey
import skysurvey_skyfield

model_3d = {"radecz": {"func": func_returning___ra_dec_z_vpec,
                      #"kwargs": {}, # function's options
                       "as":["ra", "dec", "z", "vpec"] # stroring names
					   }
          }
snia = skysurvey.SNIa.from_draw(5_000, model=model_3d)
```


## Example, 

the fully random function (dumb since ra, dec and zcosmo
are not connected. These are what is happening by default in skysurvey
for ra,dec and redshift.

```python
def get_random_3d(size, rate, 
                  zmin=0, zmax=1, zstep=1e-4, # redshift
                  skyarea=None, ra_range=[0, 360], dec_range=[-90, 90], # radec
                  vpec_prop = {"loc": 0, "scale":300},
                  **kwargs
                 ):
    """ dumb function calling ra, dec, z and vpec independently and returning them all at once. """
    from skysurvey.tools.utils import random_radec
    from skysurvey.target import rates
    
	# random draw in the sky
    ra, dec = random_radec(size=size, skyarea=skyarea, ra_range=ra_range, dec_range=dec_range)

	# redshift follow the rate
    zcosmo = rates.draw_redshift(size, rate, zmin=zmin, zmax=zmax, zstep=zstep, skyarea=skyarea, **kwargs)
	
	# random vpec
    vpec = np.random.normal(size=size, **vpec_prop)
    return ra, dec, zcosmo, vpec
	
# build the model
model_3d = {"radecz": {"func": get_random_3d,
                      #"kwargs": {}, # function's options
                       "as":["ra", "dec", "z", "vpec"] # stroring names
					   }
          }
snia = skysurvey.SNIa.from_draw(5_000, model=model_3d)
```
