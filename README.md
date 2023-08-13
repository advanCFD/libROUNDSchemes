# libROUNDSchemes: An OpenFOAM Library of High-resolution Structure-preserving Convection Schemes

libROUNDSchemes is an open-source library for OpenFOAM. It implements ROUND schemes proposed in [1] into unstructured grids based OpenFOAM. The library can significantly reduce numerical errors with a minor increased CPU cost. Moreover, the library can offer an improved structure-preserving property that gives essentially oscillation-free solutions and preserves the structures of the passive transported scalar.

## Compilation

The libROUNDSchemes library does not require any third-party dependency.
After sourcing the OpenFOAM version, simply execute:

```
./Allwmake
```
## Usage

The library can work with any OpenFOAM solvers containing convection terms.

* The library should be linked to the solver. Add the following to your system/controlDict file:

```
libs
(
    "libROUNDSchemes.so" 
);
```

* For a general scalar field alpha, select a ROUND scheme such as ROUNDAplus in system/fvSchemes as:

```
divSchemes
{
   div(phi,alpha)      Gauss ROUNDAplus;
}
```

* For a strictly bounded scalar field such as mass fraction Y, select a ROUND scheme such as ROUNDAplus in system/fvSchemes as:

```
divSchemes
{
   div(phi,Y)      Gauss ROUNDAplus01;
}
```

* For a density-based solver, select a ROUND scheme such as ROUNDAplus in system/fvSchemes as:

```
interpolationSchemes
{
   reconstruct(rho) ROUNDAplus;
}
```
* When the mesh is irregular, adding a limiter to the gradient scheme is required to suppress numerical oscillations:

```
gradSchemes
{
    default         cellLimited Gauss linear 1.0;
}
```

## Tutorials
The benchmark tests of 2D complex wave convection and Mach 3 forward step are available in _tutorial_ directory.

## Citation

If you use our library, please cite the publications describing its theory and implementation [1].
- [1] Deng, X., 2023. A Unified Framework for Non-linear Reconstruction Schemes in a Compact Stencil. Part 1: Beyond Second Order. *Journal of Computational Physics*, p.112052.

## Validation and Verification
### 1. Accuracy test
The figure below presents the variation of numerical errors with CPU cost. Compared with the conventional schemes, the ROUND-A, ROUND-A+ and ROUND-L schemes significantly reduce numerical errors with a minor increased CPU time.
![accuracyRound](https://github.com/advanCFD/libROUNDSchemes/assets/118991833/ba2d8df4-e51a-4e06-9cfd-d2d905b5a0c7)

### 2. Rotation of complex profiles
Rotation of complex profiles by solving scalar transport equation. As shown in the figure below, the ROUND scheme produces low-dissipative results and preserves the structure of the passive transported scalar.
![RotationRound](https://github.com/advanCFD/libROUNDSchemes/assets/118991833/21a3d9f5-485d-4deb-a453-ffcd51e07f66)

### 3. Supersonic and hypersonic flows around a circular cylinder
Schlieren images of the density field for the supersonic and hypersonic flows over a circular cylinder are presented below. The numerical solutions demonstrate the high-resolution and shock-capturing capability of the ROUND schemes.
![Ma3Ma5](https://github.com/advanCFD/libROUNDSchemes/assets/118991833/bdb9eebe-6aef-4fb1-8e6a-2fb3bebe1662)



