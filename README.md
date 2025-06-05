# packagedelivery
Package delivery and route optimization with Traveling Salesman Problem

```
def optimal_delivery_route(points, route, availability=None, start_time=x, speed_vihicle=x, house_delivery_time_min=x):
    """
    :param points: the (x,y), which is the corrinates of the location on a axis
    :param route: the route that is followed
    :param availability: for when each household is avaliable, could be none, though, which just means avaliable all day
    :param start_time: the time the deliverer starts working. this is to take in factor to organize household avaliability in route
    :param speed_vihicle: speed of delivery vehicle in km/h
    :param house_delivery_time_min: fixed delivery time per stop in minutes\
    """
    if availability is None:
        availability = {}

    vihicle_speed_km_per_min = speed_vihicle / 60
    time = start_time
    arrival_times = []
    violations = []

    for i in range(len(route)):
        loc = route[i]
        arrival_times.append((loc, time))

        # Check for constraint violation
        if loc in availability:
            start_window, end_window = availability[loc]
            if not (start_window <= time <= end_window):
                violations.append((loc, time))


        time += house_delivery_time_min / 60


        if i < len(route) - 1:
            dist = np.linalg.norm(np.array(points[route[i+1]]) - np.array(points[loc]))
            time += dist /vihicle_speed_km_per_min / 60


    formatted_arrival_times = []
    for loc, t in arrival_times:
        hours = int(t)
        minutes = int((t - hours) * 60)
        formatted_arrival_times.append((loc, f"{hours:02d}:{minutes:02d}"))

    return formatted_arrival_times, violations

points = {
    "A": (x1, y1),
    "B": (x2, y2),
    "C": (x3, y3),
    "D": (x4, y4),
    "E": (x5, y5),
    "F": (x6, y6),
    "G": (x7, y7)
    ...
}

availability = {
    "A": (starttime, endtime),
    ...
}
```


