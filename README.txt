Test Cases:
1. Veriry launched application is waiting for connections – Purpose of the test is to verify that the application is in a waiting state and can accept connections.
	a. Launch from Command prompt and see that the processes is running in 
	Task Manager.
	b. Run the following command to see that command execution was logged:
	-curl -X POST -H "application/json" -d '{"password":"angrymonkey"}' 
	http://127.0.0.1:8088/hash
		Results: Pass - Log return 'Malformed Input"
2. Check that the command runs only on configured port – Purpose is to verify that the application can connect to the specified port.
	a. Run with correct port 'curl http://127.0.0.1:8088/stats'
	b. Run with incorrect port 'curl http://127.0.0.1:8088/stats'
		Results: Case a Passed, Case b failed as expected
3. Verify the following endpoints, POST to /hash, GET to /hash, GET to /stats – Purpose is to verify that the described endpoints are accepted by the application.
	a. Run: curl -X POST -H "application/json" -d 
	'{"password":"angrymonkey"}' http://127.0.0.1:8088/hash
	b. Run: curl -H "application/json" http://127.0.0.1:8088/hash/1
	c. Run: curl http://127.0.0.1:8088/stats
		Results: All endpoint commands accepted
4. Application supports multiple connections simultaneously – Purpose of the test is to validate that the application can accept multiple connections simultaneously. 
	a. Create 2 batch files that executes the following command 10 times. 
	Command used: -curl -X POST -H "application/json" -d 
	'{"password":"angrymonkey"}' http://127.0.0.1:8088/hash
	b. Will application running accepting connections, execute both batch 
	files.
	c. Monitor the output from the application
		Results: Application accepted multiple connections
5. Graceful shutdown – Purpose of the test is to ensure that the application does a graceful shutdown.  That it will process pending commands and that no other commands are executed after shutdown
	a. Open 4 command prompts and navigate to the batch file.
	b. Start application so that it is waiting for connections
	c. Launch the batch file in 2 command prompts
	d. When the batch file starts to execute, run the shutdown command 
	from the 3rd command prompt
	e. In the final command prompt start the batch file.  This should be 
	run immediately after the shutdown command is executed
		Results: Application log reports: 
			Shutdown signal received
			1 Request was accepted
			Then log reported 'received requests after shutdown
			The application still accepts request and processes 
			them
6. No additional password requests should be allowed when shutdown is pending – Purpose is to ensure that no password requests are being accepted and processed if the shutdown is pending. 
	a. Run a batch file that calls out -curl -X POST -H "application/json" 
	-d '{"password":"angrymonkey"}' http://127.0.0.1:8088/hash several 
	times
	b. While the batch file is running, run the shutdown command
		Results: Password requests are still processed