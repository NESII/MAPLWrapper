# MAPLWrapper

A wrapper to make MAPL components NUOPC-compliant.

# Implementation

The wrapper is a NUOPC Model component that encapsulates a MAPL\_Cap
object which contains the entire MAPL hierarchy with three children:
History, ExtData, and a root component provided by the user, which may
have its own children. By wrapping at the
cap level MAPL retains all of its functionality and has no knowledge
of the NUOPC environment in which it is run.

The NUOPC wrapper merely acts as a container for the MAPL\_CAP and
maps its initilization and run routines onto the cap by querying it
for necessary information such as fields, grids, states, imports,
exports, etc, or by performing actions on it such as stepping the model.

The  advantage to this approach is its simplicity and that no changes are
needed at the MAPL level. The downside is that the wrapper is fairly
heavy and that it is not suitable for wrapping many individual
components since each instance of the wraper contains an entire MAPL
hierarchy with History and ExtData.

Therefore, this wrapper is best suited for wrapping entire MAPL
models or large pieces of it.

# Obtaining and Building

The wrapper requires MAPL and NUOPC to build and is included within
the latest version of MAPL included in the GEOS develop branch.

MAPL is currently only distributed with GEOS and is not
publicly available at this time.

Contact kyle.gerheiser@nasa.gov if
you would like to obtain a copy.

# Usage

To use the wrapper add a component to a NUOPC driver by calling
NUOPC\_DriverAddComp using the wrapper SetServices and then call
init\_wrapper on the returned ESMF\_GridComp with a name, the root
MAPL SetServices, and the CAP.rc for the MAPL instance.

```
call NUOPC\_DriverAddComp(driver, "agcm", wrapper\_ss, comp = agcm,
petlist = agcm\_petlist, rc = rc)

call init\_wrapper(wrapper\_gc = agcm, name = "agcm", cap\_rc\_file =
"AGCM\_CAP.rc", root\_set\_services = gcs\_set\_services, rc = rc)
```


# Limitations and Future Work

The wrapper currently only supports a fairly limited number of NUOPC
features and some options are hardcoded. For example, there is no
option to enable field mirroring within the wrapper, and
TransferActionGeomObject is always on.
However, it is well suited for simply running a
component and connecting fields.

In the future the wrapper could be made more generic and support for more NUOPC options could be toggled.




