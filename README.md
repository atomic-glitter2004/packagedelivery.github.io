# Package Delivery Sample Calculation - Optimization code and output 配送路线样本计算 - 优化代码和输出
**Project by Ash** 

**Ash的项目** 

Package delivery and route optimization that basically uses the fundementaln logic link in the classic NP-Hard problem: travelling salesman problem
这个包裹递送和路线优化过程基本与经典 NP-Hard 问题中的旅行推销员问题一样

>This page's code is how I calculated the optimal route for example
>
>本页的代码是我计算最优路线的示例

## Input code 代码部分
```
import numpy as np

def optimal_delivery_route(points, route, availability=None, start_time=9.0, speed_vihicle=15,
                           house_delivery_time_min=10):

  :param these values are just adapted to fit our theroretical circumstance, could be changed and adapted to spesific circumstnace

    if availability is None:
        availability = {}

    vihicle_speed_km_per_min = speed_vihicle / 60
    time = start_time
    arrival_times = []
    violations = []

    for i in range(len(route)):
        loc = route[i]
        arrival_times.append((loc, time))

        if loc in availability:
            start_window, end_window = availability[loc]
            if not (start_window <= time <= end_window):
                violations.append((loc, time))

        time += house_delivery_time_min / 60

        if i < len(route) - 1:
            dist = np.linalg.norm(np.array(points[route[i + 1]]) - np.array(points[loc]))
            time += dist / vihicle_speed_km_per_min / 60

    formatted_arrival_times = []
    for loc, t in arrival_times:
        hours = int(t)
        minutes = int((t - hours) * 60)
        formatted_arrival_times.append((loc, f"{hours:02d}:{minutes:02d}"))

    return formatted_arrival_times, violations


x1, y1 = 1, 3
x2, y2 = 1, 5
x3, y3 = 3, 4
x4, y4 = 5, 2
x5, y5 = 6, 6
x6, y6 = 7, 1
x7, y7 = 8, 6

points = {
    "A": (x1, y1),
    "B": (x2, y2),
    "C": (x3, y3),
    "D": (x4, y4),
    "E": (x5, y5),
    "F": (x6, y6),
    "G": (x7, y7)
}

availability = {
    "C": (10.0, 11.0),  
    "F": (9.0, 9.5)  
}


example_route = ["F", "A", "B", "D", "C", "E", "G"]


arrival_times, violations = optimal_delivery_route(points, example_route, availability)

print("Arrival Times At Position:")
for stop, t in arrival_times:
    print(f"{stop}: {t}")

print("\nUnwanted arrival out of time:")
for stop, t in violations:
    print(f"{stop} violated at {t:.2f}h")

```

## And here is my outputs 输出部分: 
```
Output:
Arrival Times At Position:
F: 09:00
A: 09:35
B: 09:53
D: 10:23
C: 10:44
E: 11:09
G: 11:27

Unwanted arrival out of time:

```


