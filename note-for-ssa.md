# SSA使用指南
参考文献：

:book:  [1] Hassani, H. (2007). "Singular spectrum analysis: methodology and comparison."

:book: [2] Vautard, R., P. Yiou and M. Ghil (1992). "Singular-Spectrum Analysis - a Toolkit for Short, Noisy Chaotic Signals." Physica D 58(1-4): 95-126.

## Aim^[1]^

decomposite the **original time series** :arrow_right: **independent & interpretable components**

* Finding trend of different resolution
* Smoothing
* Extraction of seasonality components
* Change-point detection
* ......

## decomposite

L: window length ( embedding dimension)
T: period
N: length of time series

条件：
1. L < $\frac{N}{2}$
2. L = kT (k is constant)
3. successful at  $T \in(\frac{L}{5},L)$ ^[2]^


## reconstruct

information:
> 1. every harmonic component produce 2 eigentriples with close singular values.
> 2. a pure noise series produce slowly decreasing eigenvalue spectra.
> 3. pairewise scatterplot
