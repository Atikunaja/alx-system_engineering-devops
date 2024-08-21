PostMortem is a Post incident report that details server downtime/bugs, root cause analysis and details the steps taken to resolve as well as prevent future repeat events from same causality.

This is a simple bug that I had encountered in the previous AirBnB clone API project, from 13:25 to 15:02 GMT, part of the requests to the endpoint /api/v1/places_search were resulting in wrong data responses. During this time, sending POST requests to this endpoint with amenity_ids in the request body resulted in unexpected, wrong JSON data responses. Users querying this endpoint were receiving wrong data. The root cause was in a for loop in the Flask view function handling this endpoint, which was causing the list of returned places to be incorrect.

Timeline (all times GMT)

13:25: added and saved the endpoint view function to places.py file

13:26: started testing the endpoint on the development server

13:28: noticed that some responses contained wrong data

13:30: began tracing the cause of the bug

13:45: suspected that queries to the storage system are incorrect

14:05: queries to the storage system were correct

14:10: investigated endpoint’s view logic

14:42: root cause identified as wrong structure in one of the view’s for loops

14:57: corrected the for loop logic

1502: issue resolved and endpoint returning correct data responses

Root cause and resolution

After adding the view function for /api/v1/places_search, querying this endpoint with amenity_ids list in the request body resulted in entering a for loop to filter the places queried from the storage system based on amenities. The loop was iterating through the list of places, and based on certain criteria, it deletes items from the same list, which makes it skip over some items in the places list, resulting in wrong places to be returned in the response.
The solution was to replace the places to be removed with a None value inside the loop, then returning a list of items that are not equal to None.

Corrective and preventative measures

To prevent similar bugs from occurring in the future, will be implementing the following measures:

Separating and limiting tasks handed to each function to easily report the root cause of an issue

Write smaller and readable functions to help understand the flow of logic inside it

Avoid appending or deleting from an object (list, dictionary etc.) that you’re iterating over in a loop

Test each new component of a feature to identify issues early

Use debugging and testing tools specific to the programming language used

To summarize, a bug causing wrong data responses in an endpoint was caused by a for loop that deleted items from the places list. The bug was fixed by replacing the deleted places with a None value. To prevent similar issues, measures such as separating tasks, writing smaller functions, and testing each new component should be implemented. This serves as a valuable learning experience for the future.

