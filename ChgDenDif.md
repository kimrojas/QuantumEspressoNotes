# Charge density difference in quantum espresso

## 1. scf and nscf calculation

## 2. Charge density calculation (for system A, system B, and system AB)

*in.pp.A*
```
&INPUTPP
prefix = 'A'
outdir = './tmp-A'
filplot='PP-A.dat'
plot_num=0
/
```

*in.pp.B*
```
&INPUTPP
prefix = 'B'
outdir = './tmp-B'
filplot='PP-B.dat'
plot_num=0
/
```

*in.pp.AB*
```
&INPUTPP
prefix = 'AB'
outdir = './tmp-AB'
filplot='PP-AB.dat'
plot_num=0
/
```

|NOTE:|  | |
| ---    |      ---         |  ---                    |
|     | plot_num = 0 |:: electron (pseudo-)charge density  |
|     | plot_num = 17 |:: all-electron valence charge density (PAW POTENTIAL REQUIRED)  |
|     | plot_num = 21 |:: all-electron charge density (valence+core) (PAW POTENIAL REQUIRED)|


|RUN: | |
| ---| ---|
||mpirun pp.x -in in.pp.A > out.pp.A|
||mpirun pp.x -in in.pp.B > out.pp.B|
||mpirun pp.x -in in.pp.AB > out.pp.AB|

## 3. Charge density difference calculation


*in.pp.dif*
```
 &INPUTPP
 /

 &PLOT
   nfile=3,
   filepp(1)  = 'PP_0-AB.dat',
   filepp(2)  = 'PP_0-A.dat',
   filepp(3)  = 'PP_0-B.dat',
   weight(1)  =  1.0,
   weight(2)  = -1.0,
   weight(3)  = -1.0,
   iflag      =  3
   output_format  = 6
   fileout  = 'difference.cube'
 /
```

NOTE:   (a) difference.cube can be visualized by VESTA  
        (b) change output_format = 5 and fileout = 'difference.xsf' if you want to use xcrysden (HINT: SLOWER interface)  
        (c) default nfile max = 7 so if you need more, increase 'nfilemax' in ~/QEdir/PP/src/chden_module.f90 and recompile quantum espresso
