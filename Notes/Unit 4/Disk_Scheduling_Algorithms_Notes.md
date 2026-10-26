# Disk Scheduling Algorithms – Complete Notes (Theory + Numericals)

**Lecture 28 plan:** Need for Disk Scheduling → FCFS → SSTF → SCAN & C-SCAN → LOOK & C-LOOK

---

## 1. Need for Disk Scheduling

### 1.1 Disk structure (quick recap)
- A hard disk has **platters**, each with **tracks** (concentric circles), divided into **sectors**.
- All tracks at the same position across platters form a **cylinder**.
- The **read/write head** sits on an arm that moves in/out across the tracks.

### 1.2 Components of disk access time
```
Disk Access Time = Seek Time + Rotational Latency + Transfer Time
```
| Component | Meaning | Controlled by scheduling? |
|---|---|---|
| **Seek time** | Time to move the head to the required track/cylinder | **YES – the dominant cost** |
| **Rotational latency** | Time for the required sector to rotate under the head (avg = ½ rotation) | No |
| **Transfer time** | Time to actually read/write data | No |

### 1.3 Why do we need scheduling?
- Many processes issue I/O requests at the same time → a **queue of requests** builds up for the disk.
- Disk is a slow mechanical device; the head movement (seek) is the slowest part.
- If requests are served in a random order, the head wastes time swinging back and forth.
- **Goal of disk scheduling:** choose the order of servicing requests so that
  1. **Total seek time / head movement is minimum**
  2. **Throughput is high**
  3. **Average response (waiting) time is low**
  4. **Variance in response time is small** (fairness, no starvation)

### 1.4 Key terms used in numericals
| Term | Meaning |
|---|---|
| Request queue | List of track numbers to be serviced, e.g. `98, 183, 37, 122, 14, 124, 65, 67` |
| Head position / initial head | Where the head currently is, e.g. `53` |
| Disk size / track range | e.g. `0–199` (200 tracks) |
| Seek distance | `|current − next|` (number of tracks crossed) |
| **Total head movement (THM)** | Sum of all seek distances – the answer asked in most numericals |
| Direction | Which way the head is moving (towards 0 = *left/down*, towards max = *right/up*) |

### 1.5 Universal formulas
```
Seek distance between two tracks = |A − B|
Total Head Movement (THM)        = Σ |Ti − Ti-1|
Average Seek Length              = THM / Number of requests
Total Seek Time                  = THM × (seek time per track)
```
> Example: if THM = 640 tracks and each track takes 1 ms → Seek time = 640 ms.

### 1.6 Standard method to solve ANY disk scheduling numerical (steps)
1. Write the **initial head**, **queue**, **disk range**, and **direction**.
2. Decide the **order of service** using the algorithm's rule.
3. Write the order as a path: `H → a → b → c → …`
4. For every hop write `|from − to| = distance`.
5. **Add all distances** → THM.
6. (If asked) Average seek = THM / number of requests.
7. (If asked) Draw the head-movement diagram (track number on x-axis, order on y-axis).

---

## 2. FCFS (First Come First Served)

### 2.1 Theory
- Requests are served **in the exact order they arrive** in the queue.
- No re-ordering, no optimisation.
- **Advantages:** simple, easy to implement, **fair**, no starvation.
- **Disadvantages:** large head movements (wild swings), **poor performance**, high average seek time; does not try to minimise seek.

### 2.2 Numerical 1
**Queue:** `98, 183, 37, 122, 14, 124, 65, 67`  **Head:** `53`  **Disk:** `0–199`

**Order of service:** `53 → 98 → 183 → 37 → 122 → 14 → 124 → 65 → 67`

| Move | Calculation | Distance |
|---|---|---|
| 53 → 98 | \|98 − 53\| | 45 |
| 98 → 183 | \|183 − 98\| | 85 |
| 183 → 37 | \|37 − 183\| | 146 |
| 37 → 122 | \|122 − 37\| | 85 |
| 122 → 14 | \|14 − 122\| | 108 |
| 14 → 124 | \|124 − 14\| | 110 |
| 124 → 65 | \|65 − 124\| | 59 |
| 65 → 67 | \|67 − 65\| | 2 |

**THM = 45 + 85 + 146 + 85 + 108 + 110 + 59 + 2 = 640 tracks**
**Average seek length = 640 / 8 = 80 tracks**

### 2.3 Numerical 2
**Queue:** `82, 170, 43, 140, 24, 16, 190`  **Head:** `50`  **Disk:** `0–199`

`50→82 (32), 82→170 (88), 170→43 (127), 43→140 (97), 140→24 (116), 24→16 (8), 16→190 (174)`

**THM = 32 + 88 + 127 + 97 + 116 + 8 + 174 = 642 tracks**

---

## 3. SSTF (Shortest Seek Time First)

### 3.1 Theory
- From the **current head position**, always pick the pending request that is **closest** (minimum seek distance).
- It is a **greedy** algorithm (similar to SJF in CPU scheduling).
- **Advantages:** much lower THM than FCFS, better throughput, lower average response time.
- **Disadvantages:**
  - **Starvation** – far-away requests may wait indefinitely if new nearby requests keep arriving.
  - High variance in response time; not optimal overall (greedy ≠ globally best).
  - Overhead of calculating the closest request every time.
- **Tie-break rule:** if two requests are equally close, pick any (usually the one in the current direction / lower track; state your assumption).

### 3.2 How to solve (step by step)
1. Put the head at the starting track.
2. Compute `|head − x|` for **every pending** request.
3. Choose the smallest; add it to the path; move the head there; remove it from the pending list.
4. Repeat until the list is empty. Then add all distances.

### 3.3 Numerical 1
**Queue:** `98, 183, 37, 122, 14, 124, 65, 67`  **Head:** `53`

| Step | Head at | Closest request (distance) | Pending after |
|---|---|---|---|
| 1 | 53 | **65** (12) | 98,183,37,122,14,124,67 |
| 2 | 65 | **67** (2) | 98,183,37,122,14,124 |
| 3 | 67 | **37** (30) *(98 is 31)* | 98,183,122,14,124 |
| 4 | 37 | **14** (23) | 98,183,122,124 |
| 5 | 14 | **98** (84) | 183,122,124 |
| 6 | 98 | **122** (24) | 183,124 |
| 7 | 122 | **124** (2) | 183 |
| 8 | 124 | **183** (59) | – |

**Path:** `53 → 65 → 67 → 37 → 14 → 98 → 122 → 124 → 183`

**THM = 12 + 2 + 30 + 23 + 84 + 24 + 2 + 59 = 236 tracks**
**Average seek = 236 / 8 = 29.5 tracks**

*(Compare: FCFS = 640. SSTF is almost 3× better.)*

### 3.4 Numerical 2
**Queue:** `82, 170, 43, 140, 24, 16, 190`  **Head:** `50`

- 50 → **43** (7)  [others: 82→32, 24→26, 16→34]
- 43 → **24** (19)
- 24 → **16** (8)
- 16 → **82** (66)
- 82 → **140** (58)
- 140 → **170** (30)
- 170 → **190** (20)

**THM = 7 + 19 + 8 + 66 + 58 + 30 + 20 = 208 tracks**

---

## 4. SCAN (Elevator Algorithm)

### 4.1 Theory
- Head moves in **one direction**, servicing every request on the way, until it reaches the **physical end of the disk** (track 0 or max track, e.g. 199).
- After touching the end it **reverses direction** and services the remaining requests on the way back.
- Called the **elevator algorithm** (lift goes to top/bottom, picking people on the way).
- **Advantages:** no starvation, better than FCFS, good for heavy load, lower variance than SSTF.
- **Disadvantages:** head goes to the end even if no request is there (wasted movement); requests just behind the head wait for a long time; **non-uniform waiting time** (tracks in the middle get serviced more often than the edges).
- **Always need:** the **direction** and the **disk size** (to know the end track).

### 4.2 Shortcut formulas (disk range 0 to `M`, head `H`)
Let `Lmin` = smallest request, `Lmax` = largest request, `M` = last track (e.g. 199).

| Direction first | Formula for THM |
|---|---|
| Towards **0** (left) first, then right | `H + Lmax`  *(H → 0, then 0 → Lmax)* |
| Towards **M** (right) first, then left | `(M − H) + (M − Lmin)`  *(H → M, then M → Lmin)* |

> Valid when there are requests on both sides of the head.

### 4.3 Numerical 1 – direction towards 0 (left) first
**Queue:** `98, 183, 37, 122, 14, 124, 65, 67`  **Head:** `53`  **Disk:** `0–199`

**Step 1:** Sort requests: `14, 37, | 53 (head) | 65, 67, 98, 122, 124, 183`
**Step 2:** Go left: 53 → 37 → 14 → **0** (end). Then reverse and go right: 0 → 65 → 67 → 98 → 122 → 124 → 183.

| Move | Distance |
|---|---|
| 53 → 37 | 16 |
| 37 → 14 | 23 |
| 14 → 0 | 14 |
| 0 → 65 | 65 |
| 65 → 67 | 2 |
| 67 → 98 | 31 |
| 98 → 122 | 24 |
| 122 → 124 | 2 |
| 124 → 183 | 59 |

**THM = 16 + 23 + 14 + 65 + 2 + 31 + 24 + 2 + 59 = 236 tracks**
**Check with formula:** `H + Lmax = 53 + 183 = 236` ✔

### 4.4 Numerical 1 (b) – direction towards 199 (right) first
**Path:** `53 → 65 → 67 → 98 → 122 → 124 → 183 → 199 → 37 → 14`

| Move | Distance |
|---|---|
| 53 → 65 | 12 |
| 65 → 67 | 2 |
| 67 → 98 | 31 |
| 98 → 122 | 24 |
| 122 → 124 | 2 |
| 124 → 183 | 59 |
| 183 → **199** | 16 |
| 199 → 37 | 162 |
| 37 → 14 | 23 |

**THM = 12 + 2 + 31 + 24 + 2 + 59 + 16 + 162 + 23 = 331 tracks**
**Check:** `(199 − 53) + (199 − 14) = 146 + 185 = 331` ✔

### 4.5 Numerical 2
**Queue:** `82, 170, 43, 140, 24, 16, 190`  **Head:** `50`  **Disk:** `0–199`

- **Towards 0 first:** `50 → 43 → 24 → 16 → 0 → 82 → 140 → 170 → 190`
  THM = `50 + 190 = 240` (7 + 19 + 8 + 16 + 82 + 58 + 30 + 20 = 240 ✔)
- **Towards 199 first:** `50 → 82 → 140 → 170 → 190 → 199 → 43 → 24 → 16`
  THM = `(199 − 50) + (199 − 16) = 149 + 183 = 332`

---

## 5. C-SCAN (Circular SCAN)

### 5.1 Theory
- Head moves in **one fixed direction only** (service direction), servicing requests on the way.
- On reaching the **end of the disk**, it **jumps back immediately to the other end** (return trip) **without servicing anything** on the way back, then continues in the same direction.
- Treats the disk as a **circular list** of tracks.
- **Advantages:** **uniform waiting time** (more fair than SCAN), no starvation, good under heavy load.
- **Disadvantages:** the return jump (e.g. 199 → 0) is extra head movement; head still goes to the physical ends even if no requests there.
- **Important exam note:** whether the **return jump distance is counted** in THM depends on the question/teacher. Standard textbook (Galvin) and most university answers **count it** (199). Some count only service movement. **Write your assumption clearly.** Below both are shown.

### 5.2 Numerical 1 – direction towards 199 (right)
**Queue:** `98, 183, 37, 122, 14, 124, 65, 67`  **Head:** `53`  **Disk:** `0–199`

**Path:** `53 → 65 → 67 → 98 → 122 → 124 → 183 → 199 ⟹ jump to 0 ⟹ 14 → 37`

| Move | Distance |
|---|---|
| 53 → 183 (services 65, 67, 98, 122, 124, 183) | 130 |
| 183 → 199 | 16 |
| **199 → 0 (return jump)** | **199** |
| 0 → 14 | 14 |
| 14 → 37 | 23 |

- **THM (counting the jump) = 130 + 16 + 199 + 14 + 23 = 382 tracks**
- THM (not counting the jump) = 130 + 16 + 14 + 23 = 183 tracks

### 5.3 Numerical 1 (b) – direction towards 0 (left)
**Path:** `53 → 37 → 14 → 0 ⟹ jump to 199 ⟹ 183 → 124 → 122 → 98 → 67 → 65`

| Move | Distance |
|---|---|
| 53 → 37 → 14 → 0 | 16 + 23 + 14 = 53 |
| **0 → 199 (jump)** | **199** |
| 199 → 183 | 16 |
| 183 → 124 | 59 |
| 124 → 122 | 2 |
| 122 → 98 | 24 |
| 98 → 67 | 31 |
| 67 → 65 | 2 |

- **THM (with jump) = 53 + 199 + 16 + 59 + 2 + 24 + 31 + 2 = 386 tracks**
- THM (without jump) = 187 tracks

### 5.4 Numerical 2
**Queue:** `82, 170, 43, 140, 24, 16, 190`  **Head:** `50`  **Direction:** right  **Disk:** `0–199`

Path: `50 → 82 → 140 → 170 → 190 → 199 ⟹ 0 → 16 → 24 → 43`
- 50 → 199 = 149; jump 199 → 0 = 199; 0 → 16 = 16; 16 → 24 = 8; 24 → 43 = 19
- **THM (with jump) = 149 + 199 + 16 + 8 + 19 = 391 tracks**; without jump = 192.

---

## 6. LOOK (Industry insight)

### 6.1 Theory
- An **improved SCAN**. The head moves in one direction only **as far as the last (farthest) request** in that direction – it does **not** go to the physical end of the disk.
- It then **looks ahead**: if no more requests that way, it **reverses** and services the rest.
- **Advantages:** removes the useless travel to the disk ends, so **less head movement than SCAN**, better response time.
- **Disadvantages:** still non-uniform wait for requests near the edges.
- **Industry insight:** real operating systems and disk controllers use LOOK-type (or C-LOOK) logic, because going to the physical edge with no pending request is pure waste. Linux's older *elevator* I/O schedulers were based on this idea (and SSDs now often use simple policies like *noop/none* since they have no moving head).

### 6.2 Shortcut formulas (head `H`, `Lmin`, `Lmax`)
| Direction first | THM |
|---|---|
| Towards **right (larger)** first | `(Lmax − H) + (Lmax − Lmin)` |
| Towards **left (smaller)** first | `(H − Lmin) + (Lmax − Lmin)` |

### 6.3 Numerical 1 – direction right (towards larger)
**Queue:** `98, 183, 37, 122, 14, 124, 65, 67`  **Head:** `53`

**Path:** `53 → 65 → 67 → 98 → 122 → 124 → 183 (last request – reverse here) → 37 → 14`

| Move | Distance |
|---|---|
| 53 → 183 (all requests on the right) | 130 |
| 183 → 37 | 146 |
| 37 → 14 | 23 |

**THM = 130 + 146 + 23 = 299 tracks**
**Check:** `(183 − 53) + (183 − 14) = 130 + 169 = 299` ✔

### 6.4 Numerical 1 (b) – direction left (towards smaller)
**Path:** `53 → 37 → 14 (reverse) → 65 → 67 → 98 → 122 → 124 → 183`

| Move | Distance |
|---|---|
| 53 → 37 → 14 | 16 + 23 = 39 |
| 14 → 65 | 51 |
| 65 → 67 → 98 → 122 → 124 → 183 | 2 + 31 + 24 + 2 + 59 = 118 |

**THM = 39 + 51 + 118 = 208 tracks**
**Check:** `(53 − 14) + (183 − 14) = 39 + 169 = 208` ✔

### 6.5 Numerical 2 (`82, 170, 43, 140, 24, 16, 190`, head 50)
- Right first: `(190 − 50) + (190 − 16) = 140 + 174 = 314`
- Left first: `(50 − 16) + (190 − 16) = 34 + 174 = 208`

---

## 7. C-LOOK (Circular LOOK)

### 7.1 Theory
- The **circular** version of LOOK (as C-SCAN is to SCAN).
- Head moves in one direction up to the **last request**, then **jumps directly to the first (farthest) request at the other end** – *not* to the physical disk end – and continues in the **same direction**.
- **Advantages:** least wasted movement, uniform waiting time, no starvation – the best practical choice among these for heavy load.
- **Disadvantages:** the jump (from last request to first request at the other end) is still a long head movement; slightly more complex logic.
- Again, **state whether the jump is counted** (most textbooks count it).

### 7.2 Numerical 1 – direction right
**Queue:** `98, 183, 37, 122, 14, 124, 65, 67`  **Head:** `53`

**Path:** `53 → 65 → 67 → 98 → 122 → 124 → 183 ⟹ jump to 14 ⟹ 37`

| Move | Distance |
|---|---|
| 53 → 183 | 130 |
| **183 → 14 (jump)** | **169** |
| 14 → 37 | 23 |

- **THM (with jump) = 130 + 169 + 23 = 322 tracks**
- THM (without jump) = 130 + 23 = 153 tracks

### 7.3 Numerical 1 (b) – direction left
**Path:** `53 → 37 → 14 ⟹ jump to 183 ⟹ 124 → 122 → 98 → 67 → 65`

| Move | Distance |
|---|---|
| 53 → 37 → 14 | 39 |
| **14 → 183 (jump)** | **169** |
| 183 → 124 | 59 |
| 124 → 122 | 2 |
| 122 → 98 | 24 |
| 98 → 67 | 31 |
| 67 → 65 | 2 |

- **THM (with jump) = 39 + 169 + 59 + 2 + 24 + 31 + 2 = 326 tracks**
- THM (without jump) = 157 tracks

### 7.4 Numerical 2 (`82, 170, 43, 140, 24, 16, 190`, head 50, right)
Path: `50 → 82 → 140 → 170 → 190 ⟹ 16 → 24 → 43`
- 50 → 190 = 140; jump 190 → 16 = 174; 16 → 24 → 43 = 8 + 19 = 27
- **THM (with jump) = 140 + 174 + 27 = 341**; without jump = 167.

---

## 8. Comparison – Same Problem, All Algorithms
**Queue:** `98, 183, 37, 122, 14, 124, 65, 67`  **Head:** `53`  **Disk:** `0–199`

| Algorithm | Direction | THM (tracks) | Avg seek (÷8) |
|---|---|---|---|
| FCFS | – | **640** | 80 |
| SSTF | – | **236** | 29.5 |
| SCAN | left first | **236** | 29.5 |
| SCAN | right first | **331** | 41.4 |
| C-SCAN (jump counted) | right | **382** | 47.75 |
| C-SCAN (jump counted) | left | **386** | 48.25 |
| LOOK | right first | **299** | 37.4 |
| LOOK | left first | **208** | 26 |
| C-LOOK (jump counted) | right | **322** | 40.25 |
| C-LOOK (jump counted) | left | **326** | 40.75 |

### Feature comparison
| Feature | FCFS | SSTF | SCAN | C-SCAN | LOOK | C-LOOK |
|---|---|---|---|---|---|---|
| Rule | Arrival order | Nearest first | Sweep to disk end & back | Sweep one way, jump back | Sweep to last request & back | Sweep one way to last request, jump to first |
| Goes to disk end? | No | No | **Yes** | **Yes** | No | No |
| Starvation | No | **Yes** | No | No | No | No |
| Waiting-time uniformity | Fair | Poor | Medium | **Uniform** | Medium | **Uniform** |
| Performance | Worst | Good (greedy) | Good | Good under heavy load | Better than SCAN | Better than C-SCAN |
| Extra info needed | – | – | Direction, disk size | Direction, disk size | Direction | Direction |

---

## 9. Practice Problem (with full solution)

**Queue:** `55, 58, 39, 18, 90, 160, 150, 38, 184`  **Head:** `100`  **Disk:** `0–199`  **Direction:** towards larger tracks

Sorted: `18, 38, 39, 55, 58, 90 | 100 (H) | 150, 160, 184`

| Algorithm | Path (summary) | THM |
|---|---|---|
| FCFS | 100→55→58→39→18→90→160→150→38→184 = 45+3+19+21+72+70+10+112+146 | **498** |
| SSTF | 100→90→58→55→39→38→18→150→160→184 = 10+32+3+16+1+20+132+10+24 | **248** |
| SCAN (right first) | 100→184→199→18 : `(199−100)+(199−18)` = 99+181 | **280** |
| C-SCAN (right, jump counted) | 100→199 (99) + jump (199) + 0→90 (90) | **388** |
| LOOK (right first) | `(184−100)+(184−18)` = 84+166 | **250** |
| C-LOOK (right, jump counted) | 100→184 (84) + jump 184→18 (166) + 18→90 (72) | **322** |

*(If direction were left: SCAN = 100 + 184 = 284, LOOK = (100−18)+(184−18) = 248.)*

---

## 10. Exam Tips & Common Mistakes
1. **Read the direction** – if not given, state your assumption (usually "towards higher/lower track").
2. **SCAN/C-SCAN go to the disk END (0 or max)**; **LOOK/C-LOOK stop at the last request.** This is the #1 mistake.
3. Disk of `N` tracks is numbered **0 to N−1** (e.g., 200 tracks → 0–199).
4. In **C-SCAN/C-LOOK**, the return jump is **not a service**, but most textbooks **count its distance** in THM – mention it in the answer.
5. In SSTF, recompute distances **from the current head** at every step, not from the start.
6. Always verify by the shortcut formula (SCAN/LOOK) – it catches arithmetic errors.
7. If the head is **already at an end** (e.g., at 0 moving towards 0), SCAN reverses immediately.
8. Convert to **time** if asked: `Seek time = THM × time per track`.
9. Order of efficiency (typical): **FCFS (worst) < SCAN < C-SCAN ≈ LOOK < C-LOOK / SSTF** (SSTF best average but risks starvation).

---

## 11. Quick Revision Sheet
- **FCFS** – first come, first served. Simple, fair, slow.
- **SSTF** – closest next. Fast, but **starvation**.
- **SCAN** – elevator; go to **end**, reverse.
- **C-SCAN** – go to end, **jump** to other end, continue same direction.
- **LOOK** – SCAN but reverse at **last request**.
- **C-LOOK** – C-SCAN but jump from **last request to first request**.
- **THM = Σ |difference|**; **Avg seek = THM / n**.
