### 1. FCFS CPU Scheduling (with Arrival Times)

```python
n = int(input("No. of processes: "))
bt = list(map(int, input("Burst times: ").split()))
at = list(map(int, input("Arrival times: ").split()))
wt = [0]*n
tat = [0]*n
ct = [0]*n

p = sorted(range(n), key=lambda i: at[i])
t = 0
for i in p:
    t = max(t, at[i]) + bt[i]
    ct[i] = t
    tat[i] = ct[i] - at[i]
    wt[i] = tat[i] - bt[i]

print("\nPID\tAT\tBT\tWT\tTAT")
for i in range(n):
    print(i+1, "\t", at[i], "\t", bt[i], "\t", wt[i], "\t", tat[i])
print("\nAvg WT:", sum(wt)/n, " Avg TAT:", sum(tat)/n)
```

### 2. SJF CPU Scheduling (Non-Preemptive)

```python
n = int(input("No. of processes: "))
bt = list(map(int, input("Burst times: ").split()))
at = list(map(int, input("Arrival times: ").split()))
wt = [0]*n; tat = [0]*n; done=[0]*n
t = 0; done_cnt = 0

while done_cnt < n:
    idx = -1
    for i in range(n):
        if not done[i] and at[i] <= t:
            if idx == -1 or bt[i] < bt[idx]:
                idx = i
    if idx == -1:
        t += 1
        continue
    t += bt[idx]
    tat[idx] = t - at[idx]
    wt[idx] = tat[idx] - bt[idx]
    done[idx] = 1
    done_cnt += 1

print("\nPID\tAT\tBT\tWT\tTAT")
for i in range(n):
    print(i+1,"\t",at[i],"\t",bt[i],"\t",wt[i],"\t",tat[i])
print("\nAvg WT:", sum(wt)/n, " Avg TAT:", sum(tat)/n)
```

### 3. Priority CPU Scheduling (Non-Preemptive)

```python
n = int(input("No. of processes: "))
bt = list(map(int, input("Burst times: ").split()))
at = list(map(int, input("Arrival times: ").split()))
pr = list(map(int, input("Priorities: ").split()))
wt = [0]*n; tat = [0]*n; done=[0]*n
t = 0; done_cnt = 0

while done_cnt < n:
    idx = -1
    for i in range(n):
        if not done[i] and at[i] <= t:
            if idx == -1 or pr[i] < pr[idx]:
                idx = i
    if idx == -1:
        t += 1
        continue
    t += bt[idx]
    tat[idx] = t - at[idx]
    wt[idx] = tat[idx] - bt[idx]
    done[idx]=1
    done_cnt+=1

print("\nPID\tAT\tBT\tPR\tWT\tTAT")
for i in range(n):
    print(i+1,"\t",at[i],"\t",bt[i],"\t",pr[i],"\t",wt[i],"\t",tat[i])
print("\nAvg WT:", sum(wt)/n, " Avg TAT:", sum(tat)/n)
```

### 4. Round Robin CPU Scheduling

```python
n = int(input("No. of processes: "))
bt = list(map(int, input("Burst times: ").split()))
at = list(map(int, input("Arrival times: ").split()))
q = int(input("Time quantum: "))
rt = bt.copy(); wt=[0]*n; tat=[0]*n; t=0; done=[0]*n
from collections import deque
qz = deque()

while any(rt):
    for i in range(n):
        if at[i] <= t and rt[i] > 0 and i not in qz:
            qz.append(i)
    if not qz:
        t += 1
        continue
    i = qz.popleft()
    ex = min(q, rt[i])
    rt[i] -= ex
    t += ex
    for j in range(n):
        if at[j] <= t and rt[j] > 0 and j not in qz:
            qz.append(j)
    if rt[i]==0:
        tat[i]=t-at[i]
        wt[i]=tat[i]-bt[i]

print("\nPID\tAT\tBT\tWT\tTAT")
for i in range(n):
    print(i+1,"\t",at[i],"\t",bt[i],"\t",wt[i],"\t",tat[i])
print("\nAvg WT:", sum(wt)/n, " Avg TAT:", sum(tat)/n)
```

### 5. FIFO Page Replacement

```python
n = int(input("Enter number of frames: "))
pages = list(map(int, input("Enter page reference string: ").split()))
frames = []
faults = 0
hits = 0

print("\nPage\tFrames\t\tPage Fault")
for p in pages:
    if p not in frames:
        faults += 1
        if len(frames) < n:
            frames.append(p)
        else:
            frames.pop(0)
            frames.append(p)
        pf = "Yes"
    else:
        hits += 1
        pf = "No"
    print(p, "\t", frames, "\t", pf)

hit_ratio = hits / len(pages)
print("\nTotal Page Faults:", faults)
print("Total Page Hits:", hits)
print("Hit Ratio: {:.2f}".format(hit_ratio))
```

### 6. LRU Page Replacement

```python
n = int(input("Enter number of frames: "))
pages = list(map(int, input("Enter page reference string: ").split()))
frames = []
recent = []
faults = 0
hits = 0

print("\nPage\tFrames\t\tPage Fault")
for p in pages:
    if p not in frames:
        faults += 1
        if len(frames) < n:
            frames.append(p)
        else:
            lru = recent.pop(0)
            frames.remove(lru)
            frames.append(p)
        pf = "Yes"
    else:
        hits += 1
        pf = "No"
        recent.remove(p)
    recent.append(p)
    print(p, "\t", frames, "\t", pf)

hit_ratio = hits / len(pages)
print("\nTotal Page Faults:", faults)
print("Total Page Hits:", hits)
print("Hit Ratio: {:.2f}".format(hit_ratio))
```

### 7. Sequential File Allocation

```python
n = int(input("Enter total number of blocks: "))
f = int(input("Enter number of files: "))
mem = [0]*n
files = []

for i in range(f):
    start = int(input(f"\nEnter starting block of file {i+1}: "))
    length = int(input("Enter length of file: "))
    if start + length > n or any(mem[start:start+length]):
        print("Cannot allocate (out of range or already used)")
        continue
    alloc = list(range(start, start + length))
    for j in alloc:
        mem[j] = 1
    files.append((i+1, length, alloc))

print("\nFile\tLength\tBlocks Allocated")
for fno, length, alloc in files:
    print(f"{fno}\t{length}\t{alloc}")
```
