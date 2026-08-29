# Dust + Ice Radiative Transfer Notes: From Chemistry to RADMC-3D

These notes summarize the concepts we discussed while reading and interpreting the paper on simulated ice observations of protoplanetary disks.

> **Source:** Ballering, Cleeves & Anderson (2021), *The Astrophysical Journal*, 920, 115.
>
> The notes distinguish the paper's stated methodology from explanatory interpretation.

---

## 1. Big-picture workflow

The paper connects a chemical disk model to radiative transfer through the following chain:

```text
Chemical model
     |
     | ice abundances x_i
     v
Equation (5)
     |
     | ice mass densities rho_i
     v
Equation (6)
     |
     | distribute ice between small/large grains
     v
V_i = rho_i / rho_m,i
     |
     | convert mass density to material volume
     v
f_i = V_i / sum(V_i)
     |
     | volume fractions
     v
Bruggeman mixing
     |
     | effective optical constants n_eff(lambda), k_eff(lambda)
     v
Mie theory
     |
     | absorption opacity, scattering opacity,
     | phase function / scattering matrix
     v
RADMC-3D
     |
     v
Synthetic spectra / images
```

The paper explicitly describes this sequence: chemical abundances are extracted for six ice species, converted into dust+ice mass densities, apportioned between grain populations, converted to volume fractions, and then used to construct representative optical properties for RADMC-3D.

---

# 2. Phase function: what does it mean physically?

The scattering phase function describes the angular distribution of scattered radiation.

If a photon is scattered through an angle `theta`, the phase function tells us how probable/strong scattering is at that angle.

For strongly forward-scattering grains, the phase function can be strongly peaked near

```text
theta = 0 degrees
```

meaning that scattered photons tend to continue almost in the original direction.

A small scattering angle does **not** mean every photon is deflected by exactly that angle. It describes the angular distribution of scattering.

---

# 3. The 3-degree forward-scattering truncation

The paper states that the Mie calculation provides the full scattering matrix at 181 angles from 0 to 180 degrees. It then says:

> "We truncate the phase function within 3° of forward-scattering because it rises sharply toward the peak and is difficult to sample properly."

The physical/computational interpretation is:

```text
0 deg                         3 deg                         180 deg
 |-----------------------------|-------------------------------|
       forward-scattering             explicitly sampled
          region
```

For the radiative-transfer calculation, scattering events within 3 degrees of the forward direction are treated as effectively producing no meaningful change in the photon packet's direction.

Thus:

```text
theta < 3 deg  --> effectively ignore the deflection
theta >= 3 deg --> treat the scattering explicitly
```

This does **not** mean the dust physically does not interact with the photon. The dust still scatters the photon physically. It means the simulation approximates the very small deflection as negligible.

The paper says this truncation has minimal impact on the radiative transfer.

---

# 4. Scattered stellar light, scattered warm-dust light, and direct warm-dust emission

A useful conceptual decomposition is:

### Red: scattered stellar light

```text
STAR
  |
  | stellar photon
  v
 DUST
  |
  | scattering
  v
OBSERVER
```

This component is stellar radiation that interacted with dust through scattering before reaching the observer.

### Blue: scattered warm-dust light

```text
WARM DUST
    |
    | thermal emission
    v
  DUST
    |
    | scattering
    v
 OBSERVER
```

Here the photon originates from thermal emission by warm dust and is then scattered before reaching the observer.

### Orange: direct warm-dust emission

```text
WARM DUST
    |
    | thermal emission
    |
    +------------------> OBSERVER
             no scattering
```

This is thermal dust emission that reaches the observer without a scattering event.

The important distinction is the **origin of the photon** and its subsequent propagation history.

---

# 5. How to conceptually separate these components in RADMC-3D

RADMC-3D does not simply attach a permanent label such as "stellar photon" or "dust photon" to every photon in the final ray-traced spectrum in the same way a photon-history analysis code can.

A controlled decomposition can instead be constructed with separate runs.

Conceptually:

| Component | Stellar source | Dust temperature | Scattering |
|---|---|---|---|
| Scattered stellar light | ON | thermal emission suppressed for the diagnostic run | ON |
| Scattered warm-dust light | OFF | normal | ON |
| Direct warm-dust emission | OFF | normal | OFF / zero scattering events |

The exact implementation depends on the RADMC-3D setup and version.

A crucial point is that simply using `nostar` is not equivalent to removing the star as a source of photons in every part of a scattering calculation. If the goal is a clean physical decomposition, the stellar source should actually be disabled for the dust-only diagnostic run.

Also, do not recalculate the physical dust temperature merely to decompose the emergent spectrum. The temperature should normally be calculated from the physical model first and then reused for controlled post-processing experiments.

---

# 6. Equation (5): ice mass density

The paper gives the mass density of each ice species as

$$
\boxed{
\rho_{\rm ice}
=
\rho_g
\frac{x_{\rm ice}M_{\rm ice}}
{x_gM_g}
}
\tag{5}
$$

where:

- `rho_ice` = mass density of the particular ice species
- `rho_g` = gas mass density
- `x_ice` = abundance of the ice species relative to total H atoms
- `M_ice` = molar/molecular mass of the ice species
- `x_g` = adopted gas abundance relative to total H atoms
- `M_g` = adopted mean gas mass

The paper uses

$$
x_g=0.64,
\qquad
M_g=2.44.
$$

---

## 6.1 Why does gas density appear if ice is on dust?

This is a very important conceptual distinction.

Ice is physically deposited on dust grains. The gas is **not** where the ice is stored.

The reason gas density appears in Equation (5) is that the chemical abundance is defined relative to hydrogen atoms.

For example,

$$
x_{\rm H_2O}
=
\frac{n_{\rm H_2O}}{n_{\rm H}}.
$$

If the local gas density is known, it gives the local number of hydrogen atoms. The abundance then tells us how many ice molecules correspond to those hydrogen atoms.

The conversion is therefore:

```text
chemical abundance relative to H
          |
          v
number of ice molecules per volume
          |
          v
ice mass per volume
```

So Equation (5) is a **normalization/conversion step**, not a statement that ice occupies the gas.

The paper then separately distributes the ice mass between the small- and large-grain populations.

---

# 7. Equation (6): distributing ice between small and large grains

The paper assumes that the amount of ice associated with a grain population is proportional to the available dust surface area.

The fraction of ice mass assigned to the large-grain population is

$$
\boxed{
f_{\rm ice,l}
=
\frac{f_{\rm dust,l}}
{
\sqrt{a_{\max,l}/a_{\max,s}}\,(1-f_{\rm dust,l})
+
f_{\rm dust,l}
}
}
\tag{6}
$$

where:

- `f_ice,l` = fraction of the total ice mass assigned to large grains
- `f_dust,l` = local fraction of dust mass in large grains
- `a_max,l` = maximum grain size of the large-grain population
- `a_max,s` = maximum grain size of the small-grain population

For the adopted grain-size distribution with `q = 3.5`, the paper states that the ratio of surface area to mass of the small-grain population relative to the large-grain population is

$$
\sqrt{\frac{a_{\max,l}}{a_{\max,s}}}=1000.
$$

---

## 7.1 What is `f_dust,l`?

It is the **local fraction of the dust mass density in the large-grain population**:

$$
\boxed{
f_{\rm dust,l}
=
\frac{\rho_{\rm dust,l}}
{\rho_{\rm dust,l}+\rho_{\rm dust,s}}
}
$$

Thus it is conceptually a **cell-by-cell quantity**.

If the underlying disk model assumes the same small/large dust ratio everywhere, then its numerical value may be identical in every cell. But the definition is local.

---

# 8. Why surface area instead of dust mass?

Ice grows on grain surfaces.

Suppose two populations contain the same dust mass:

```text
Small grains:
• • • • • • • • •
• • • • • • • • •

Large grains:
●       ●       ●
```

The small grains have much more total surface area.

For a grain:

$$
A \propto a^2
$$

while

$$
m \propto a^3.
$$

Therefore

$$
\frac{A}{m}\propto\frac{1}{a}.
$$

So small grains provide more surface area per unit mass.

For the grain-size distribution used in the paper, this leads to the stated factor

$$
R=
\sqrt{\frac{a_{\max,l}}{a_{\max,s}}}.
$$

The paper adopts `R = 1000`.

Thus:

$$
\text{small-grain surface area}
\propto
R(1-f_{\rm dust,l})
$$

and

$$
\text{large-grain surface area}
\propto
f_{\rm dust,l}.
$$

Therefore the large-grain fraction of total surface area, and hence the assumed large-grain fraction of ice mass, is

$$
f_{\rm ice,l}
=
\frac{f_{\rm dust,l}}
{R(1-f_{\rm dust,l})+f_{\rm dust,l}}.
$$

---

## 8.1 Numerical example

Suppose

$$
f_{\rm dust,l}=0.1
$$

and

$$
R=1000.
$$

Then:

$$
f_{\rm ice,l}
=
\frac{0.1}{1000(0.9)+0.1}
\approx1.11\times10^{-4}.
$$

So only about

$$
0.011\%
$$

of the ice would be assigned to the large grains under this assumption.

The remaining approximately 99.989% goes to small grains.

This happens because the small grains provide vastly more surface area per unit mass.

---

# 9. Equation (6) is not the fraction of H2O, CO, CO2, etc.

This distinction is essential.

Equation (6) does **not** tell you:

```text
H2O = 40%
CO2 = 20%
CO = 10%
...
```

Instead it tells you:

```text
What fraction of the ice mass goes onto LARGE grains?
```

For example:

$$
\rho_{\rm H_2O,l}
=
f_{\rm ice,l}\rho_{\rm H_2O}
$$

and

$$
\rho_{\rm H_2O,s}
=
(1-f_{\rm ice,l})\rho_{\rm H_2O}.
$$

The same splitting is applied to each ice species.

The relative amount of H2O versus CO2 versus CO comes from their chemical abundances in Equation (5).

---

# 10. Volume occupied by each material: `V_i`

After determining the mass densities, the paper converts each material's mass density into a volume-per-volume quantity:

$$
\boxed{
V_i=
\frac{\rho_i}{\rho_{m,i}}
}
$$

where:

- `rho_i` = mass density of material `i` in the cell
- `rho_m,i` = intrinsic/material density of material `i`

The paper calls this the "volume density (volume of material per volume of space)." It then normalizes these quantities to get volume fractions.

---

## 10.1 Why does dividing by material density give a volume?

Start from

$$
\rho_m=\frac{M}{V}.
$$

Rearrange:

$$
V=\frac{M}{\rho_m}.
$$

For a cell, the mass per unit cell volume is \(\rho_i\). Therefore

$$
\frac{V_i}{V_{\rm cell}}
=
\frac{\rho_i}{\rho_{m,i}}.
$$

So the paper's `V_i` can be understood as:

$$
\boxed{
V_i=
\frac{\text{volume occupied by material }i}
{\text{volume of the cell}}
}
$$

It is therefore dimensionless.

---

## 10.2 Example

Suppose:

$$
\rho_{\rm H_2O}=0.01\ {\rm g\,cm^{-3}}
$$

and the material density of H2O ice is

$$
\rho_{m,\rm H_2O}=0.87\ {\rm g\,cm^{-3}}.
$$

Then:

$$
V_{\rm H_2O}
=
\frac{0.01}{0.87}
=
0.0115.
$$

This means that the amount of H2O mass in the cell would occupy about 1.15% of the cell volume if it were packed at its bulk material density.

If the actual cell volume were 100 cm^3, then the corresponding physical H2O volume would be

$$
0.0115\times100
=
1.15\ {\rm cm^3}.
$$

---

# 11. Volume fraction

The `V_i` values are not yet normalized fractions of the solid mixture.

The paper then calculates

$$
\boxed{
f_i=
\frac{V_i}{\sum_j V_j}
}
$$

where the sum is over all constituents in the mixture.

For example, suppose:

$$
V_{\rm sil}=0.60,
$$

$$
V_{\rm H_2O}=0.30,
$$

$$
V_{\rm CO_2}=0.10.
$$

Then:

$$
\sum_i V_i=1.00.
$$

So:

$$
f_{\rm sil}=60\%,
$$

$$
f_{\rm H_2O}=30\%,
$$

$$
f_{\rm CO_2}=10\%.
$$

These are **volume fractions**.

---

# 12. Mass fraction versus volume fraction

These are not the same.

Suppose:

$$
\rho_{\rm sil}=3\times10^{-14}
$$

and

$$
\rho_{\rm H_2O}=1\times10^{-14}
$$

g cm^-3.

The mass fractions would be

$$
X_{\rm sil}
=
\frac{3}{3+1}
=
75\%
$$

and

$$
X_{\rm H_2O}=25\%.
$$

But silicate and H2O have different intrinsic densities.

Using

$$
V_i=\frac{\rho_i}{\rho_{m,i}}
$$

changes the relative volumes.

The paper deliberately uses **volume fractions, not mass fractions**, to mix the materials.

---

# 13. What does the volume fraction represent physically?

It is useful to think of the grain material as a mixture:

```text
One representative grain

+--------------------------------+
|                                |
|  silicate  silicate  H2O      |
|       H2O       CO2            |
|  silicate     H2O             |
|        CO2       silicate     |
|                                |
+--------------------------------+
```

The volume fractions tell us how much of the grain's material is represented by each constituent.

For example:

$$
f_{\rm sil}=0.60
$$

means 60% of the material volume is assigned to silicate.

$$
f_{\rm H_2O}=0.30
$$

means 30% is assigned to H2O ice.

And so on.

---

# 14. Laboratory optical constants

For each pure material, the authors collect laboratory-measured optical constants from the literature.

The complex refractive index can be written as

$$
\boxed{
m(\lambda)=n(\lambda)+ik(\lambda)
}
$$

where:

- `n(lambda)` is the real part
- `k(lambda)` is the imaginary part

The real and imaginary parts describe how electromagnetic radiation propagates through and is absorbed by the material.

The paper uses one set of optical constants for each species, so it does **not** model changes in the strength and exact spectral location of ice features with temperature.

---

# 15. Why interpolate onto a common wavelength grid?

Different laboratory measurements generally use different wavelength grids.

For example:

```text
H2O:   lambda_1, lambda_2, lambda_3, ...
CO2:   lambda'_1, lambda'_2, lambda'_3, ...
sil:   lambda'', lambda'', ...
```

To mix materials at a given wavelength, all datasets need to be evaluated at the same wavelengths.

The paper therefore interpolates every optical-constant dataset onto a common grid of 683 wavelength points between

$$
0.1\ \mu{\rm m}
$$

and

$$
104\ \mu{\rm m}.
$$

The grid is particularly dense in the near-to-mid-IR, where the ice features occur.

---

# 16. Why extrapolate outside laboratory measurements?

Some ice optical constants are only measured over limited wavelength ranges.

The authors state that, at the edges of the measured ranges:

- the imaginary part is close to zero
- the real part is approximately constant

They therefore extend these edge values into wavelengths where laboratory measurements are unavailable.

This is an **extrapolation assumption**, not a claim that those wavelengths were directly measured.

Conceptually:

```text
measured range

------|=======================|------
      ↑                       ↑
      measured optical constants

outside this range:
k -> approximately 0
n -> approximately constant
```

---

# 17. Bruggeman mixing rule

Now we have:

1. optical constants for each pure constituent
2. volume fractions for each constituent

The authors assume that the grain is an **intimate mixture** of its constituent species.

This means they do not treat the ice and dust as separate populations of macroscopic grains for this particular effective-medium calculation.

Instead, they construct one effective optical medium.

For each material:

$$
m_i=n_i+ik_i
$$

and therefore

$$
\epsilon_i=m_i^2.
$$

The Bruggeman effective-medium approximation finds an effective dielectric function

$$
\epsilon_{\rm eff}
$$

from the individual dielectric functions and volume fractions.

For multiple constituents, the Bruggeman equation can be expressed as

$$
\boxed{
\sum_i
f_i
\frac{\epsilon_i-\epsilon_{\rm eff}}
{\epsilon_i+2\epsilon_{\rm eff}}
=0
}
$$

Then:

$$
m_{\rm eff}
=
\sqrt{\epsilon_{\rm eff}}
=
n_{\rm eff}+ik_{\rm eff}.
$$

So the calculation is:

```text
H2O optical constants ─────┐
CO2 optical constants ─────┤
silicate optical constants ┤
graphite optical constants ┤
CH3OH optical constants ───┤
                           |
volume fractions ──────────┘
              |
              v
     Bruggeman mixing
              |
              v
effective n(lambda), k(lambda)
```

---

# 18. What does "intimate mixture" mean?

It means the different materials are treated as components of the same effective material on scales relevant to the optical calculation.

This is different from assuming:

```text
separate silicate grain
separate ice grain
separate graphite grain
```

Instead, the calculation represents a composite dust/ice material with one effective optical response.

The paper explicitly says:

> "By using the Bruggeman mixing rule, we assume the grains are an intimate mixture of the constituent species."

This is an assumption of the model.

---

# 19. From effective optical constants to Mie opacities

Once the authors have

$$
n_{\rm eff}(\lambda)
$$

and

$$
k_{\rm eff}(\lambda),
$$

they can calculate the optical behavior of the grains using Mie theory.

This produces quantities such as:

$$
\kappa_{\rm abs}(\lambda)
$$

and

$$
\kappa_{\rm scat}(\lambda).
$$

It also gives the angular scattering information, including the phase function/scattering matrix.

The paper states that it computes absorption and scattering opacity spectra from the optical constants using Mie theory with the Mie code included in RADMC-3D.

---

# 20. Complete physical picture

The whole method can therefore be understood as four major layers.

## Layer 1 — Chemistry

The chemical model tells us:

$$
x_{\rm H_2O},
x_{\rm CO},
x_{\rm CO_2},
x_{\rm CH_3OH},
x_{\rm NH_3},
x_{\rm CH_4}.
$$

These describe **how much of each ice species exists relative to hydrogen**.

## Layer 2 — Mass and grain composition

Equation (5):

$$
x_i\rightarrow\rho_i.
$$

Equation (6):

$$
\rho_i\rightarrow
\rho_{i,\rm small},
\rho_{i,\rm large}.
$$

Then:

$$
\rho_i\rightarrow V_i
$$

and

$$
V_i\rightarrow f_i.
$$

Now we know the **composition of the dust/ice material by volume**.

## Layer 3 — Optical properties

$$
\{f_i,n_i,k_i\}
\rightarrow
n_{\rm eff},k_{\rm eff}
$$

using Bruggeman mixing.

Then:

$$
n_{\rm eff},k_{\rm eff}
\rightarrow
\kappa_{\rm abs},\kappa_{\rm scat},P(\theta)
$$

using Mie theory.

## Layer 4 — Radiative transfer

RADMC-3D uses:

- density
- temperature
- absorption opacity
- scattering opacity
- scattering phase function/matrix
- stellar radiation

to calculate synthetic spectra/images.

---

# 21. The most important conceptual distinctions

### A. Ice abundance is not ice mass density

$$
x_{\rm ice}
\neq
\rho_{\rm ice}.
$$

Abundance is a number ratio relative to hydrogen.

Mass density is grams per cubic centimeter.

Equation (5) connects them.

---

### B. Ice is not distributed through the gas

The gas density is used only as the reference needed to convert the abundance into an absolute density.

The ice is assigned to dust populations through Equation (6).

---

### C. `f_dust,l` is not `f_ice,l`

$$
f_{\rm dust,l}
=
\text{fraction of dust mass in large grains}
$$

whereas

$$
f_{\rm ice,l}
=
\text{fraction of ice mass assigned to large grains}.
$$

They are generally very different because the ice allocation is surface-area weighted.

---

### D. Equation (6) is not the H2O/CO2/CO composition

Equation (6) answers:

> What fraction of the ice goes to large grains?

The chemical abundances answer:

> How much H2O, CO, CO2, etc. exists?

---

### E. `V_i` is not the same thing as the normalized volume fraction

First:

$$
V_i=\frac{\rho_i}{\rho_{m,i}}.
$$

Then:

$$
f_i=\frac{V_i}{\sum_jV_j}.
$$

So `V_i` is the unnormalized volume-per-cell quantity, while `f_i` is the normalized composition by volume.

---

### F. Volume fraction is not mass fraction

Because different materials have different intrinsic densities:

$$
f_i\neq X_i
$$

in general.

The paper explicitly uses volume fractions for the dust/ice mixture.

---

# 22. One-line summary of each equation

### Equation (5)

$$
\boxed{
\rho_{\rm ice}
=
\rho_g
\frac{x_{\rm ice}M_{\rm ice}}
{x_gM_g}
}
$$

**Meaning:** Convert chemical abundance into the mass density of each ice species.

### Equation (6)

$$
\boxed{
f_{\rm ice,l}
=
\frac{f_{\rm dust,l}}
{\sqrt{a_{\max,l}/a_{\max,s}}(1-f_{\rm dust,l})+f_{\rm dust,l}}
}
$$

**Meaning:** Divide the ice mass between large and small grains according to their available surface area.

### Volume calculation

$$
\boxed{
V_i=\frac{\rho_i}{\rho_{m,i}}
}
$$

**Meaning:** Convert the mass density of each constituent into the volume it would occupy at its intrinsic material density.

### Volume fraction

$$
\boxed{
f_i=\frac{V_i}{\sum_jV_j}
}
$$

**Meaning:** Normalize the material volumes to obtain the composition by volume.

### Bruggeman

$$
\boxed{
\{f_i,\epsilon_i\}
\rightarrow
\epsilon_{\rm eff}
}
$$

**Meaning:** Combine the optical properties of the intimate mixture into one effective optical medium.

### Mie theory

$$
\boxed{
n_{\rm eff},k_{\rm eff}
\rightarrow
\kappa_{\rm abs},\kappa_{\rm scat},P(\theta)
}
$$

**Meaning:** Calculate how the resulting grains absorb and scatter light.

---

# 23. Practical mental model

When implementing or interpreting this paper, keep these three questions separate:

### Question 1 — How much ice is there?

Use the chemistry + Equation (5).

### Question 2 — Which grains carry the ice?

Use Equation (6), based on grain surface area.

### Question 3 — What are the optical properties of those grains?

Convert mass densities to volume fractions, mix optical constants with Bruggeman, and use Mie theory.

That separation prevents most of the confusion around Equations (5) and (6).

---

## Source note

These notes are based primarily on the uploaded paper and our discussion of its methodology. In particular, the paper states that the chemical model provides abundances of six ice species, that these are converted into mass densities, that ice is apportioned between small and large grains according to dust surface area, and that the resulting dust/ice mixture is represented using volume fractions. It then constructs optical properties for radiative-transfer modeling.

