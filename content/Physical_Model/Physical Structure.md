# 🌌 Physical Structure of Protoplanetary Disks

> **Core idea:** A protoplanetary disk is a rotating, flattened structure of gas and dust surrounding a young star.  
> Its **radial structure** is primarily controlled by gravity and rotation, while its **vertical structure** is controlled by the balance between stellar gravity and gas pressure.

---

## 1. Overview

A protoplanetary disk can be described in cylindrical coordinates

$$
(r,\phi,z),
$$

where

- $r$ — cylindrical distance from the star,
- $\phi$ — azimuthal angle,
- $z$ — height above/below the disk midplane.

The disk is approximately axisymmetric, so many models assume

$$
\frac{\partial}{\partial\phi}=0.
$$

A useful conceptual picture is:

```text
                         z
                         ↑
                         │
                 hot upper atmosphere
                    ╱───────────╲
                  ╱               ╲
                ╱    molecular     ╲
              ╱       layer         ╲
            ╱─────────────────────────╲
           │                           │
           │       dense midplane      │
           │                           │
            ╲─────────────────────────╱
              ╲                     ╱
                ╲                 ╱
                  ╲_____________╱
                         │
                         ★
                       Star
                         │
                         └──────→ r
````

The disk is therefore a **three-dimensional structure**, even though it is geometrically thin compared with its radial extent:

$$
H \ll r,
$$

where $H$ is the disk scale height.

---

# 2. The Three Fundamental Ingredients

The physical structure of a disk follows from three basic ingredients:

### ① Gravity

The central star gravitationally attracts the gas toward it.

### ② Rotation

The disk rotates around the star and is therefore supported radially by centrifugal acceleration.

### ③ Pressure

Gas pressure provides support against the vertical component of stellar gravity.

Thus, approximately,

$$
\boxed{
\text{Radial direction: gravity} \leftrightarrow \text{rotation}
}
$$

while

$$
\boxed{
\text{Vertical direction: gravity} \leftrightarrow \text{pressure}
}
$$

This distinction is fundamental to understanding disk structure.

---

# 3. Stellar Gravitational Potential

For a star of mass $M_\star$, the Newtonian gravitational potential is

$$
\Phi(R)
=
-\frac{GM_\star}{R},
$$

where $R$ is the spherical distance from the star.

In cylindrical coordinates,

$$
R=\sqrt{r^2+z^2}.
$$

Therefore,

$$
\boxed{
\Phi(r,z)
=
-\frac{GM_\star}{\sqrt{r^2+z^2}}
}
$$

This is the gravitational potential used in the vertical structure equation.

---

# 4. Vertical Hydrostatic Equilibrium

## 4.1 Start from Euler's Equation

The equation of motion for an inviscid fluid is

$$
\rho
\frac{D\mathbf{v}}{Dt}
=
-\nabla P
-
\rho\nabla\Phi.
$$

Here,

* $\rho$ is the gas density,
* $P$ is the gas pressure,
* $\mathbf{v}$ is the velocity,
* $\Phi$ is the gravitational potential.

Taking the vertical component gives

$$
\rho\frac{Dv_z}{Dt}
=
-\frac{\partial P}{\partial z}
-
\rho\frac{\partial\Phi}{\partial z}.
$$

---

## 4.2 Hydrostatic Approximation

For a disk in vertical hydrostatic equilibrium, there is approximately no vertical acceleration:

$$
\frac{Dv_z}{Dt}\approx0.
$$

Therefore,

$$
0
=
-\frac{\partial P}{\partial z}
-
\rho\frac{\partial\Phi}{\partial z}.
$$

Rearranging,

$$
\boxed{
\frac{1}{\rho}
\frac{\partial P}{\partial z}
=
-\frac{\partial\Phi}{\partial z}
}
$$

This is the **general equation for hydrostatic equilibrium in a gravitational potential**.

---

# 5. Deriving the Disk Equation

Substitute the stellar potential

$$
\Phi
=
-\frac{GM_\star}{\sqrt{r^2+z^2}}.
$$

Then

$$
-\frac{\partial\Phi}{\partial z}
=
\frac{\partial}{\partial z}
\left(
\frac{GM_\star}{\sqrt{r^2+z^2}}
\right).
$$

Therefore,

$$
\boxed{
\frac{1}{\rho_{\rm gas}}
\frac{\partial P}{\partial z}
=
\frac{\partial}{\partial z}
\left(
\frac{GM_\star}
{\sqrt{r^2+z^2}}
\right)
}
$$

This is the equation shown in the original figure.

---

## 5.1 Explicit Form of the Gravitational Term

Differentiating,

$$
\frac{\partial}{\partial z}
\left(
\frac{GM_\star}{\sqrt{r^2+z^2}}
\right)
=
-\frac{GM_\star z}
{(r^2+z^2)^{3/2}}.
$$

Hence,

$$
\boxed{
\frac{1}{\rho_{\rm gas}}
\frac{\partial P}{\partial z}
=
-\frac{GM_\star z}
{(r^2+z^2)^{3/2}}
}
$$

or equivalently,

$$
\boxed{
\frac{\partial P}{\partial z}
=
-\rho_{\rm gas}
\frac{GM_\star z}
{(r^2+z^2)^{3/2}}
}
$$

---

# 6. Physical Interpretation

The equation simply states:

$$
\boxed{
\text{Pressure force}
+
\text{gravitational force}
=0
}
$$

Above the disk midplane ($z>0$),

$$
-\frac{GM_\star z}{(r^2+z^2)^{3/2}}<0,
$$

so gravity points downward.

The pressure must therefore decrease upward:

$$
\frac{\partial P}{\partial z}<0.
$$

The resulting pressure force,

$$
-\frac{1}{\rho}\frac{\partial P}{\partial z},
$$

points upward and balances gravity.

---

# 7. Why Does the Disk Rotate?

Hydrostatic equilibrium describes the **vertical** structure.

In the radial direction, the dominant balance is different.

For nearly circular motion,

$$
\frac{v_\phi^2}{r}
\approx
\frac{GM_\star}{r^2}.
$$

Therefore,

$$
v_\phi
\approx
\sqrt{\frac{GM_\star}{r}}.
$$

This is the Keplerian velocity:

$$
\boxed{
v_K(r)=\sqrt{\frac{GM_\star}{r}}
}
$$

and the corresponding Keplerian angular frequency is

$$
\boxed{
\Omega_K
=
\sqrt{\frac{GM_\star}{r^3}}
}
$$

Thus the disk is approximately:

* **Keplerian in the radial direction**
* **hydrostatic in the vertical direction**

---

# 8. Equation of State

The hydrostatic equation contains both pressure and density.

To obtain the density structure, we need an equation of state.

For an ideal gas,

$$
\boxed{
P
=
\frac{\rho k_B T}{\mu m_H}
}
$$

where

* $k_B$ is Boltzmann's constant,
* $T$ is the gas temperature,
* $\mu$ is the mean molecular weight,
* $m_H$ is the hydrogen mass.

Define the isothermal sound speed:

$$
\boxed{
c_s^2
=
\frac{k_B T}{\mu m_H}
}
$$

Then

$$
P=\rho c_s^2.
$$

---

# 9. General Vertical Structure

Substituting

$$
P=\rho c_s^2
$$

into hydrostatic equilibrium gives

$$
\frac{1}{\rho}
\frac{\partial}{\partial z}
(\rho c_s^2)
=
-\frac{GM_\star z}
{(r^2+z^2)^{3/2}}.
$$

Since $c_s^2\propto T$,

$$
\frac{\partial\ln\rho}{\partial z}
+
\frac{\partial\ln T}{\partial z}
=
-\frac{GM_\star z}
{c_s^2(r^2+z^2)^{3/2}}.
$$

Thus,

$$
\boxed{
\frac{\partial\ln\rho}{\partial z}
=
-\frac{\partial\ln T}{\partial z}
-
\frac{GM_\star z}
{c_s^2(r^2+z^2)^{3/2}}
}
$$

This equation is especially important because it shows that:

> **Given the temperature structure $T(r,z)$, hydrostatic equilibrium determines the vertical density structure $\rho(r,z)$.**

This is why disk models often calculate the temperature first and then solve for the density.

---

# 10. Vertically Isothermal Disk

Consider the simplest case:

$$
T=T(r),
$$

so that temperature does not vary with height.

At a fixed radius $r$,

$$
c_s=c_s(r)
$$

is constant with $z$.

The hydrostatic equation becomes

$$
c_s^2
\frac{\partial\ln\rho}{\partial z}
=
-\frac{GM_\star z}
{(r^2+z^2)^{3/2}}.
$$

Therefore,

$$
\frac{\partial\ln\rho}{\partial z}
=
-\frac{GM_\star z}
{c_s^2(r^2+z^2)^{3/2}}.
$$

Integrating from the midplane ($z=0$) to height $z$ gives

$$
\ln\left(\frac{\rho}{\rho_0}\right)
=
\frac{GM_\star}{c_s^2}
\left[
\frac{1}{\sqrt{r^2+z^2}}
-
\frac{1}{r}
\right].
$$

Hence,

$$
\boxed{
\rho(r,z)
=
\rho_0(r)
\exp
\left[
\frac{GM_\star}{c_s^2}
\left(
\frac{1}{\sqrt{r^2+z^2}}
-
\frac{1}{r}
\right)
\right]
}
$$

where $\rho_0(r)$ is the midplane density.

This is the **full vertical density structure** without making the thin-disk approximation.

---

# 11. Thin-Disk Approximation

Protoplanetary disks are generally geometrically thin:

$$
z\ll r.
$$

We can therefore expand

$$
\frac{1}{\sqrt{r^2+z^2}}
$$

around $z=0$:

$$
\frac{1}{\sqrt{r^2+z^2}}
\approx
\frac{1}{r}
-
\frac{z^2}{2r^3}.
$$

Therefore,

$$
\frac{1}{\rho}
\frac{\partial P}{\partial z}
\approx
-\frac{GM_\star}{r^3}z.
$$

Since

$$
\Omega_K^2
=
\frac{GM_\star}{r^3},
$$

we obtain

$$
\boxed{
\frac{1}{\rho}
\frac{\partial P}{\partial z}
=
-\Omega_K^2z
}
$$

This is the standard thin-disk vertical hydrostatic equation.

---

# 12. The Disk Scale Height

For an isothermal disk,

$$
P=\rho c_s^2.
$$

Thus,

$$
c_s^2
\frac{\partial\ln\rho}{\partial z}
=
-\Omega_K^2z.
$$

Integrating,

$$
\ln\rho
=
-\frac{\Omega_K^2z^2}{2c_s^2}
+
\text{constant}.
$$

Therefore,

$$
\rho(r,z)
=
\rho_0(r)
\exp
\left(
-\frac{z^2}{2H^2}
\right),
$$

where

$$
\boxed{
H
=
\frac{c_s}{\Omega_K}
}
$$

is the **pressure scale height**.

Thus,

$$
\boxed{
\rho(r,z)
=
\rho_0(r)
\exp
\left(
-\frac{z^2}{2H^2}
\right)
}
$$

The vertical density distribution is therefore approximately **Gaussian**.

---

# 13. What Determines the Disk Thickness?

Since

$$
H=\frac{c_s}{\Omega_K},
$$

and

$$
c_s\propto\sqrt{T},
$$

while

$$
\Omega_K\propto r^{-3/2},
$$

we obtain

$$
H
\propto
\sqrt{T}\,r^{3/2}.
$$

The aspect ratio is

$$
\boxed{
\frac{H}{r}
=
\frac{c_s}{v_K}
}
$$

so the disk becomes geometrically thicker when:

* the gas is hotter,
* the sound speed is larger,
* the stellar gravity is weaker.

Conversely, a cold disk close to a massive star is relatively thin.

---

# 14. Surface Density

The three-dimensional density $\rho(r,z)$ can be vertically integrated to obtain the surface density:

$$
\boxed{
\Sigma(r)
=
\int_{-\infty}^{+\infty}
\rho(r,z)\,dz
}
$$

For the Gaussian density profile,

$$
\rho(r,z)
=
\rho_0
e^{-z^2/(2H^2)},
$$

we obtain

$$
\Sigma
=
\sqrt{2\pi}\rho_0H.
$$

Therefore,

$$
\boxed{
\rho_0(r)
=
\frac{\Sigma(r)}
{\sqrt{2\pi}H(r)}
}
$$

This is extremely useful in disk modeling.

The radial surface-density profile and the vertical hydrostatic structure can therefore be treated separately:

$$
\boxed{
\Sigma(r)
\quad+\quad
H(r)
\quad\Longrightarrow\quad
\rho(r,z)
}
$$

---

# 15. A Useful Mental Model

The disk structure can be summarized as follows:

```text
                    STAR
                     ★
                     │
             gravitational field
                     │
                     ↓

        ┌─────────────────────────┐
        │      hot atmosphere     │
        │                         │
        │        molecular        │
        │          layer          │
        │                         │
        │=========================│
        │      dense midplane     │
        │=========================│
        │                         │
        │        molecular        │
        │          layer          │
        │                         │
        │      hot atmosphere     │
        └─────────────────────────┘
                     │
                     ↓

       Vertical direction:
       pressure ↔ stellar gravity

       Radial direction:
       rotation ↔ stellar gravity
```

---

# 16. Origin of the Hydrostatic Equation

The equation

$$
\frac{1}{\rho}
\frac{\partial P}{\partial z}
=
\frac{\partial}{\partial z}
\left(
\frac{GM_\star}{\sqrt{r^2+z^2}}
\right)
$$

should **not** be regarded as an equation invented specifically for protoplanetary disks.

Its origin is much more fundamental.

The chain is:

$$
\boxed{
\text{Euler equation}
}
$$

↓

$$
\boxed{
\text{Hydrostatic equilibrium}
}
$$

↓

$$
\boxed{
\frac{1}{\rho}\frac{\partial P}{\partial z}
=
-\frac{\partial\Phi}{\partial z}
}
$$

↓

$$
\boxed{
\Phi
=
-\frac{GM_\star}{\sqrt{r^2+z^2}}
}
$$

↓

$$
\boxed{
\frac{1}{\rho}
\frac{\partial P}{\partial z}
=
-\frac{GM_\star z}
{(r^2+z^2)^{3/2}}
}
$$

The equation is therefore a direct consequence of **Newtonian gravity and fluid mechanics**.

---

# 17. Historical Literature

The exact equation in the original figure appears in **Richard Teague's 2017 PhD thesis**, in the section *Volume Density Structure*, as equation (2.5).

However, Teague did **not** originate the physical equation.

A useful historical chain is:

| Level                      | Reference                              | Contribution                                |
| -------------------------- | -------------------------------------- | ------------------------------------------- |
| Fundamental physics        | Euler equation + Newtonian gravity     | Basic force balance                         |
| Early theoretical work     | von Weizsäcker (1948)                  | Early astrophysical disk treatment          |
| Classical disk theory      | **Pringle (1981)**                     | Canonical accretion-disk formulation        |
| Circumstellar disks        | **Alexander, Clarke & Pringle (2004)** | Explicit full vertical hydrostatic equation |
| Protoplanetary disks       | **Andrews et al. (2012)**              | Modern disk-structure implementation        |
| Exact source of the figure | **Teague (2017)**                      | Equation (2.5) shown here                   |

### Key references

**Pringle, J. E. (1981)**
*Accretion Discs in Astrophysics*
Annual Review of Astronomy and Astrophysics, **19**, 137–162.

**Alexander, R. D., Clarke, C. J., & Pringle, J. E. (2004)**
*The effects of X-ray photoionization and heating on the structure of circumstellar discs*
Monthly Notices of the Royal Astronomical Society, **354**, 71.

**Andrews, S. M. et al. (2012)**
*The TW Hya Disk at 870 μm: Comparison of CO and Dust Radial Structures*
The Astrophysical Journal, **744**, 162.

**Teague, R. (2017)**
*Tracing the Earliest Stages of Planet Formation through Modelling and Sub-mm Observations*
PhD Thesis, Heidelberg University.

---

# 18. Important Assumptions

The standard hydrostatic disk equation relies on several assumptions.

### 1. Point-mass gravity

$$
\Phi=-\frac{GM_\star}{R}.
$$

The disk's self-gravity is neglected.

---

### 2. Hydrostatic vertical equilibrium

$$
\frac{Dv_z}{Dt}\approx0.
$$

The disk is assumed to have negligible large-scale vertical acceleration.

---

### 3. Axisymmetry

$$
\frac{\partial}{\partial\phi}=0.
$$

---

### 4. Ideal-gas equation of state

$$
P=\frac{\rho k_BT}{\mu m_H}.
$$

---

### 5. Thin-disk approximation

When deriving the Gaussian profile,

$$
z\ll r
$$

is additionally assumed.

---

### 6. Vertically isothermal structure

The Gaussian solution assumes

$$
T=T(r).
$$

This assumption is **not** required for the original hydrostatic equation itself.

---

# 19. Full vs. Approximate Structure

It is important not to confuse these two equations.

### Full gravitational field

$$
\boxed{
\frac{1}{\rho}
\frac{\partial P}{\partial z}
=
-\frac{GM_\star z}
{(r^2+z^2)^{3/2}}
}
$$

This remains valid even when $z/r$ is not extremely small.

### Thin-disk approximation

$$
\boxed{
\frac{1}{\rho}
\frac{\partial P}{\partial z}
\simeq
-\Omega_K^2z
}
$$

This assumes

$$
z\ll r.
$$

The second equation leads to the familiar Gaussian vertical density profile.

---

# 20. Complete Physical Picture

The structure of a simple protoplanetary disk can therefore be summarized by:

### Gravity

$$
\Phi
=
-\frac{GM_\star}{\sqrt{r^2+z^2}}
$$

### Keplerian rotation

$$
v_K
=
\sqrt{\frac{GM_\star}{r}}
$$

### Keplerian frequency

$$
\Omega_K
=
\sqrt{\frac{GM_\star}{r^3}}
$$

### Sound speed

$$
c_s
=
\sqrt{\frac{k_BT}{\mu m_H}}
$$

### Scale height

$$
H
=
\frac{c_s}{\Omega_K}
$$

### Aspect ratio

$$
\frac{H}{r}
=
\frac{c_s}{v_K}
$$

### Hydrostatic equilibrium

$$
\frac{1}{\rho}
\frac{\partial P}{\partial z}
=
-\frac{GM_\star z}
{(r^2+z^2)^{3/2}}
$$

### Thin, vertically isothermal density

$$
\boxed{
\rho(r,z)
=
\frac{\Sigma(r)}
{\sqrt{2\pi}H(r)}
\exp
\left[
-\frac{z^2}{2H^2(r)}
\right]
}
$$

Together, these equations provide the basic physical framework for modeling the structure of a protoplanetary disk.

---

# 21. The Big Picture

The most important conceptual chain is:

$$
\boxed{
M_\star
\longrightarrow
\Phi
\longrightarrow
g_z
\longrightarrow
P(z)
\longrightarrow
\rho(z)
}
$$

More explicitly,

$$
M_\star
\rightarrow
\Phi(r,z)
=
-\frac{GM_\star}{\sqrt{r^2+z^2}}
$$

then

$$
\Phi
\rightarrow
g_z
=
-\frac{GM_\star z}
{(r^2+z^2)^{3/2}}
$$

then

$$
g_z
\rightarrow
\frac{1}{\rho}
\frac{\partial P}{\partial z}
=
g_z
$$

and, with an equation of state,

$$
P(\rho,T)
\rightarrow
\rho(r,z).
$$

Thus:

> **The thermal structure of a disk determines its pressure support, and pressure support determines how the gas is vertically distributed against stellar gravity.**

---

## ⭐ Key Takeaways

1. **A protoplanetary disk is approximately Keplerian radially and hydrostatic vertically.**

2. The vertical hydrostatic equation is

   $$
   \boxed{
   \frac{1}{\rho}
   \frac{\partial P}{\partial z}
   =
   -\frac{GM_\star z}
   {(r^2+z^2)^{3/2}}
   }
   $$

3. This equation comes directly from **Euler's equation + Newtonian gravity**.

4. The equation in the original figure is the same equation written using the stellar gravitational potential.

5. For a vertically isothermal ideal gas,

   $$
   P=\rho c_s^2.
   $$

6. In the thin-disk limit,

   $$
   z\ll r,
   $$

   the gravitational acceleration becomes

   $$
   g_z\simeq-\Omega_K^2z.
   $$

7. This produces a Gaussian vertical density distribution:

   $$
   \rho(r,z)
   =
   \rho_0(r)
   e^{-z^2/(2H^2)}.
   $$

8. The scale height is

   $$
   \boxed{
   H=\frac{c_s}{\Omega_K}
   }
   $$

9. A hotter disk has a larger scale height and is therefore geometrically thicker.

10. The equation in the screenshot is from **Teague (2017), Eq. (2.5)**, but the underlying physics is much older and belongs to the classical theory of hydrostatic fluids and accretion disks.

---

> ### One-line summary
>
> **A protoplanetary disk exists as a thin rotating structure because stellar gravity is balanced by orbital motion in the radial direction and by gas pressure in the vertical direction; given the disk temperature, this vertical hydrostatic balance determines its density structure.**

