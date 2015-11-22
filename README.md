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
### Make POST calls from Client to the URL using cURL
```
curl -X POST -d "{\"starting_from_location_id\":\"2\",\"location_ids\":[\"1\",\"3\",\"4\"]}" http://localhost:8082/trips
```
```
Response:
{"id":1700,"status":"planning","starting_from_location_id":"2","best_route_location_ids":["4","3","1"],"total_uber_costs":88,"total_uber_duration":4974,"total_distance":64.77000000000001}
```
### Make GET calls From Client to the URL using cURL
```
curl -X GET http://localhost:8082/trips/1700
```
```
Response:
{"id":1700,"status":"planning","starting_from_location_id":"2","best_route_location_ids":["4","3","1"],"total_uber_costs":88,"total_uber_duration":4974,"total_distance":64.77000000000001}
```
### Make PUT calls from Client to the URL using cURL
First PUT call
```
curl -X PUT http://localhost:8082/trips/1700/request
```
```
Response:
{"id":1700,"status":"processing","starting_from_location_id":"2","next_destination_location_id":"4","best_route_location_ids":["4","3","1"],"total_uber_costs":88,"total_uber_duration":4974,"total_distance":64.77000000000001,"uber_wait_time_eta":9}
```
Another PUT call
```
curl -X PUT http://localhost:8082/trips/1700/request
```
```
Response:
{"id":1700,"status":"processing","starting_from_location_id":"2","next_destination_location_id":"3","best_route_location_ids":["4","3","1"],"total_uber_costs":88,"total_uber_duration":4974,"total_distance":64.77000000000001,"uber_wait_time_eta":9}
```
Third PUT call
```
curl -X PUT http://localhost:8082/trips/1700/request
```
```
Response:
{"id":1700,"status":"processing","starting_from_location_id":"2","next_destination_location_id":"1","best_route_location_ids":["4","3","1"],"total_uber_costs":88,"total_uber_duration":4974,"total_distance":64.77000000000001,"uber_wait_time_eta":9}
```
Last PUT call
```
curl -X PUT http://localhost:8082/trips/1700/request
```
```
Response:
{"Message":"You have reached your destination."}
```
Comments: Run the RESTTripPlanner.go file first using "go run RESTTripPlanner.go" (without double quotes) and in separate command prompt run the cURL command as specified above. The number of PUT calls are limited to the number of locations in the request(including the starting location). When the last location in 'best_route_location_ids' is reached and you place an another PUT call, you get the message "You have reached your destination."

The same can be achieved using postman(application) where you can post raw values to URL and retrieve data

