
|    NRP     |      Name      |
| :--------: | :------------: |
| 5025231161 | Muhammad Rizqy Hidayat |
| 5025231075 | Reino Yuris Kusumanegara |
| 5025231082 | Muhammad Hanif Fakriansyah |
| 5025231011 | Fazle Robby Pratama |

# Graph Theory Final Project

</div>

## Introduction
In today's fast-paced world, deciding where to eat can often be a time-consuming process. Whether it's choosing between different types of cuisine, balancing budget constraints, or finding a location that's convenient for everyone, the challenge of picking a restaurant that satisfies all criteria can become overwhelming. To address this issue, we propose a Restaurant Recommendation System that utilizes advanced search algorithms to provide personalized, optimal restaurant suggestions based on user preferences, location, and other key factors. By leveraging methods such as informed search, local search, and constraint satisfaction, this system aims to simplify the decision-making process, delivering recommendations that are both efficient and tailored to individual or group needs.

## Background
When individuals or groups plan to eat out, there will always debate in choosing where they want to eat due to the vary preferences of itselfs. They must also consider the constraints such as budget, approximated cost, travel to distance, operational hours and many more. The abundance amount of restaurant options merge with how limited time and varying preferences create this problem.
Hence, in here we develop a Restaurant Recommendation system to help people that still in sea and want to choose their preferences of restaurant quickly. By using search methodology such as Uninformed, Informed, Local, and Adversarial search.

## Problem and Graph Theory Implementation
Generally, when an individual or group of people wants to eat out, they often face difficulty choosing a restaurant that matches their preferences, budget, and location. This problem becomes more complex when there are multiple people with different preferences, such as:<br>
- Cuisine type (e.g., fast food, traditional).
- Budget constraints.
- Proximity and ratings.

### Graph Theory Approach
1. **Nodes:** Represent restaurants.
2. **Edges:** Represent distances between restaurants or between the user’s location and the restaurants.
3. **Algorithm:** Breadth-First Search (BFS) is used to find the shortest path (distance) to restaurants that meet the criteria.

By implementing Graph Theory, the system models the problem efficiently, enabling fast computation of optimal routes and recommendations.

## Uniqueness
#### 1. Multiple User Preferences
When several people are involved, each with their own food preferences, budget, and location, the system can combine and balance these multiple preferences to provide a recommendation that satisfies the majority of users. This is particularly useful for situations like family outings, group lunches, or meetings where diverse preferences must be considered. Not only for that, individuals can involve themself in this system too, of course. 
#### 2. Distance and time efficiency calculation
By leveraging search algorithms like A* search, the system can prioritize nearby restaurants that minimize travel time, which is crucial for users with limited time, such as lunch breaks. It ensures that the recommendation not only fits the food preferences but also accounts for time efficiency. 
#### 3. Quick decision-making
This recommendation system uses a combination of uninformed, informed, and local search algorithms to find optimal solutions. Algorithms like A* help find the best routes to nearby restaurants and can be used to refine restaurant choices based on the users' preferences, adjusting for cost, location, and group size. By using multiple algorithms, the system ensures both speed and accuracy in producing recommendations.

## Methodology
For our project, we have chosen to develop a Restaurant Recommendation System. With the vast number of dining options available, finding the perfect restaurant that matches a user’s preferences can be a challenging task. Our system aims to address this by intelligently recommending restaurants based on key factors such as location, cuisine, price range, and user reviews. By employing advanced search algorithms like Breadth-First Search (BFS), A Search*, and Genetic Algorithms, our system can provide users with personalized, optimal dining recommendations that best suit their tastes and needs.

## Summary
The system successfully recommends restaurants by considering user preferences and location. Using BFS ensures that recommendations are optimal in terms of distance and criteria, while filtering options based on user-defined parameters like budget and cuisine type.

## Key Observations 
- BFS is effective for finding nearby restaurants, but the time complexity increases with the number of nodes (restaurants).
- The system balances multiple user preferences effectively using search algorithms.
- The graph visualization aids in understanding the recommendation paths

## Revision
Our initial code was working more like a greedy search instead of a true Breadth-First Search (BFS). We also realized, BFS does not inherently prioritize the nearest restaurants to the user but explores nodes level by level. To address this and ensure that the closest restaurants are prioritized while maintaining the existing output, we revised the implementation to use Best-First Search (BestFS). BestFS uses a heuristic — in this case, the distance between the user and the restaurant — to guide the search efficiently toward the nearest results first. This approach achieves the intended behavior without altering the output. Here are the revised code snippets:


### BFS Changed into BestFS
```
# BestFS search function to prioritize only distance 
def bestfs(user_location, restaurants, max_distance):
    queue = deque([(user_location, [])])  # Start from user's location
    visited = set()  # To track visited locations
    best_restaurants = []  # List to store the closest restaurants found
    search_times = []  # List to store the search time for each restaurant

    while queue and len(best_restaurants) < 5:
        current_location, path = queue.popleft()

        # Sort restaurants by distance to prioritize the closest ones
        for restaurant in sorted(restaurants, key=lambda r: calculate_distance(current_location, r["location"])):
            start_time = time.time()  # Start timing for this restaurant

            # Skip if the restaurant has already been visited
            if restaurant["location"] in visited:
                continue

            # Calculate the distance from the current location to the restaurant
            distance = calculate_distance(current_location, restaurant["location"])

            # Skip restaurants that are beyond the max distance
            if distance > max_distance:
                continue

            # Mark as visited and add to the best restaurants list
            visited.add(restaurant["location"])
            new_path = path + [restaurant]  # Add the restaurant to the path
            best_restaurants.append(restaurant)

            # Log the time taken to process this restaurant
            end_time = time.time()
            search_times.append((restaurant['name'], end_time - start_time))

            # Stop if we've found enough restaurants
            if len(best_restaurants) >= 5:
                break

            # Add the restaurant to the queue for further exploration
            queue.append((restaurant["location"], new_path))  # BestFS: continue exploring from this restaurant

    return best_restaurants, search_times
```


### Adjustment on BestFS Argument
```
elif priority == "distance":
    start_time = time.time()  
    best_restaurants, search_times = bestfs(user_location, restaurants, max_distance)
    end_time = time.time() 
```
