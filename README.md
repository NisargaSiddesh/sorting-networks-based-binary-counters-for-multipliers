* **Version 1** → Optimized for **average swap count**
* **Version 2** → Optimized for **worst-case swap count**

# Sorting-Network-Based Binary Counters for Multipliers

## 📌 Overview

This repository contains **Verilog implementations** of sorting networks and binary counters integrated into high-speed multiplier architectures.
The project explores **swap-based optimization** in sorting networks and its effect on **power, delay, and area** when used in (7,3) and (15,4) counters inside 16×16 multipliers.

We implement:

* **2021 Design:** From Guo & Li, *IEEE TVLSI 2021*
* **2023 Design:** From Vidyadhar et al., *IEEE 2023*
* **Version 1:** Optimized for **average swap count**
* **Version 2:** Optimized for **worst-case swap count**

---


## ⚙️ Methodology

### 1. Sorting Networks

Sorting networks arrange bits so that all `1`s are shifted toward the MSB side.
We use a **Compare-And-Exchange (CAE)** block for each comparator:

```verilog
module CAE(input a, b, output min, max);
    assign min = a & b;
    assign max = a | b;
endmodule
```

> **Note:** The CAE block computes outputs for every input combination, even if swapping is unnecessary.
> This means reduced swap count does **not** directly translate to reduced switching activity.

---

### 2. Binary Counters

* **(7,3) Counter:** Uses 4-input and 3-input sorters.
* **(15,4) Counter:** Uses 8-input and 7-input sorters.
* Sorted outputs are passed through one-hot encoding logic to generate the count.

---

### 3. Multiplier Integration

Partial products of a 16×16 multiplier are reduced column-wise using the counters.
This improves **speed** by parallelizing additions in the partial product reduction stage.

---

## 📊 Key Observations

* **Goal:** Reduce switching power by minimizing swap operations in the network.
* **Finding:** Version 1 and Version 2 have slightly lower power than 2021/2023 designs,
  but the reduction is not as significant as expected.
* **Reason:** The CAE’s unconditional operation leads to constant transitions regardless of swap necessity.
