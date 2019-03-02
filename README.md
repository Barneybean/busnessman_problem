# Travelman Problem
Fly through all 50 U.S. state capitals without revisiting and back to origin with smallest distance

Calculated Geo Distance between any two state capitals. Working on next step

## Min Distance: 18993.80 Miles 

## Route:
** ['Phoenix', 'Santa Fe', 'Denver', 'Cheyenne', 'Pierre', 'Bismarck', 'St. Paul', 'Madison', 'Springfield', 'Jefferson City', 'Topeka', 'Lincoln', 'Des Monies', 'Indianapolis', 'Frankfort', 'Columbus', 'Charleston', 'Richmond', 'Annapolis', 'Dover', 'Trenton', 'Harrisburg', 'Albany', 'Hartford', 'Providence', 'Boston', 'Concord', 'Montpelier', 'Augusta', 'Lansing', 'Nashville', 'Atlanta', 'Montgomery', 'Tallahassee', 'Columbia', 'Raleigh', 'Jackson', 'Baton Rouge', 'Little Rock', 'Oklahoma City', 'Austin', 'Salt Lake City', 'Boise', 'Helena', 'Olympia', 'Salem', 'Carson City', 'Sacramento', 'Juneau', 'Honolulu', 'Phoenix'] **

 ![Screenshot](/Mapping/travelman1.png)

## main algorithm:
``` result_distances = []
result_routes = []

for start_city in capital_cities: 
    route = [start_city] #to acommandate firt city
    total_distance = 0

    def optimize_distance(city):
        nth = 0
        global total_distance
        min_distance = distance_unique[distance_unique["origin_city"] == city]["distance_mile"].min()
        min_distance_city = distance_unique[distance_unique["distance_mile"] == min_distance]["destination_city"].values[0]
        while min_distance_city in route: 
            #take nth smallest distance to avoid duplicate route
            nth += 1 
            min_distance_row = distance_unique[distance_unique["origin_city"] == city].nsmallest(nth, 'distance_mile', keep='last').iloc[nth-1,]
            min_distance = min_distance_row["distance_mile"]
            min_distance_city = min_distance_row["destination_city"]
        
        route.append(min_distance_city)
        total_distance += min_distance    

    for i in range(0,49): 
        optimize_distance(route[-1])
    
    #add Sccramento as final destination
    route.append(route[0])

    #adding distance between Holonunu to Sacramento 
    distance_end_origin = distance_unique[(distance_unique["origin_city"] == start_city) & (distance_unique["destination_city"] == route[-2])]["distance_mile"].values[0]
    total_distance = total_distance + distance_end_origin
    
    result_distances.append(total_distance)
    result_routes.append(route)          