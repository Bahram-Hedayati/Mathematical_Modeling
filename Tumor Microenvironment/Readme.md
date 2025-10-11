# Mathematical Modeling of Single Tumor Growth

This notebook mathematically models single tumor growth process in a tissue microenvironment using the **Invasion Percolation algorithm**. The simulation models the tumor's expansion based on the resistance of the surrounding tissue voxels.

## Simulation Overview

The simulation proceeds as follows:

0.  **Libraries and Functions:**
    * Import required Python libraries
    * Write helper functions

1.  **Initialization:**
    *   A 2D grid representing the tissue is created with randomly assigned resistance values for each voxel.
    *   A tumor core is initialized at a central location in the grid.

2.  **Tumor Invasion:**
    *   The tumor grows by invading neighboring voxels.
    *   The algorithm prioritizes invasion into voxels with lower resistance.
    *   A priority queue is used to manage the potential invasion sites, always selecting the one with the minimum resistance.
    *   The simulation runs for a specified number of growth steps, representing a period of time (15 days).
    *   At each step, the voxel with the lowest resistance from the current tumor frontier is invaded.
    *   The newly invaded voxel's neighbors are added to the frontier if they are not already part of the tumor.

3. **Save Results:**
    * In order to save the location of cancerous voxels, the **csv library** is leveraged.
    * **Pandas library** is utilized to make sure that the cancerous voxels are saved correctly.

# Tumor Growth Model
For completeness, the theoretical principles behind the number of voxels I simulated in this notebook is explained clearly.

To simulate the cancerous cell proliferation within an environment, there is a need for a biologically reasonable tumor growth model. Among lots of tumor growth models, such as exponential growth, exponential linear, power law, Gompertz, Richards, Von Bertalanffy, etc., I consider the logistic growth model, which is reliable, interpretable, descriptive for many types of tumors, supports biologically meaningful parameters, and computationally tractable. The logistic tumor growth model is developed by Edelstein-Keshet, which considers resource limitations, such as space, oxygen, and nutrients, to simulate a realistic tumor expansion. This model leverages the logistic equation introduced by Verhulst, which is written as

$$
\frac{dN}{dt} = rN \left( 1 - \frac{N}{K} \right)
$$

where $N$ represents the population size, $r$ denotes the growth rate, and $K$ refers to the carrying (maximum) capacity. Considering the logistic equation as the baseline, the logistic tumor growth can obtain the number of cancerous cells over time using 

$$
N(t) = \frac{K}{1 + \left( \frac{K - N_0}{N_0} \right) e^{-rt}},
$$

where $N(t)$ and $N_0$ denote the number of cells at time $t$ and the initial number of cells within the environment, respectively. Carrying capacity $K$ refers to the maximum number of cells that the environment can fit. Furthermore, the intrinsic growth rate of the cancerous cells is shown by $r$. 

The tumor's growth rate $r$ in the equation above depends on various factors such as tumor type, experimental setting (in vitro, in vivo, or in silico), cell line proliferation rate, etc. For instance, we can compute this rate according to the typical doubling times of cancer proliferation cells per 1 to 3 days using the relationship below

$$
r = \frac{\ln 2}{T_d}.
$$

where $T_d$ denotes the doubling time, which means the total number of cancerous cells doubles every $T_d$ days. Therefore, the growth rates for different values of typical doubling times $T_{d1} = 1$, $T_{d2} = 2$, and $T_{d3} = 3$ per hour can be obtained as $r_1 = 0.0289$, $r_2 = 0.0144$, $r_3 = 0.0096$, respectively.

In this simulation, I assumed there are arount 30 cells inside each voxel, which is a realistic assumption according to the biological scientific references.