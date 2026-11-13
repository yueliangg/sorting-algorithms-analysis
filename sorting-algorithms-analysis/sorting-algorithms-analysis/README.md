# Sorting Algorithms: Performance, Complexity and Trade-offs

An experimental comparison of four sorting algorithms (Bubble, Merge, Quick and Heap Sort). Each algorithm was measured by comparison count, input sensitivity and memory use. The project also includes an individual investigation into making Merge Sort in-place.

**Key insight:** theoretical Big-O alone does not decide which algorithm is best. The structure of the input data, the memory limits and the need for worst-case guarantees all change the answer.

---

## Project Overview

| | |
|---|---|
| **Type** | Algorithm analysis and benchmarking (team of 2, plus individual investigations) |
| **Algorithms** | Bubble Sort, Merge Sort, Quick Sort (median-of-three, three-way partitioning), Heap Sort |
| **Measures** | Comparison counts, normalised growth, element movements, peak memory, runtime |
| **Datasets** | Sizes from 5 to 100,000, plus predefined cases (sorted, reverse, almost sorted, all identical, few unique) |

---

## Tech Stack

Python, NumPy, pandas, Matplotlib, tracemalloc, Jupyter Notebook

---

## Part A: Bubble Sort vs Merge Sort

Both algorithms were instrumented to count comparisons, checked for correctness, and benchmarked across increasing dataset sizes.

| n | Bubble Sort | Merge Sort |
|---|---|---|
| 100 | 8,514 | 534 |
| 10,000 | 98.8 million | 120,000 |
| 100,000 | 9.97 billion | 1.54 million |

- At n = 100,000, Bubble Sort made **about 6,490 times more comparisons** than Merge Sort.
- Normalised counts stayed roughly constant at large n, which confirms **O(n²)** growth for Bubble Sort and **O(n log n)** for Merge Sort.
- **Bubble Sort is highly sensitive to input order.** Its early-termination check needs only 999 comparisons on sorted data but 999,000 on reverse-sorted data. Merge Sort stayed consistent at around 5,000 comparisons for every arrangement.

---

## Part B: Quick Sort vs Heap Sort

| Input | Quick Sort | Heap Sort |
|---|---|---|
| Random (n = 100,000) | **2,813,769** | 3,019,828 |
| Already sorted | 38,824 | **17,583** |
| Reverse sorted | 29,884 | **15,965** |
| All identical | **2,003** | 2,994 |
| Few unique values | **3,937** | 14,177 |

- On random data, both algorithms grew at roughly O(n log n). Quick Sort used **about 6.8% fewer comparisons** at n = 100,000.
- **Three-way Quick Sort is strongest when there are many duplicates.** It used about 72% fewer comparisons on "few unique" data, because values equal to the pivot are removed from further recursion.
- **Heap Sort is more consistent.** It did better on ordered inputs and guarantees O(n log n) worst-case time with O(1) extra memory.

**Conclusion:** there is no universal winner. Choose Quick Sort for strong average performance and duplicate-heavy data, and Heap Sort when worst-case predictability or limited memory matters.

---

## Individual Investigation: Making Merge Sort In-Place

*Thawdar Lin Wai*

Standard Merge Sort needs an O(n) temporary buffer for merging. This investigation built a **bottom-up, shift-based in-place variant** that uses only O(1) auxiliary memory. It then measured what that memory saving costs.

| Measure | Standard Merge Sort | In-Place Variant |
|---|---|---|
| Auxiliary space | O(n) | O(1) |
| Peak memory at n = 16,384 | 263.9 KiB | **0.29 KiB (about 900 times less)** |
| Element movements, random input (n = 2,048) | 22,528 | 1,055,226 |
| Runtime, random input (n = 2,048) | 8.12 ms | 170.90 ms (21 times slower) |
| Runtime, reverse sorted (n = 2,048) | 7.87 ms | 271.25 ms (34 times slower) |
| Worst-case time | O(n log n) | O(n²) |

- Both versions made **identical comparison counts**, so the extra cost came entirely from **data movement**. Disordered inputs require many elements to be shifted.
- The in-place variant stays competitive on already-sorted and almost-sorted data, where little shifting is needed.
- **Takeaway:** "in-place" is a memory property, not a guarantee of better performance. The variant suits memory-constrained environments where data is mostly ordered. Standard Merge Sort is the better choice when predictable runtime matters.

Poster: [`in_place_merge_sort_poster.pdf`](in_place_merge_sort_poster.pdf)

---

## Individual Investigation: Sorting Stability

*Toh Ke Ting*

This investigation explains why stability matters (keeping equal-key records in their original order). It turns an unstable Selection Sort into a stable, shifting-based version and measures the extra movement cost of preserving stability. The full analysis is in the notebook.

---

## Repository Structure

```
├── sorting_algorithms_analysis.ipynb   # Full implementation, experiments and analysis
├── sorting_algorithms_slides.pdf       # Presentation slides
├── in_place_merge_sort_poster.pdf      # Individual investigation poster
└── README.md
```

---

## How to Run

```bash
git clone <this-repo-url>
cd <repo-folder>
pip install numpy pandas matplotlib
jupyter notebook sorting_algorithms_analysis.ipynb
```

Then select **Kernel > Restart & Run All**. The data generator uses a fixed seed (`random.seed(42)`), so results are reproducible. The largest Bubble Sort benchmarks can take several minutes.

---

## Team

Developed by Toh Ke Ting and Thawdar Lin Wai for the Data Structures and Algorithms module at Singapore Polytechnic.
