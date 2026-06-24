# Data science

This knowledge describes content that process the data.


## Table of Contents

- [Data science](#data-science)
  - [Table of Contents](#table-of-contents)
  - [Libraries](#libraries)
    - [Pandas](#pandas)
      - [DataFrame](#dataframe)
    - [Numpy](#numpy)
      - [ND Array](#nd-array)
    - [HDF5](#hdf5)
  - [Mathematics, statistics, linear algebra](#mathematics-statistics-linear-algebra)
  - [Signal processing and control](#signal-processing-and-control)
    - [Control package](#control-package)
    - [SciPy control/simulation](#scipy-controlsimulation)


## Libraries

### Pandas

Library for working mainly with tabular data

#### DataFrame

Pandas is required.
If `inplace=True`, changes are applied to the original object; otherwise, operations typically return a new object.

```python
import numpy as np
import pandas as pd

# Load from TSV/CSV
data = pd.read_csv(
    "data/files/complex_data_example.tsv",
    sep="\t",
    quotechar="'",
    dtype={"salary": int},
    usecols=["name", "birth_date"],
    parse_dates=["birth_date"],
    skiprows=10,
    na_values=[".", "??"],
)

# Create objects
series = pd.Series([1, 2, 3, 4])
df = pd.DataFrame(
    [["tom", 10, 1], ["nick", 15, 2], ["juli", 14, 3]],
    columns=["name", "age", "row_id"],
)
df_from_dict = pd.DataFrame.from_dict({"a": 1, "b": 2}, orient="index")

base_matrix = np.array([[1, 2], [2, 3], [3, 4]])
df_from_array = pd.DataFrame(base_matrix, columns=["c1", "c2"])

# Access
name_col = df["name"]
subset = df[["name", "age"]]
first_col = df.iloc[:, 0:1]
first_row = df.iloc[0]

# Column/index metadata
removed_row_id = df.pop("row_id")
first_column_name = df.columns[0]
age_col_idx = df.columns.get_loc("age")
filtered = df.loc[df["name"] == "juli"]

# Build derived values
df["name_and_age"] = df["name"] + "_" + df["age"].astype(str)

# Keep index conversion non-destructive
indexed = df.set_index("name")
restored = indexed.reset_index(drop=False)

# Convert to array/list
age_values = df["age"].values
subset_values = subset.iloc[:, 1:].values
age_list = df["age"].tolist()

# Date range and resample
length = len(df.index)
start = pd.Timestamp(year=2020, month=3, day=14, hour=4)
df["event_start"] = pd.date_range(start=start, periods=length, freq="h")
df["event_start_time"] = df["event_start"].dt.time

df_by_time = df.set_index("event_start")
df_by_time.index = pd.to_datetime(df_by_time.index)
df_by_time.sort_index(inplace=True)
monthly_resampled = df_by_time.resample("ME").sum(numeric_only=True)

# Safe copy
df_copy = df.copy()

# Reorder columns
df_copy.insert(0, "age_copy", df_copy["age"])
first_col = df_copy.pop(df_copy.columns[0])
df_copy.insert(1, first_col.name, first_col)

# Combine DataFrames
left = pd.DataFrame([["a", 1], ["b", 2], ["c", 3]], columns=["letter", "number"])
right = pd.DataFrame([["c", 1], ["d", 2], ["e", 3]], columns=["letter", "number2"])

concat_rows = pd.concat([left, right], ignore_index=True)
concat_cols = pd.concat([left, right], axis=1)
merged = pd.merge(left, right, on="letter", how="left")

grouped_stats = concat_rows.groupby("letter").agg({"number": np.mean, "number2": np.size})
group_c = concat_rows.groupby("letter").get_group("c")

# Stats helpers
num_mean = left["number"].mean()
num_std = left["number"].std()
rolling_mean = left["number"].rolling(2).mean()
rolling_std = left["number"].rolling(2).std()
without_outliers = left[left["number"] < 2 * num_std]

# Matrix utility example
tensor = np.zeros((3, 4, 5))
moved_shape = np.moveaxis(tensor, 0, -1).shape  # (4, 5, 3)

# Keep numeric columns only
numeric_only = concat_rows.select_dtypes(include=["number"])

# Transpose
transposed = left.T
```

### Numpy

#### ND Array

```python
import numpy as np

# Shared setup (vectors/matrices are reused below)
vector = np.array([1, 2, 3, 4, 5, 6, 7])
vector_b = np.array([3, 4, 7, 6, 7, 8, 11, 12, 14])
matrix = np.array([[1, 2, 3], [3, 4, 5], [4, 5, 6]])
matrix_b = np.array([[4, 3, 5], [5, 7, 4], [6, 5, 8], [6, 5, 8]])
matrix_2x2 = np.array([[1, 2], [3, 4]])
matrix_1x2 = np.array([[5, 6]])
tensor = np.array(
    [
        [[1, 1, 1, 1], [1, 1, 1, 1], [1, 1, 1, 1]],
        [[2, 2, 2, 2], [2, 2, 2, 2], [2, 2, 2, 2]],
        [[3, 3, 3, 3], [3, 3, 3, 3], [3, 3, 3, 3]],
    ]
)

# Shapes
shape_vector = vector.shape
shape_matrix = matrix.shape
row_count = matrix.shape[0]

# Convert
as_list = vector.tolist()
as_1d_list = vector.reshape(-1).tolist()
scalar_value = vector[0].item()
as_int = vector.astype(int)

# Basic index/append/insert/roll
selected = matrix[1, 2]
extended = np.append(vector, 8)
inserted = np.insert(vector, 1, [11, 12])
rolled = np.roll(vector, 2)

# Slicing
col_no_shape = matrix[:, 1]
col_keep_shape = matrix[:, 1:2]
row = matrix[1, :]

# Boolean mask
mask = vector % 3 == 0
multiples_of_3 = vector[mask]

# np.take
take_rows = np.take(matrix_b, [1, 2], axis=0)
take_cols = np.take(matrix_b, [1, 2], axis=1)

# Swap rows/columns
matrix_swapped_rows = matrix.copy()
matrix_swapped_rows[[0, 2], :] = matrix_swapped_rows[[2, 0], :]

matrix_swapped_cols = matrix.copy()
matrix_swapped_cols[:, [0, 1]] = matrix_swapped_cols[:, [1, 0]]

# Stack/concatenate
stacked_rows = np.vstack([matrix, matrix])
stacked_cols = np.hstack([matrix, matrix])
stack_from_vectors = np.stack((np.array([1, 2, 3]), np.array([2, 3, 4])))

concat_axis0 = np.concatenate((matrix_2x2, matrix_1x2), axis=0)
concat_axis1 = np.concatenate((matrix_2x2, matrix_1x2.T), axis=1)
concat_flat = np.concatenate((matrix_2x2, matrix_1x2), axis=None)

# Append column
x = np.array([[10, 20, 30], [40, 50, 60]])
y = np.array([[100], [200]])
z = np.append(x, y, axis=1)

# Stats helpers
amin_by_row = np.amin(stack_from_vectors, axis=1)
mean_value = np.mean(x)
std_value = np.std(x)
max_abs = max(matrix.min(), matrix.max(), key=abs)

# Clip and argmin
clip_source = np.array([1.0, 2.0, 3.0, -4.0, 5.0, 6.0])
np.minimum(clip_source, 3, out=clip_source)
argmin_idx = np.unravel_index(np.argmin(matrix), shape=matrix.shape)

# Ones/zeros and slices
zeros_like = np.zeros_like(matrix)
ones = np.ones((3, 3))
slice_expr = np.index_exp[1:]

# Modify/delete
thresholded = matrix.astype(float).copy()
thresholded[thresholded > 0.5] = 0.5
deleted_row = np.delete(matrix, 1, axis=0)

# NaN handling
nan_matrix = np.array([[1.0, np.nan], [3.0, 4.0]])
without_nan_flat = nan_matrix[~np.isnan(nan_matrix)]
without_nan_rows = nan_matrix[~np.isnan(nan_matrix).any(axis=1)]
filled_nan = np.nan_to_num(nan_matrix, nan=0.0)

# Conditional replacement
where_src = np.array([1, 2, 3, 4, 1, 0, 0, 0.2])
where_result = np.where(where_src >= 1, where_src, 1)

# Sliding window via strides
window = 2
arr_1d = np.array([1, 2, 3, 4])
arr_2d = np.array([[1, 2, 3, 4], [3, 4, 5, 6]])
shape_1d = arr_1d.shape[:-1] + (arr_1d.shape[-1] - window + 1, window)
shape_2d = arr_2d.shape[:-1] + (arr_2d.shape[-1] - window + 1, window)
strides_1d = arr_1d.strides + (arr_1d.strides[-1],)
strides_2d = arr_2d.strides + (arr_2d.strides[-1],)
sliding_1d = np.lib.stride_tricks.as_strided(arr_1d, shape=shape_1d, strides=strides_1d)
sliding_2d = np.lib.stride_tricks.as_strided(arr_2d, shape=shape_2d, strides=strides_2d)

# Sum and products
sum_axis0 = np.sum([[0, 1], [0, 5]], axis=0)
sum_axis1 = np.sum([[0, 1], [0, 5]], axis=1)

dot_vectors = np.dot(np.array([1, 2, 3]), np.array([1, 2, 3]))
elemwise = matrix * matrix
dot_matrix = np.dot(matrix, matrix)

# Shape/size/cumsum
shape_info = matrix.shape
count = matrix.size
cumsum_values = np.cumsum(matrix)

# Clip
clip_demo = np.array([10, 7, 4, 3, 2, 2, 5, 9, 0, 4, 6, 0])
clipped = np.clip(clip_demo, 2, 6)

# Set difference and unique
setdiff = np.setdiff1d(np.array([1, 2, 3, 4, 5, 6, 7, 8, 9]), vector_b)
unique_rows = np.unique(matrix_b, axis=0)

# Reshape and transpose
flat = matrix.reshape(-1)
column = matrix.reshape(-1, 1)
pairs = matrix.reshape(-1, 3)
fortran_order = np.reshape(np.array([[1, 2, 3], [4, 5, 6]]), 6, order="F")

tensor_t102 = tensor.transpose(1, 0, 2)
tensor_t120 = tensor.transpose(1, 2, 0)

# Generation helpers
rand_matrix = np.random.randn(3, 3)
linspace_t = np.linspace(0, 20, 1000)
arange_t = np.arange(1, 3, 1)
arange_zero = np.arange(3)
sin_values = np.sin(arange_zero)

# Argmax/argmin
max_idx = np.argmax(matrix)
min_idx = np.argmin(matrix)

# Neighborhood iteration
arr = np.array([1, 2, 3, 4, 5, 6])
for i in range(len(arr)):
    indices = range(i - 1, i + 1)
    neighborhood = arr.take(indices, mode="clip")
    print(neighborhood)

# Linear algebra
eig_vals, eig_vecs = np.linalg.eig(rand_matrix)
inv_rand = np.linalg.inv(rand_matrix)
det_rand = np.linalg.det(rand_matrix)

# BLAS/LAPACK backend info
np.__config__.show()
```


### HDF5

It's data format and python library that allows to serialize the data.

```python
# Create

import h5py
import numpy as np

arr = np.random.randn(1000)

with h5py.File('random.hdf5', 'w') as f:
    dset = f.create_dataset("default", data=arr)

# Read

with h5py.File('random.hdf5', 'r') as f:
    data = f['default']
    print(min(data))

# Specify Data Types to Optimize Space

with h5py.File('several_datasets.hdf5', 'w') as f:
    dset_int_1 = f.create_dataset('integers', (10, ), dtype='i1')
    dset_int_8 = f.create_dataset('integers8', (10, ), dtype='i8')
    dset_complex = f.create_dataset('complex', (10, ), dtype='c16')
    dset_int_1[0] = 1200
```


## Mathematics, statistics, linear algebra

Matrix and linear algebra operations are mostly covered in the NumPy array section.

```python
from math import e
from statistics import stdev

import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
import sympy as sp
from IPython.display import Math, display
from scipy import stats
from scipy.special import binom
from sklearn import preprocessing
from statsmodels.stats.stattools import jarque_bera

# Shared matrix/vector setup
vec = np.array([1, 3, 5, 77], dtype=float)
sample = np.array([1, 2, 8, 4, 5], dtype=float)
x_matrix = np.array([[10, 20, 30], [40, 50, 60]], dtype=float)

# Basic math
display(Math(r"F(k) = \int_{-\infty}^{\infty} f(x) e^{2\pi i k} dx"))
sqrt_val = 9 ** (1 / 2)
rounded = round(5.76543, 2)
e_const = e
e_numpy = np.exp(1)
modulo = 7 % 3
power_val = 2**4

# Statistics with pandas/scipy/numpy
series = pd.Series([1, 4, 6, 88])
summary = series.describe()
stats_summary = stats.describe(vec)

np.random.seed(5)
rand_vec = np.random.randn(3)
rand_mat = np.random.randn(3, 4)
normal_dist = np.random.normal(2, 6, 1000)

t_linspace = np.linspace(0, 20, 1000)
t_arange = np.arange(1, 3, 1)
t_zero = np.arange(3)

mean_val = np.mean(x_matrix)
std_val = np.std(x_matrix)
std_sample = stdev([1, 2, 3, 4, 5])

hist_counts, hist_edges = np.histogram(x_matrix, bins=10)
cumulative = np.cumsum(x_matrix)
derivative = np.diff(sample)

# SymPy
sp.init_printing()
x_sym, y_sym, z_sym = sp.symbols("x y z")
s_sym = sp.Symbol("s")

fraction = sp.together(1 / x_sym + 1 / y_sym + 1 / z_sym)
expanded = ((x_sym + y_sym) ** 2).expand()
substituted = sp.cos(x_sym).subs(x_sym, 0)

expr = sp.sympify("x**2 + 3*x - 1/2")
simplified = sp.simplify((x_sym**3 + x_sym**2 - x_sym - 1) / (x_sym**2 + 2 * x_sym + 1))
sqrt_eval = sp.sqrt(8).evalf()

numeric_arg = np.arange(10)
sin_lambda = sp.lambdify(x_sym, sp.sin(x_sym), "numpy")
sin_values = sin_lambda(numeric_arg)

roots = sp.solve(x_sym**2 - 1, x_sym)
system_solution = sp.solve([x_sym + y_sym - 1, x_sym - y_sym - 1], [x_sym, y_sym])

# Symbolic matrix
m11, m12, m21, m22 = sp.symbols("m11 m12 m21 m22")
b1, b2 = sp.symbols("b1 b2")
a_sym = sp.Matrix([[m11, m12], [m21, m22]])
b_sym = sp.Matrix([[b1], [b2]])

sqr_mat = a_sym**2
mat_mul = a_sym * b_sym
det_sym = a_sym.det()
inv_sym = a_sym.inv()

# FFT
dt = 0.1
t_fft = np.arange(0, 10, dt)
y_fft = np.sin(t_fft)
ffty = np.fft.fft(y_fft)
real_ffty = ffty.real
imag_ffty = ffty.imag
freqs = np.fft.fftfreq(len(t_fft), dt)

# Differential equation: y'' + y = 0
f_fun = sp.Function("f")
ode_eq = f_fun(x_sym).diff(x_sym, x_sym) + f_fun(x_sym)
ode_solution = sp.dsolve(ode_eq, f_fun(x_sym))

# Integration
int_expr = sp.exp(-x_sym)
int_unbounded = sp.integrate(int_expr, x_sym)
int_bounded = sp.integrate(int_expr, (x_sym, 0, 1)).evalf()

# Correlation
corr_df = pd.DataFrame([[1, 2, 3], [4, 5, 9], [7, 8, 78]], columns=["one", "two", "three"])
corr_matrix = corr_df.corr(method="pearson")
corr_style = corr_matrix.style.format("{:.2}").background_gradient(cmap=plt.get_cmap("coolwarm"), axis=1)
np_corr = np.corrcoef(np.array([1, 2, 4, 5, 9]), np.array([7, 8, 78, 34, 2]))

# Combinatorics and normality test
comb = binom(5, 2)
residuals = [1, 3, 5, 2, 4]
score, pvalue, _, _ = jarque_bera(residuals)
if pvalue < 0.10:
    print("We have reason to suspect the residuals are not normally distributed.")
else:
    print("The residuals seem normally distributed.")

# Scaling
x_train = np.array([[1.0, -1.0, 2.0], [2.0, 0.0, 0.0], [0.0, 1.0, -1.0]])
standard_scaler = preprocessing.StandardScaler()
scaled = standard_scaler.fit_transform(x_train)
back = standard_scaler.inverse_transform(scaled)

minmax_scaler = preprocessing.MinMaxScaler(feature_range=(0, 1))
scaled_minmax = minmax_scaler.fit_transform(x_train)
```

## Signal processing and control

```python
import numpy as np
import scipy.signal as sig
from scipy.signal import butter, filtfilt, hilbert

# Shared setup
sample_rate = 1024
dt = 1.0 / sample_rate
t = np.arange(0, 2, dt)
t1 = 10
n = len(t)

# Signal
y = sig.sawtooth(2 * np.pi / 100 * t, 0.5)

# Chirp signal
t_chirp = np.linspace(0, 10, 5001)
w = sig.chirp(t_chirp, f0=6, f1=1, t1=t1, method="linear")

# Hanning window
hann_window = np.hanning(n)

# Tukey window
tukey_window = sig.windows.tukey(n)

# Fourier transform
freq = np.fft.fftfreq(n, dt)
ffty = np.fft.fft(y)
real_ffty = np.real(ffty)
imag_ffty = np.imag(ffty)

# Inverse Fourier transform
y_back = np.fft.ifft(ffty)

# Power spectral density
psd = (np.abs(ffty) ** 2) / n

# Hilbert transform - Envelope, phase, frequency
analytic_signal = hilbert(y)
amplitude_envelope = np.abs(analytic_signal)
instantaneous_phase = np.unwrap(np.angle(analytic_signal))
instantaneous_frequency = np.diff(instantaneous_phase) / (2.0 * np.pi) * sample_rate

# Filtering
t_filter = np.linspace(0, 10, 501)
chirp_signal = sig.chirp(t_filter, f0=6, f1=0.1, t1=10, method="linear")

wn = 0.1
filter_order = 2
b, a = butter(filter_order, wn, "high", analog=False)

x_dem = sig.lfilter(b, a, chirp_signal)

# Apply filter forward and backward
x_dem_zero_phase = filtfilt(b, a, chirp_signal)
```


You can use either SciPy or the `control` package for control-system modeling and simulation.

### Control package

```python
import control as ct
import numpy as np

# Shared state-space matrices
a = np.array([[0.0, 1.0], [-1.0, -1.0]])
b = np.array([[0.0], [1.0]])
c = np.array([1.0, 0.0])
d = np.array([0.0])

sys = ct.ss(a, b, c, d)

# Transfer function and conversion
g = ct.tf(1, [1, 1, 1])
sys_from_tf = ct.ss(g)

# Convert from continuous to discrete
gd = ct.c2d(g, 0.01)
```

### SciPy control/simulation

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy import signal
from scipy.integrate import odeint

# Shared state-space matrices
a = np.array([[0.0, 1.0], [-1.0, -1.0]])
b = np.array([[0.0], [1.0]])
c = np.array([1.0, 0.0])
d = np.array([0.0])

sys = signal.StateSpace(a, b, c, d)
t_step, y_step = signal.step(sys)

# Simulate lsim with piecewise input
t = np.linspace(0, 100, 101)
u = np.zeros(len(t))
u[10:50] = 1.0
u[50:] = 2.0
t_out, y_out, x_out = signal.lsim(sys, u, t)


# ODE example: dy/dt = -k*y
def model(y, t):
    k = 0.3
    return -k * y


y0 = 5
t = np.linspace(0, 20)
y = odeint(model, y0, t)

plt.plot(t, y)
plt.xlabel("time")
plt.ylabel("y(t)")
plt.show()
```