
# Electrostatic potentials

The NWChem Electrostatic Potential (ESP) module derives partial atomic
charges that fit the quantum mechanical electrostatic potential on
selected grid points.

The ESP module is specified by the NWChem task directive

`task esp`

The input for the module is taken from the ESP input block
```
ESP  
  ... 
END
```
## Grid specification

The grid points for which the quantum mechanical electrostatic potential
is evaluated and used in the fitting procedure of the partial atomic
charges all lie outside the van der Waals radius of the atoms and within
a cutoff distance from the atomic centers. The following input
parameters determine the selection of grid points.

  - If a grid file is found, the grid will be read from that file. If no
    grid file is found, or the keyword

`       recalculate`  
  
is given, the grid and the electrostatic potential is recalculated.`

  - The extent of the grid is determined by

`       range <real rcut>` 
  
where rcut is the maximum distance in nm between a grid point and any of the atomic centers. 
When omitted, a default value for rcut of 0.3 nm is used.

  - The grid spacing is specified by

`       spacing <real spac>  `
  
where spac is the grid spacing in <img alt="$nm$" src="https://raw.githubusercontent.com/wiki/nwchemgit/nwchem/svgs/55e64309e6bad09453ebdfb3c7254f1f.svg?invert_in_darkmode&sanitize=true" align=middle width="24.209295pt" height="14.10255pt"/> for the regularly spaced grid points.
If not specified, a default spacing of 0.05 nm is used.

  - The van der Waals radius of an element can be specified by

`       radius <integer iatnum> <real atrad>`  
  
where iatnum is the atomic number for which a van der Waals radius of atrad in nm will be used in the grid point determination.  
Default values will be used for atoms not specified.

  - The probe radius in nm determining the envelope around the molecule
    is specified by

`       probe <real probe default 0.07>`

  - The distance between atomic center and probe center can be
    multiplied by a constant factor specified
by

`       factor <real factor default 1.0>`  
  
All grid points are discarded that lie within a distance factor*(radius(i)+probe) from any atom **i**.

  - Schwarz screening is applied using

`      screen [<real scrtol default 1.0D-5>]`

## Constraints

Additional constraints to the partial atomic charges can be imposed
during the fitting procedure.

  - The net charge of a subset of atoms can be constrained
using

`       constrain <real charge {<integer iatom>}`  
  
where charge is the net charge of the set of atoms {iatom}. A negative atom number iatom can be  
used to specify that the partial charge of that atom is substracted in the sum for the set.

  - The net charge of a sequence of atoms can be constrained using
```
       constrain <real charge> <integer iatom> through <integer jatom> 
```  
where charge is the net charge of the set of atoms {[iatom:jatom]}.

  - A group of atoms can be constrained to have the same charge with

`       constrain equal {<integer iatom>}`

  - The individual charge of a group of atoms can be constrained to be equal to those of a second group of atoms
with
```
      constrain group  <integer iatom> <integer jatom>  to  <integer katom> <integer latom>
```  
resulting in the same charge for atoms iatom and katom, for atoms iatom+1 and k atom+1, ... for atoms jatom and latom.

  - A special constraint

`       constrain xhn  <integer iatom> {<integer jatom>}`  
  
can be used to constrain the set {iatom,{jatom}} to zero charge, and constrain all atoms  in {jatom} to have the same charge. This can be used, for example, to restrain a methyl    group to zero charge, and have all hydrogen carrying identical charges.

## Restraints

Restraints can be applied to each partial charge using the RESP charge
fitting procedure.

  - The directive for charge restraining is
```
      restrain [hfree] (harmonic [<real scale>] | \ 
      hyperbolic [<real scale> [<real tight>]]  \  
       [maxiter <integer maxit>]  [tolerance <real toler>])
```
Here hfree can be specified to exclude hydrogen atoms from the
restaining procedure. Variable scale is the strength of the restraint
potential, with a default of 0.005 au for the harmonic restraint and a
default value of 0.001 au for the hyperbolic restraint. For the
hyperbolic restraints the tightness tight can be specified to change the
default value of 0.1 e. The iteration count that needs to be carried out
for the hyperbolic restraint is determined by the maximum number of
allowed iterations maxiter, with a default value of 25, and the
tolerance in the convergence of the partial charges toler, with a
default of 0.001 e.

## Plotting

The electrostatic potential computed on a grid can be displayed using
the gOpenMol [1](http://www.csc.fi/english/pages/g0penMol) code.

A necessary step requires to convert the .plt file computed by the ESP
module from human readable to machine readable format; this can be
achieved by using the gOpenMol Pltfile converter.

In practice, you would need to follow the following steps

1.  rename *filename.plt* to *filename.plt.txt*
2.  start gOpenMol
3.  open the **<ins>R</ins>un** menu
4.  select the **Pltfile (conversion)** utility
5.  choose *filename.plt.txt* as input file name and *filename.plt* as
    the output file name
6.  select **Formatted ==\> Unformatted**
7.  read the *filename.xyz* file by going to: **File -\> Import -\>
    Coord -\> Browse -\> Apply**; you should see your system in the main
    graphical window as connections between atoms
8.  plot the density by going to **Plot -\> Contour**
9.  first click **<ins>B</ins>rowse** and read the gOpenMol filename.plt
    file:
10. then click **Import**
11. define desired **Contour level**
12. click **<ins>A</ins>pply**

More details at:
<http://nms.kcl.ac.uk/lev.kantorovitch/codes/lev00/node71.html>