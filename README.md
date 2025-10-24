### 1. FCFS CPU Scheduling

```python
bt = [5, 3, 8]  # burst times
n = len(bt)
wt = [0]*n
tat = [0]*n

for i in range(1, n):
    wt[i] = wt[i-1] + bt[i-1]

for i in range(n):
    tat[i] = wt[i] + bt[i]

print("Waiting Times:", wt)
print("Turnaround Times:", tat)
print("Average Waiting Time:", sum(wt)/n)
print("Average Turnaround Time:", sum(tat)/n)
```

### 2. SJF (Non-Preemptive) CPU Scheduling

```python
bt = [6, 8, 7, 3]
n = len(bt)
p = list(range(n))
p.sort(key=lambda x: bt[x])
wt = [0]*n
tat = [0]*n

for i in range(1, n):
    wt[p[i]] = wt[p[i-1]] + bt[p[i-1]]

for i in range(n):
    tat[i] = wt[i] + bt[i]

print("Waiting Times:", wt)
print("Turnaround Times:", tat)
print("Average Waiting Time:", sum(wt)/n)
print("Average Turnaround Time:", sum(tat)/n)
```

### 3. Priority CPU Scheduling (Non-Preemptive)

```python
bt = [10, 1, 2, 1]
priority = [3, 1, 4, 2]  # lower number = higher priority
n = len(bt)
p = list(range(n))
p.sort(key=lambda x: priority[x])
wt = [0]*n
tat = [0]*n

for i in range(1, n):
    wt[p[i]] = wt[p[i-1]] + bt[p[i-1]]

for i in range(n):
    tat[i] = wt[i] + bt[i]

print("Waiting Times:", wt)
print("Turnaround Times:", tat)
print("Average Waiting Time:", sum(wt)/n)
print("Average Turnaround Time:", sum(tat)/n)
```

### 4. Round Robin CPU Scheduling

```python
bt = [5, 4, 2]
q = 2  # time quantum
n = len(bt)
rt = bt.copy()
t = 0
wt = [0]*n

while max(rt) > 0:
    for i in range(n):
        if rt[i] > 0:
            dt = min(rt[i], q)
            t += dt
            rt[i] -= dt
            if rt[i] == 0:
                wt[i] = t - bt[i]

tat = [wt[i] + bt[i] for i in range(n)]
print("Waiting Times:", wt)
print("Turnaround Times:", tat)
print("Average Waiting Time:", sum(wt)/n)
print("Average Turnaround Time:", sum(tat)/n)
```

### 5. FIFO Page Replacement

```python
pages = [1, 2, 3, 2, 4, 1, 5]
frames = 3
memory = []
faults = 0
hits = 0

for p in pages:
    if p not in memory:
        if len(memory) < frames:
            memory.append(p)
        else:
            memory.pop(0)
            memory.append(p)
        faults += 1
    else:
        hits += 1

print("Page Faults:", faults)
print("Page Hits:", hits)
print("Hit Ratio:", hits / len(pages))
```

### 6. LRU Page Replacement

```python
pages = [1, 2, 3, 2, 2, 5, 1]
frames = 3
memory = []
faults = 0
hits = 0

for p in pages:
    if p in memory:
        hits += 1
        memory.remove(p)
    else:
        faults += 1
        if len(memory) == frames:
            memory.pop(0)
        memory.append(p)

print("Page Faults:", faults)
print("Page Hits:", hits)
print("Hit Ratio:", hits / len(pages))
```

### 7. Sequential File Allocation

```python
def sequential_file_allocation():
    n = int(input("Enter number of files: "))
    files = []
    for i in range(n):
        start = int(input(f"\nEnter starting block for File {i+1}: "))
        length = int(input(f"Enter length for File {i+1}: "))
        files.append([f"File {i+1}", start, length])

    print("\nFile\tStart Block\tLength\tBlocks Occupied")
    for name, start, length in files:
        blocks = list(range(start, start + length))
        print(f"{name}\t{start}\t\t{length}\t{blocks}")

sequential_file_allocation()
```
