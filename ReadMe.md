                                              Key-Value Data Model Lab
I.	Prerequisites
-	Ensure you have Docker Desktop installed and running on your machine

II.	Install Redis using Docker
a.	Pull Redis Docker Image
                            docker pull redis:7.2
b.	Run this command to start a Redis container:
           docker run --name redis-lab -p 6379:6379 -d redis:7.2
c.	Verify Redis container is running
                               docker ps
d.	Connect to Redis CLI inside the container:
                             docker exec -it redis-lab redis-cli
e.	Test connection by typing
                    PING the response should be PONG
f.	Use a Docker Compose file (docker-compose.yml) for easy management
g.	start Redis with
                          docker-compose up -d
III.	Perform CRUD operations
Sample use case: immigration case record cache
In an immigration system, quick access to case records is critical. Redis can cache each immigration case using unique keys to store applicant data, status, and interview schedules in JSON format for fast retrieval, updates, and deletions.
	Create (Add Immigration Case)
SET case:1001 '{"applicant_name":"KATEMBO JOSEPH","status":"Pending","interview_date":"2026-01-05","nationality":"Congolese"}'
This Creates key case:1001 storing applicant details.
	Read (Retrieve Case Data)
GET case:1001   this returns all stored info of the immigration case.
	Update (Modify Status or Interview Date)
SET case:1001 '{"applicant_name":"KATEMBO JOSEPH","status":"Approved","interview_date":"2026-01-05","nationality":"Congolese"}'
Updates case status from "Pending" to "Approved."
	Delete (Close/Remove Case)
DEL case:1001 this removes the data when case is closed.
	List All Cases (for admin overview)
KEYS case:*
IV.	Practical Immigration Sample Data
SET case:1001 '{"applicant_name":"KATEMBO JOSEPH","status":"Pending","interview_date":"2026-01-05","nationality":"Congolese"}'
SET case:1002 '{"applicant_name":"SHARON BUHLE","status":"Interviewed","interview_date":"2025-11-28","nationality":"SouthAfrican"}'
SET case:1003 '{"applicant_name":"CHIOMA NANCY","status":"Approved","interview_date":"2025-10-25","nationality":"Nigerian"}'
This setup allows immigration officers rapid access to case data for processing and decision making.
V.	Applied Scenario: Immigration Case Status Tracking System
	Realistic problem
Immigration offices handle thousands of daily case status checks by applicants via online portals, requiring sub-millisecond responses to avoid long wait times and system overloads. Traditional relational databases struggle with this high-volume, simple-lookup workload, but key-value stores like Redis excel at caching volatile status data (e.g., "Pending" to "Approved") for millions of concurrent reads/writes.
	Modelling and Solving with Redis Key-Value
Model each immigration case as a key-value pair: unique case ID as key (e.g., case:1001), JSON value holding status, applicant details, and timestamps for instant access.
Data Population from earlier CRUDE
SET case:1001 '{"applicant_name":"KATEMBO JOSEPH","status":"Pending","interview_date":"2026-01-05","nationality":"Congolese"}'
SET case:1002 '{"applicant_name":"SHARON BUHLE","status":"Interviewed","interview_date":"2025-11-28","nationality":"SouthAfrican"}'
Real-Time Queries (fast lookups):
GET case:1001   this returns status instantly for portal
KEYS case:*    this lists active cases for admin dashboard


Status Updates (handle high-velocity changes):
SET case:1001 '{"applicant":"KATEMBO JOSEPH","status":"Interview Scheduled","nationality":"Congolese","last_update":"2026-01-07"}'
Expiration for Cleanup (auto-delete closed cases):
This solves the problem by enabling 100k+ queries/sec with <1ms latency, reducing server load ideal for session-like immigration tracking where complex joins aren't needed.
SETEX case:1001 3600 '{"status":"Closed"}'  
The command name (Set with Expiry), the key name (unique identifier for the immigration case), Expiration time in seconds (3600 seconds), the JSON string value stored under the key  .




Test