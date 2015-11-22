# CMPE273-Assignment3
# Building a location and trip planner service in Go
Part II - Trip Planner

## Usage
Build REST endpoints to suggest best possible route for the TRIP.
This application uses MongoDB to store and retrieve data and UBER APIs.
### Install

If the packages ("github.com/julienschmidt/httprouter","gopkg.in/mgo.v2","gopkg.in/mgo.v2/bson") are not installed, go to the code file's folder and do
```
go get 
```
```
go build

```
Before starting the server make sure you give the access_key constant a value.
```
const access_key = "YOUR_UBER_ACCESS_TOKEN"
```
Replace YOUR_UBER_ACCESS_TOKEN with your value.  
The access token can be obtained by following the steps specified at https://developer.uber.com/v1/auth/ 

Start the  server:
```
go run RESTTripPlanner.go
```
The input Location Id's  are:
5: Fairmont Hotel, San Francisco (950 Mason St,San Francisco,CA 94108)  
6: Golden Gate Bridge,California  
7: Pier 39 (Beach Street and The Embarcadero,San Francisco,CA 94133 )  
8: Golden gate Park  
9: Twin Peaks (501 Twin Peaks Blvd,San Francisco,CA 94114)  

### Make POST calls from Client to the URL using cURL
```
curl -X POST -d "{\"starting_from_location_id\":\"5\",\"location_ids\":[\"6\",\"7\",\"8\",\"9\"]}" http://localhost:8082/trips
```
```
Response:
{"id":100,"status":"planning","starting_from_location_id":"5","best_route_location_ids":["7","6","8","9"],"total_uber_costs":70,"total_uber_duration":4069,"total_distance":27.48}
```
### Make GET calls From Client to the URL using cURL
```
curl http://localhost:8082/trips/100
```
```
Response:
{"id":100,"status":"planning","starting_from_location_id":"5","best_route_location_ids":["7","6","8","9"],"total_uber_costs":70,"total_uber_duration":4069,"total_distance":27.48}
```
### Make PUT calls from Client to the URL using cURL
First PUT call
```
curl -X PUT http://localhost:8082/trips/100/request
```
```
Response:
{"id":100,"status":"processing","starting_from_location_id":"5","next_destination_location_id":"7","best_route_location_ids":["7","6","8","9"],"total_uber_costs":70,"total_uber_duration":4069,"total_distance":27.48,"uber_wait_time_eta":2}
```
Another PUT call
```
curl -X PUT http://localhost:8082/trips/100/request
```
```
Response:
{"id":100,"status":"processing","starting_from_location_id":"5","next_destination_location_id":"6","best_route_location_ids":["7","6","8","9"],"total_uber_costs":70,"total_uber_duration":4069,"total_distance":27.48,"uber_wait_time_eta":2}
```
Third PUT call
```
curl -X PUT http://localhost:8082/trips/100/request
```
```
Response:
{"id":100,"status":"processing","starting_from_location_id":"5","next_destination_location_id":"8","best_route_location_ids":["7","6","8","9"],"total_uber_costs":70,"total_uber_duration":4069,"total_distance":27.48,"uber_wait_time_eta":2}
```
Fourth PUT call
```
curl -X PUT http://localhost:8082/trips/100/request
```
```
Response:
{"id":100,"status":"processing","starting_from_location_id":"5","next_destination_location_id":"9","best_route_location_ids":["7","6","8","9"],"total_uber_costs":70,"total_uber_duration":4069,"total_distance":27.48,"uber_wait_time_eta":2}
```
LAST PUT call
```
curl -X PUT http://localhost:8082/trips/100/request
```
```
Response:
{"id":100,"status":"processing","starting_from_location_id":"5","next_destination_location_id":"5","best_route_location_ids":["7","6","8","9"],"total_uber_costs":70,"total_uber_duration":4069,"total_distance":27.48,"uber_wait_time_eta":2}
```

Comments: Run the RESTTripPlanner.go file first using "go run RESTTripPlanner.go" (without double quotes) and in separate command prompt run the cURL command as specified above. The number of PUT calls for a single trip are limited to the number of locations in the request(including the starting location).   
When the next_destination_location_id equals starting_from_location_id, the trip is completed. However, if you place further PUT requests the trip starts again.  

The same can be achieved using postman(application) where you can post raw values to URL and retrieve data
Output images of both have been attached.
