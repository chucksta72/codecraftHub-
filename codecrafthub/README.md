CodeCraftHub - Beginner Flask REST API with JSON file storage
CodeCraftHub is a tiny, beginner-friendly learning project to build a simple REST API using Python and Flask. It stores course data in a JSON file (no database) and provides basic CRUD operations so developers can learn how REST endpoints work, how to validate input, and how to read/write data from disk.

What you’ll learn

How to create a Flask-based REST API
How to store data in a JSON file (courses.json)
How to implement CRUD operations for a resource (courses)
How to perform basic input validation (required fields, date format, valid status)
How to handle common errors gracefully and return helpful messages
Project overview

Name: CodeCraftHub
Purpose: A simple learning platform where developers can track courses they want to learn.
Storage: JSON file named courses.json
Endpoints: CRUD operations under /api/courses
No authentication or user management (intentional for learning REST basics)
Features

Create a new course (POST /api/courses)
Retrieve all courses (GET /api/courses)
Retrieve a single course by ID (GET /api/courses/{id}) or via query (?id=)
Update a course completely (PUT /api/courses)
Delete a course (DELETE /api/courses)
Auto-generated id (starting from 1)
Auto-generated created_at timestamp
Input validation for required fields, date format, and status values
Auto-creation of the data file if it doesn’t exist
Basic error handling with clear messages
Prerequisites

Python 3.x (tested with Python 3.8+)
pip (Python package manager)
Installation instructions (step-by-step)

Create a project directory and navigate into it
mkdir CodeCraftHub
cd CodeCraftHub
(Optional) Create a virtual environment
python -m venv venv
On Windows: venv\Scripts\activate
On macOS/Linux: source venv/bin/activate
Install Flask (or install from requirements.txt if you have one)
pip install Flask
Optional: pip install -r requirements.txt (if you create one with Flask listed)
Ensure the application files are in place
You should have app.py (the Flask app) and a data folder (optional; the app will create courses.json automatically).
Run the application
python app.py
Open http://localhost:5000 to see the API in action (the endpoints are under /api/courses).
API endpoints documentation (with examples) Base URL (for all examples): http://localhost:5000

Create a new course
Endpoint: POST /api/courses
Description: Add a new course
Required fields in JSON body: name, description, target_date (YYYY-MM-DD), status
Valid status values: "Not Started", "In Progress", "Completed"
Request (example payload): { "name": "Intro to Python", "description": "Learn Python basics", "target_date": "2026-08-01", "status": "Not Started" }
Command (copy-paste): Unix/macOS: curl -s -X POST http://localhost:5000/api/courses
-H "Content-Type: application/json"
-d '{"name":"Intro to Python","description":"Learn Python basics","target_date":"2026-08-01","status":"Not Started"}' Windows (PowerShell): curl -Method POST http://localhost:5000/api/courses -Headers @{"Content-Type"="application/json"} -Body '{"name":"Intro to Python","description":"Learn Python basics","target_date":"2026-08-01","status":"Not Started"}'
Expected successful response (example): Status: 201 Created Body: { "id": 1, "name": "Intro to Python", "description": "Learn Python basics", "target_date": "2026-08-01", "status": "Not Started", "created_at": "2026-07-14T12:34:56.789Z" }
Get all courses
Endpoint: GET /api/courses
Description: Retrieve all courses
Request: curl http://localhost:5000/api/courses
Command: Unix/macOS: curl -s http://localhost:5000/api/courses Windows (PowerShell): curl -Uri http://localhost:5000/api/courses
Expected successful response: Status: 200 OK Body: A JSON array of course objects, e.g. [ { "id": 1, "name": "Intro to Python", "description": "Learn Python basics", "target_date": "2026-08-01", "status": "Not Started", "created_at": "2026-07-14T12:34:56.789Z" } ]
Get a specific course by ID
Endpoint: GET /api/courses/{id}
Description: Retrieve one course by its numeric ID
Request: Unix/macOS: curl -s http://localhost:5000/api/courses/1 Windows (PowerShell): curl -Uri http://localhost:5000/api/courses/1
Expected successful response (example for id=1): Status: 200 OK Body: { "id": 1, "name": "Intro to Python", "description": "Learn Python basics", "target_date": "2026-08-01", "status": "Not Started", "created_at": "2026-07-14T12:34:56.789Z" }
Error example (not found): curl -s http://localhost:5000/api/courses/999 Response: { "error": "Course not found." }
Update a course completely
Endpoint: PUT /api/courses
Description: Full update of a course (requires id and all fields)
Required fields in JSON body: id, name, description, target_date, status
Valid status values: "Not Started", "In Progress", "Completed"
Request (example payload): { "id": 1, "name": "Intro to Python - Updated", "description": "Learn Python basics and syntax", "target_date": "2026-08-10", "status": "In Progress" }
Command: Unix/macOS: curl -s -X PUT http://localhost:5000/api/courses
-H "Content-Type: application/json"
-d '{"id":1,"name":"Intro to Python - Updated","description":"Learn Python basics and syntax","target_date":"2026-08-10","status":"In Progress"}' Windows (PowerShell): curl -Method PUT http://localhost:5000/api/courses -Headers @{"Content-Type"="application/json"} -Body '{"id":1,"name":"Intro to Python - Updated","description":"Learn Python basics and syntax","target_date":"2026-08-10","status":"In Progress"}'
Expected successful response: Status: 200 OK Body: { "id": 1, "name": "Intro to Python - Updated", "description": "Learn Python basics and syntax", "target_date": "2026-08-10", "status": "In Progress", "created_at": "same as before" }
Error case: updating a non-existent course curl -s -X PUT http://localhost:5000/api/courses -H "Content-Type: application/json" -d '{"id":999,"name":"X","description":"Y","target_date":"2026-08-01","status":"Not Started"}' Response: { "error": "Course not found." }
Error case: missing fields curl -s -X PUT http://localhost:5000/api/courses -H "Content-Type: application/json" -d '{"id":1,"name":"X"}' Response: { "error": "Missing fields for update: description, target_date, status" }
Delete a course
Endpoint: DELETE /api/courses
Description: Delete a course by id
Required fields in JSON body: id
Request: Unix/macOS: curl -s -X DELETE http://localhost:5000/api/courses
-H "Content-Type: application/json"
-d '{"id":1}' Windows (PowerShell): curl -Method DELETE http://localhost:5000/api/courses -Headers @{"Content-Type"="application/json"} -Body '{"id":1}'
Expected successful response: Status: 204 No Content Body: empty
Error case: delete non-existent course curl -s -X DELETE http://localhost:5000/api/courses -H "Content-Type: application/json" -d '{"id":999}' Response: { "error": "Course not found." }
Error case: missing id curl -s -X DELETE http://localhost:5000/api/courses -H "Content-Type"="application/json" -d '{}' Response: { "error": "Missing field: id" }
Error scenarios: invalid parameter formats
Invalid id query parameter on list (GET /api/courses?id=abc) curl -s "http://localhost:5000/api/courses?id=abc" Response: { "error": "Invalid id parameter. Must be an integer." }
Get by id that doesn’t exist using direct path curl -s http://localhost:5000/api/courses/999 Response: { "error": "Course not found." }
Testing file I/O error handling (optional)
These steps are for advanced testing, and involve changing file permissions to simulate read/write errors.
Example: simulate write error
Before: create a course first to have an existing file
chmod u-w courses.json
Attempt to POST a new course and observe a 500 error: {"error":"Failed to write data file."}
Restore permissions: chmod u+w courses.json
Example: simulate read error
chmod a-r courses.json
Attempt to GET /api/courses
Observe a 500 error: {"error":"Failed to read data file."}
Restore permissions: chmod a+r courses.json
Notes on error messages

Missing required fields: "Missing required fields: <field1>, <field2>, ..."
Invalid status: "Invalid status. Must be one of: Not Started, In Progress, Completed."
Invalid target_date: "Invalid target_date format. Use YYYY-MM-DD."
Course not found: "Course not found."
Missing field for update: "Missing fields for update: <field1>, <field2>, ..."
Missing field: id (for delete/update): "Missing field: id"
File I/O errors: "Failed to read data file." or "Failed to write data file."
Project structure explanation

app.py

The Flask application that defines all API routes under /api/courses
Reads/writes data from courses.json using helper functions
Performs input validation and error handling
Auto-generates IDs and created_at timestamps
data/

Optional directory to store the JSON file
The code will create courses.json automatically if it doesn’t exist
courses.json

The JSON file that stores the list of course objects
Each course is a JSON object with fields: id, name, description, target_date, status, created_at
requirements.txt (optional)

Example contents: Flask>=2.0
README.md

This document you’re reading now
Example initial data (optional)

You can start with an empty list: []
Or with a sample course: [ { "id": 1, "name": "Intro to Python", "description": "Learn Python basics", "target_date": "2026-08-01", "status": "Not Started", "created_at": "2026-07-14T12:34:56.789Z" } ]
If you’d like, I can tailor this README to a specific folder layout (Option A single-file app vs a small package structure) or add a quick-start script to automate setup and testing. Happy to help you extend it as you continue learning REST APIs.
