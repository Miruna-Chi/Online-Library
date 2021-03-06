# Online Library - Communication via REST API

### client.c

- server_conn_send:
	- open the connection with the server
	- send the message
	- close the connection
	- keep the answer

- main:
	- we initialize data (ip, host), we open the connection with the server

~ while (1):
- exit -> close the connection, socket, exit
- register
	-> check if the order is valid
	-> check if we are already logged in (we have a cookie)
		- if yes: error -> displays the error
		- if not: prompt for data entry,
		we create the message, we send it to the server
		(see "auth" in requests)
		-> if successful (ret == 0), display this
		-> not successful (show error)
- log in
	-> procedure almost identical to register (url and messages
	error differs)
- logout
	-> check if the order is valid
	-> check if the user is already logged in (they have a cookie)
	-> cookie = 0 (the user is no longer logged in) -> all operations
	 that require logging in will no longer be possible
	-> have_jwt = 0 -> the user no longer has access to the library
	because they are no longer logged in
	-> create the message, send it to the server
- enter_library
	-> check if the order is valid
	-> check if the user is logged in
	-> if none of the above branches were chosen:
	we create the message, send it to the server, set the variable
	"Have_jwt"
- get_books
	-> check if the order is valid
	-> check if the user has access to the library
	-> if none of the above branches were chosen, create
	the message, send it to the server
	-> if the response from the server did not come with an error:
	Parse the books in the library, display them
- get_book
	-> check if the order is valid
	-> check if the user has access to the library
	-> if none of the above branches were chosen, we display a
	prompt for entering the id, create the message, send it to
	the server
	-> if the response from the server did not come with an error:
	parse the details of the book, display them
- delete_book
	-> check if the order is valid
	-> check if we have access to the library
	-> if none of the above branches were chosen, we display a
	prompt for entering the id, create the message, send it to
	the server
	-> we display an error or success message
- else: invalid command

### requests.c

- compute delete request:
-> basically post, but no content

I chose to use the "parson" library primarily because I have
wrote the program in C.

- create_auth_payload
	-> receives username and password and returns a string in json format
	(the one in the statement)
	-> using the functions in the "parson" library we create a json object with
	the required fields, serialize the data and create the corresponding string

- print_json_error
	-> we take the data from {}, we parse the error with the help of the functions from
	the parson library, display the string.

- parse_json_book
	-> we take the data from {} -> json object, we parse the fields with the help of
	functions in the parson library), we display the book data.

- parse_json_books
	-> we take the data from [] -> we will have an array of json objects ->
	take each one and print the id and title fields
	-> if there are no books in the library, we display a nice message

- handle_server_response
	-> we make a token with the server response code
	-> if there is an error, parse, print, return 1
	-> if not, return 0

- return_cookie
	-> pretty self explanatory, this one

- auth
	-> used for register and login
	-> create prompt for username and password
	-> compute_post_request with
	data (except url, identical to login and register)

- make_book
	-> used to create the message containing the json string with the added book
	-> displays prompts for each field of a book
	-> serializes the data and builds a json object that it
	converts to a string (all through the parson library)
	-> creates the message with the data (compute_post_request), returns it
