# libROUNDSchemes: An OpenFOAM Library of High-resolution Structure-preserving Convection Schemes
[![DOI](https://zenodo.org/badge/570191772.svg)](https://zenodo.org/badge/latestdoi/570191772)

libROUNDSchemes is an open-source library for OpenFOAM. The library implements the ROUND schemes proposed by [1] into OpenFOAM. The library can significantly reduce numerical errors with a minor increased CPU cost. Moreover, the library can offer an improved structure-preserving property that gives essentially oscillation-free solutions and preserves the structures of the passive transported scalar.

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

If you use our library, please cite the publications describing its theory [1] and implementation [2].
- [1] Deng, X., 2023. A Unified Framework for Non-linear Reconstruction Schemes in a Compact Stencil. Part 1: Beyond Second Order. *Journal of Computational Physics*, p.112052.
- [2] Deng, X., 2023. A new open-source library based on novel high-resolution structure-preserving convection schemes. *Journal of Computational Science*, p.102150.

## Validation and Verification
### 1. Accuracy test
The figure below presents the variation of numerical errors with CPU cost. Compared with the conventional schemes, the ROUND-A, ROUND-A+ and ROUND-L schemes significantly reduce numerical errors with similar CPU time.
![accuracyRound](https://github.com/advanCFD/libROUNDSchemes/assets/118991833/ba2d8df4-e51a-4e06-9cfd-d2d905b5a0c7)

### 2. Rotation of complex profiles
Rotation of complex profiles by solving scalar transport equation. As shown in the figure below, the ROUND scheme produces low-dissipative results and preserves the structure of the passive transported scalar.
![RotationRound](https://github.com/advanCFD/libROUNDSchemes/assets/118991833/21a3d9f5-485d-4deb-a453-ffcd51e07f66)

### 3. Convection-diffusion problems of a passively transported scalar
The convection-diffusion problem is solved here. The Peclet number is 14000. The central scheme solves the diffusion term. We use different convection schemes to discretize the convection term. The results are included below. The classic TVD schemes distort the passively transported profile. The ROUND scheme produces the most accurate result. The details of this case are in Deng, X., Massey, J.C. and Swaminathan, N., 2023. Large-eddy simulation of bluff-body stabilized premixed flames with low-dissipative, structure-preserving convection schemes. *AIP Advances*, 13(5).
![convectionDiffusion](https://github.com/advanCFD/libROUNDSchemes/assets/118991833/1988b702-d589-4523-9eb6-27f6417cf022)

### 4. Mach 3 wind tunnel with a forward step
Instantaneous temperature contours of Mach 3 wind tunnel with a forward step are presented below. The mesh size is 1/160. Compared with conventional schemes, ROUND schemes are able to resolve small-scale vortices with high resolution.
![Mach3ForwardStep](https://github.com/advanCFD/libROUNDSchemes/assets/118991833/b27a8245-8bba-4412-b9f2-57e318f0df97)

### 5. Supersonic and hypersonic flows around a circular cylinder
Schlieren images of the density field for the supersonic and hypersonic flows over a circular cylinder are presented below. The numerical solutions demonstrate the high-resolution and shock-capturing capability of the ROUND schemes.
![Ma3Ma5](https://github.com/advanCFD/libROUNDSchemes/assets/118991833/bdb9eebe-6aef-4fb1-8e6a-2fb3bebe1662)

### 6. Impingement of a supersonic jet on a cone mounted on a flat plate
A challenging case is presented here to demonstrate the accuracy and robustness of ROUND schemes.
![impingement](https://github.com/advanCFD/libROUNDSchemes/assets/118991833/b8a7af05-9b1e-48c6-a5ee-9031b8cc980d)

### 7. Large-eddy simulation of bluff-body stabilized premixed flames
ROUND schemes are tested for LES of bluff-body stabilized premixed flames. The ROUND schemes improve the numerical resolution. Furthermore, ROUND schemes can better preserve the time-averaged field's statistical axis-symmetry compared with conventional schemes. The details of this simulation are in Deng, X., Massey, J.C. and Swaminathan, N., 2023. Large-eddy simulation of bluff-body stabilized premixed flames with low-dissipative, structure-preserving convection schemes. *AIP Advances*, 13(5).
![AIPROUND3](https://github.com/advanCFD/libROUNDSchemes/assets/118991833/22246d4a-7838-40be-9499-f841108444c1)

### 8. Comparison with high-order schemes
The work(Yang, M. and Li, S., 2023. An efficient implementation of compact third-order implicit reconstruction solver with a simple WBAP limiter for compressible flows on unstructured meshes. Engineering Applications of Computational Fluid Mechanics, 17(1), p.2249135.) shows that the ROUND scheme outperforms high-order schemes in some cases such as the isentropic vortex advection test and the two blast wave interaction problem.
![isentropic vortex](https://github.com/advanCFD/libROUNDSchemes/assets/118991833/da4c6ff4-7c5a-4220-b18b-3f986e3cff1d)
![two blast wave](https://github.com/advanCFD/libROUNDSchemes/assets/118991833/149c2180-c7b7-422c-bb51-b2f9a87c7715)

